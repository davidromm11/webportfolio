---
layout: post
title: 'Building DownTownOS: A Custom Operating System from Scratch'
date: 2025-11-09
---

**Note:** Find the full code repository at [https://github.com/Timmy387/DownTownOS](https://github.com/Timmy387/DownTownOS)

An operating system is software that acts as a bridge between computer hardware and applications. Without it, using a personal computer would require a user knowing how to communicate with hardware in machine code (basically impossible). So a classmate and I decided to build a custom OS from scratch for the 8088 PC, which we named DownTownOS.

This was a test of creativity and conceptual knowledge, but most of all, a very rewarding project. We did most of the work in the last couple weeks of our senior year at college, getting together to work on this as guilt-free procrastination from studying for finals. We hadn't actually assembled the 8088 hardware to run this OS (we were missing a floppy disk controller), so we used the Bochs emulator configured to the same specs to see how it would theoretically perform.

## The Hardware Interface Layer

The 8088 processor doesn't understand C, which is what we decided to write most of DownTownOS in. It doesn't understand file systems or memory allocation. All it truly understands is very low-level machine code moving data in and out of registers and performing basic operations, exactly what assembly is for. So the very first code we wrote was a collection of assembly functions that would become the bridge between our higher-level C code and the hardware.

These functions live in `asmUtils.asm` and `kernel.asm`. They're critical because they use BIOS interrupts to talk to hardware in the early boot stages, since the OS hasn't loaded yet. Interrupts are firmware built into early CPUs that provide building blocks for basic functions. Without them, we'd have to write a ton more code.

#### Reading the Keyboard

Consider the simple act of reading a key from the keyboard. On modern systems, you might call `getchar()` or read from stdin. But when you're writing your own OS, there is no stdin.

We wrote an assembly function that uses BIOS interrupt `INT 16h` to check if a keystroke is waiting, and if so, grabs it and returns it in the AX register. Every character the user types goes through this code. The BIOS interrupt is hard-coded into the CPU, which saves us from writing keyboard controller drivers from scratch.

#### Writing to the Screen

Similarly, displaying text requires direct manipulation of video memory. VGA text mode maps a section of memory at address `0xB800:0000`. Each character on screen is represented by two bytes, one for the ASCII character, one for the color.

Our `putInMemory` function takes a segment, offset, and character, then writes directly to video memory. This function gets called thousands of times per second. When you type a command and see characters appear on screen, each one is individually written to video memory at the calculated position.

#### The Cursor

The VGA controller has registers that control where the cursor appears. Moving the cursor requires sending commands to the video BIOS using `INT 10h`. Our function takes row and column parameters and tells the BIOS where to put that blinking cursor.

#### Disk I/O

Reading from and writing to disk is how the file system works. These assembly functions use `INT 13h` (BIOS disk services), addressing using cylinder, head, and sector. Every file read and write goes through this. When you save a file in the text editor, when the system reads file system metadata, when the bootloader loads the kernel, it's just 15 lines of assembly that handle all these operations.

## Memory

Now that we can talk to hardware, we need somewhere to store the data.

The 8088 gives you 64KB of addressable memory per segment. We allocated the top 32KB of our data segment (`0x8000` to `0xFFFF`) as a heap. But having memory isn't enough, we also need a way to manage it. This is where `malloc()` and `free()` come in.

#### Memory Allocation

We didn't want to deal with dynamic allocations where every request could be a different size, it seemed too complex for a simple OS (though it would save memory space). Instead, we built a simple linked-list allocator using fixed metadata blocks. Each chunk of allocated memory has a small header that tracks its size, whether it's free or in use, and where the next block is.

This is essentially a bitmask approach at a block level, a "blockmask." The entire heap is just a linked list of these blocks where we can quickly scan for free space, mark blocks as used, and rejoin adjacent free blocks when memory is freed.

#### Initialization

When the system boots, our `initHeap()` function sets up two special blocks: a head block at the start (32KB of free space) and a tail block at the end (marks the end). This tells our system the valid address range for the heap.

When you call `malloc(1024)`, here's what happens: search for space by traversing the linked list looking for consecutive free blocks totaling at least 1024 bytes plus header size, coalesce any adjacent free blocks into one large block, split the block if we find more space than needed, and return a pointer to the memory address.

#### Freeing Memory

When you call `free(ptr)`, the system marks the block as free, immediately coalesces it with any adjacent free blocks, and updates the linked list. This prevents fragmentation. If you allocate and free 100 tiny blocks, they'll merge back into one large free block automatically.

#### The Memory Crisis Handler

If malloc fails because the system runs out of memory, it can't continue operating. So we implemented a fairly harsh error handler that clears the screen, displays "Memory Error" in the center, and shows a message telling you to press ESC to reboot. When memory runs out, the only option is to reboot. That's the extent of our memory error handling.

## The Screen

Now we have hardware access and memory management. The next challenge was building a usable display system.

We decided to build our own buffered screen system. Writing to video memory is slow in the computer world, so instead of modifying video memory directly for every character, we keep a buffer representation of the entire screen in memory.

We have a `rowData` structure that holds 80 characters, tracks where to write the next character, and stores the color for that row. The screen is 25 rows by 80 columns (2000 characters total). The system keeps this entire buffer in RAM and only writes to video memory when `printScreen()` is called.

#### Color

Each character has a color byte where the first 4 bits represent foreground color and the last 4 bits represent background color. Colors can be changed with the `color` command, and the configuration is saved to disk so your preferences persist across reboots.

#### The Scrolling Problem

When text reaches the bottom of the screen, you need to scroll. But scrolling is also a memory management problem. Our `scroll()` function frees the top row (it's scrolling off screen), shifts all rows up (row 1 becomes row 0, row 2 becomes row 1, etc.), creates a fresh bottom row by allocating new empty memory, and adjusts the cursor position.

This is called every time text would go past row 24. It's very efficient since we aren't copying data anywhere, we're just changing pointers.

#### Print

The `print()` function is more complex than it seems. It must handle backspace (delete previous character), handle newlines (move to next row, scroll if needed), handle word wrapping (automatically break at 80 characters), and update both the screen buffer and video memory. The logic handles all the edge cases like being at the start of a line when you hit backspace, or being at the bottom of the screen when you hit enter.

## Strings

A string in C is just a char pointer with no length information. You can easily write past the end (buffer overflow) or forget to null-terminate, so we built a safer abstraction. A String struct that always knows its length and has dynamically allocated memory.

#### String Operations

Creating a string allocates memory and copies the data. Adding a character reallocates to grow the string. Every keystroke in the command prompt calls this function. Type "hello" and it's called 5 times, each time reallocating slightly more memory.

#### Parsing Commands

The most complex string operation is `split()`, which tokenizes a command line into arguments while respecting quoted strings. For example, the input `mkdir "my project"` becomes `["mkdir", "my project"]` (not `["mkdir", "my", "project"]`).

The algorithm counts delimiters (spaces) to allocate the array, tracks whether we're inside quotes, builds each token character by character, and skips over the quotes themselves. This is what allows commands like `color "light blue"` to work correctly.

## The File System

A file system is a massive organizational challenge. You have a floppy disk with 1,440 sectors of 512 bytes each (720KB total). We need to organize it so users can create files and directories, navigate a tree structure, read and write data, and keep track of what's used and what's free.

#### The Physical Layout

We divided the disk like this: Sector 0 is the bootloader (512 bytes), Sector 1 is the bitmask (tracks which blocks are used), Sector 2 is the root directory inode, Sector 3 is the configuration file, Sectors 4-5 are reserved for the kernel, and Sectors 6+ are file and directory data (1024 blocks available). Track 0-6 are reserved for the OS, and Track 7 onward belongs to the file system.

#### The Inode Structure

Every file and directory is represented by an inode (index node) that stores which block on disk it lives at, its parent directory's block number, whether it's a directory or file, its size in blocks, whether it's been saved, its filename (max 32 chars), and an array of block numbers for its data.

The inode is exactly 512 bytes so it fits perfectly in one disk sector. The filename is stored immediately after the struct, and the direct block pointers follow that meaning that each inode can directly reference up to 230 data blocks (but the current implementation only uses 4, limiting files to 2KB).

#### Block Number to Physical Address

The file system needs to convert a block number (like block 47) into physical disk coordinates: head, track, sector. There's some straightforward math that calculates the track by dividing by sectors per track and adding the file start track offset, then figures out the head and sector from the remainder.

#### The Bitmask

The bitmask is a 512-byte array where each bit represents one block, 0 means free, 1 means used. The first few bits are always 1 (bootloader, bitmask itself, root directory). With 512 bytes × 8 bits = 4096 bits, but only 1024 blocks are used (limited by disk geometry).

Finding free space just means scanning through the bitmask until we find a 0 bit. Marking a block as used involves some bit manipulation to set the correct bit to 1, then writing the bitmask back to disk.

#### Creating a File

When you type `touch myfile.txt`, here's what happens: check if the file exists in the current directory, find a free block by scanning the bitmask, build an inode with the free block number and filename, write the inode to disk, update the parent directory to include this new file's block number, and write the parent directory back to disk. It's a lot of disk I/O for creating an empty file.

#### Reading a File

When you open `myfile.txt` in the text editor, the system finds the file in the current directory, reads its inode from disk, reads each data block listed in the inode's direct pointers, concatenates all blocks into one continuous character array, and returns the contents. If the file doesn't exist, it returns 0.

#### Directory Navigation

The current directory is stored as a static variable so it persists across function calls. When you `cd projects/`, the system searches the current directory for an inode named "projects/", then overwrites the static current directory pointer with the new one.

The `pwd` command works by following parent pointers backward to root, building up the path as it goes.

## The Shell

With memory, screen, strings, and files working, we could finally build what users actually interact with, the command shell.

#### The Command Loop

The shell is an infinite loop that displays a prompt (`$`), reads keystrokes one at a time, builds up a command string, and when Enter is pressed, parses and executes the command. It also has shortcuts like Ctrl+L to clear the screen and Ctrl+R to reboot.

#### Command Parsing and Dispatch

When you press Enter, the command string is tokenized and dispatched. We split the input on spaces, grab the first token as the command name, then compare it against all known commands (ls, cd, mkdir, touch, rm, color, tw, etc.). If it matches, we call the corresponding function. If not, we display an error.

#### Help System

Every command supports a `-h` flag that prints usage information. Type `help` and you get a list of all available commands with a note to use the `-h` flag to learn more about each one.

## The Text Editor

One of my personal favorite functions of DownTownOS is TypeWriter, a full-screen text editor that runs entirely within the OS. It's like a miniature version of vim or nano.

#### Design Challenge

Building a text editor requires displaying the file content, showing header and footer, handling cursor movement, handling editing, tracking modifications (show "modified" indicator), saving to disk, and preserving shell state.

#### File Structure

An open file is represented by a struct that holds the filename, the actual file content as an array of rows, header rows, footer rows, the number of lines in the file, and which row is at the top of the screen (for scrolling).

#### Opening a File

When you type `tw myfile.txt`, the system reads the file contents from disk, splits into rows at newline characters, creates a header row with the centered filename and space for "modified" indicator, creates footer rows with keyboard shortcuts, saves the current screen state so it can be restored, clears the screen, and displays the file.

#### Editor Screen Layout

The screen shows 1 header row (colored differently, shows filename and save status), 22 content rows (the actual editable text), and 2 footer rows (keyboard shortcuts in different color). This layout makes efficient use of all of the 25-row display.

#### Editing Loop

Once the file is displayed, TypeWriter enters its own event loop. It waits for keystrokes and handles ESC or Ctrl+X to exit, Ctrl+S to save, arrow keys to move the cursor, and regular characters to insert text. There's a saved flag that tracks whether the file has been modified.

#### Character Insertion

When you type a regular character, the system has to shift existing characters right to make room, handle line wrapping if the line exceeds 80 characters, and update the cursor position. It also handles special keys like backspace, tab, and enter.

#### Enter Key

Pressing Enter is complex because it needs to create a new row in the rows array, split the current line at the cursor position, reallocate the entire rows array, move remaining lines down, and position the cursor at the start of the new line. I like how responsive TypeWriter feels because it's doing a lot of work behind the scenes but efficiently.

#### Backspace

Backspace is the inverse operation. If you're at the start of a line, it must merge with the previous line, delete the current row, and reallocate the rows array. Otherwise, it just deletes the previous character.

#### Saving the File

When you press Ctrl+S, the system concatenates all rows with newlines between them, calculates the total size in blocks, frees the old data blocks (clears bits in the bitmask), allocates new blocks and writes the data, and updates the inode with the new block pointers. It's a complete rewrite of the file every time.

#### Exiting the Editor

When you press ESC or Ctrl+X, TypeWriter frees the entire file structure (all rows, headers, footers), restores the saved shell screen, and returns to the command loop. When you return to the shell, your command history, cursor position, and screen content are exactly as you left them.

## Configuration and Persistence

We have a config struct that tracks whether the system has been initialized, whether default directories have been created, the terminal color scheme, the TypeWriter title bar color, and the TypeWriter footer color. This entire structure fits in one 512-byte block (sector 3), right after the bootloader and before the file system metadata.

#### First Boot

When the system boots for the very first time, `initFileSystem()` checks if the config has been initialized. If not, it builds a fresh config, initializes the bitmask marking the first few blocks as used, creates the root directory inode, and writes everything to disk.

#### User Home Directories

On first boot, the system also creates a default directory structure: a `users/` directory with subdirectories for `timmy/` and `dave/`. In the OS's current state you start in timmy/'s home. This is only done once. On subsequent boots, it just navigates back to your home directory.

#### Color Persistence

When you type `color yellow` or `color -bg blue`, the system updates the config in memory, calls `saveConfig()` to write to disk, and updates all existing screen rows to use the new color. Next time you boot, your color scheme is preserved.

## The Build System

Now let's talk about how all this code actually becomes a bootable floppy disk image.

#### Build Pipeline

The build goes like this: C source files get compiled by BCC (Bruce's C Compiler for 8086) into object files, assembly sources get assembled by NASM into object files, all object files get linked together by ld86 into a kernel binary, and finally dd (disk dump) writes everything to the disk image.

#### Step by Step

First, we create an empty 720KB floppy disk image filled with zeros. Then we compile the bootloader with NASM into a raw 512-byte binary and write it to sector 0. Next, we compile all the C sources with BCC and assemble all the assembly sources. The linker then resolves all function calls between C and assembly. Finally, we write the kernel to the disk starting at sector 3.

#### The Development Environment

The project includes a Docker container so you can build it anywhere. It installs ancient tools like BCC, NASM, and ld86 that you'd never have on a modern system. There's also a VS Code dev container config so you can open the project and it automatically builds everything you need.

## Boot Sequence

#### Power On

When you flip the power switch on an IBM PC, the CPU starts at a hardcoded address, the BIOS ROM executes and runs POST (Power-On Self Test), it checks RAM and keyboard and other hardware, then it looks for a bootable device.

#### Loading the Bootloader

The BIOS reads sector 0 of the floppy disk into memory at address `0x7C00` and checks if the last two bytes are `0xAA55` (the boot signature). If present, the BIOS jumps to `0x7C00`, transferring control to our bootloader.

#### Bootloader Execution

Now our code is running for the first time. The bootloader sets up memory segments (code, data, stack) in the right places in memory. This establishes the memory layout the OS will use.

#### Loading the Kernel

The bootloader must read the kernel from disk into memory. But BIOS can only read 9 sectors at a time on many systems. The kernel is bigger than that, so the bootloader reads in multiple stages, carefully alternating between disk heads and tracks, reading 9 sectors at a time until the entire kernel is loaded. This loads up to ~30KB of kernel code. Once it's all in memory, the bootloader jumps to the kernel's entry point.

#### Kernel Initialization

The kernel starts by initializing video, initializing the heap, initializing the file system, navigating to the user home directory, clearing the screen, printing a welcome message, and starting the shell.

Each initialization step builds on the previous ones. Video is needed to display anything, heap is needed to allocate memory, file system reads config and bitmask from disk, home directory setup creates user directories if it's the first boot, and finally the shell starts accepting commands.

#### Our Welcome Message

The system prints "Welcome to Dave and Timmy's OS, DownTownOS. Type help for a list of commands you can use." Then the prompt appears: `$`

The cursor blinks. The user can type commands. The OS is running.

## What We Learned

By building an OS from scratch, we learned how bootloaders work, how memory allocation works, how file systems work, how screens work, how text editors work, and how assembly and C interact. Of course there are much more complicated ways of doing each part of this, but we got the basics.

The 8088 is obsolete, but the ideas persist. Every OS has a bootloader, memory allocation, file systems, screen management, and system calls. The OS has many limitations. No multitasking, any program can crash the system, no networking, tiny files, tiny disk, and no GUI. Each of these features multiplies complexity. DownTownOS strips away all of the fancy advancements and demonstrates pure, essential concepts.

## Running It Today

You can boot DownTownOS in Bochs, VirtualBox, or even real hardware if you have an old PC and floppy drive. The `disk0.img` file is a complete system.

This was one of my favorite projects I've been a part of so far and I learned a lot from it. Thank you for reading all of this, hopefully you found it as cool as I do.
