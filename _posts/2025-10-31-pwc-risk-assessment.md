---
layout: post
title: "PwC Risk Assessment Simulation"
date: 2025-10-31
---

Yesterday I completed the PwC US Cyber Security Consulting Job Simulation on Forage, focused on performing a risk assessment and evaluating IT controls. The experience walked through what a real cybersecurity risk assessment might look like, including performing a gap analysis, documenting tests of design and operating effectiveness, and summarizing findings for a final presentation.

---

## Conducting the Gap Analysis

The first task was to conduct a gap analysis to determine what a company would need to do to comply with SOX. I was given documentation of the company’s current procedures and had to identify what was missing compared to what SOX actually mandates.

Since this was my first risk assessment, I asked AI to help outline a basic step-by-step process to follow:

1. Outline what the madnated process flow should look like.
2. Use reference materials or frameworks to recall what good controls look like.
3. Read the client document line by line and ask: 
   - What could go wrong
   - Is there a control in place to prevent it from going wrong
4. Write every gap in a simple audit format: Gap, Risk.
5. Form questions and recommendations based on gaps found.

---

## Existing Process Overview

Here’s the summarized “normal” process flow provided by the client:

- An employee submits a requisition to the procurement system, which is automatically approved.
- The PC find the best option and submit a purchase order (PO).
- The PO is sent to the supplier, who sends back a receipt of the PO.
- The supplier ships materials with a goods receipt, which is entered into the warehouse management system.
- The supplier sends an invoice, which is compared to the goods receipt.
- The warehouse or inventory team uses a shared password to check the “invoice validated” box in the general ledger system. Once checked, the purchasing team is notified to send payment.
- The PC writes and signs a check for the supplier (even if they were the same person who initiated the requisition). Checks must be sent within 90 days.
- Access to the purchasing system is managed by the Procurement Department. Anyone can request access over email, and privileged access can be granted with verbal approval over the phone.

---

## Gaps Identified

When compared to best practices, I recognized the following gaps, risks:

- Autoamtic approval of requisitions, risk of unauthorized spending.
- A single person selects vendors, gets quotes, and creates POs. no separation of duties creates high risk of fraud.
- Shared password to confirm invoices, no audit trail. 
- Same person writes and signs checks, fraud risk.
- No mention of budgets or restricted funds, possible misuse of funds.
- Access granted via email or phone, minimal security or auditability (also breaks least privilege.

---

## Drafting My Email Summary

Next, I summarized my findings in an email to the senior associate:

> Hello Senior Associate,
>
> I reviewed your P2P SOP and compared the documented process to standard IT controls and cash disbursement practices. Below is a summary of control gaps and related risks:
>
> **Business Process Gaps**
> - Purchase requisitions auto-approve for all employees without any authorization. 
> - The same purchasing coordinator requests, approves, and pays vendors (no separation of duties).
> - No mention of any budgets or restricted funds.
> - Only one check signer is required for disbursements, who is the same PC from above.
>
> **System Control Gaps**
> - Shared credentials used to validate invoices lack auditability and accountability.
> - Procurement system access granted via email or verbal approval can be easily bypassed or forged.
> - No mention of user access reviews or privilege management.
>
> **Key Risks**
> - Fraudulent or unauthorized disbursements.
> - Lack of accountability for system actions.
> - Misuse of funds or noncompliance with restrictions.
>
> **Questions for Follow-Up**
> - Do you have any separation of duty practices in place?
> - Do you conduct any operational reviews or audits, and how are they documented?
>
> Best, 
> David

After reviewing the example answer, I realized I had the right idea but didn’t execute it quite as cleanly. I missed a few control gaps and my email formatting could have been more polished. For a first attempt, I was happy with how it turned out.

---

## SDLC Walkthrough

The next activity involved preparing for a meeting with an IT manager to discuss the System Development Life Cycle (SDLC) of a new payroll system. It was mentioned that the IT manager was vague, so I was told to come prepared with questions to fill in any gaps.

I first reviewed the standard SDLC phases:

- Initiation: Identify the need and purpose for the system, start security planning, and define key roles.  
- Development: Design, purchase, and program the system; conduct risk assessments and document security requirements.  
- Implementation: Configure security, conduct design reviews, test functionality, and obtain formal authorization to operate.  
- Operation: Maintain and monitor the system, test modifications, and ensure ongoing compliance.  
- Disposal: Plan for system retirement, data archiving, or secure destruction.

### MedTech’s SDLC Notes
- Initiation: No ISSO identified.  
- Development: “Testing” too broad.  
- Implementation: Missing design reviews or system tests.  
- Operation: No mention of configuration management.  
- Disposal: No disposal phase documented.

These notes helped me perform well on the quiz during the simulated walkthrough.

---

## Final Summary

The final task was to create a one-slide summary of all my findings to present to the company’s compliance manager. I condensed everything into a few high-level points highlighting the key process and system control gaps, along with associated risks. Once I submitted the slide, the simulation was complete.

Overall, this was a fun and valuable exercise. It helped me get comfortable reading real documentation, identifying control weaknesses, and communicating findings clearly. It also made me wonder how different a real-world gap analysis might be. Probably harder, and maybe with more hidden issues that companies are intentially trying not to expose.

---

### Key Takeaways

- Completed a **cybersecurity risk assessment** simulation for PwC.  
- Performed a **gap analysis** comparing company procedures to SOX requirements.  
- Documented a **Test of Design and Operating Effectiveness**.  
- Created a **summary slide** for presentation to a compliance manager.  
- Gained hands-on experience in **identifying risks and control deficiencies**.

---

**PwC US Cyber Security Consulting Job Simulation — October 2025**
