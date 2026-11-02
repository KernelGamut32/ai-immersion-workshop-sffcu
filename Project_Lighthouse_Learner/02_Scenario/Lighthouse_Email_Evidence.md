# Lighthouse Email Evidence

Project Lighthouse | Scenario evidence as of October 29, 2026

**Fictional training messages.** All names, addresses, figures, and events are invented. These are readable copies of the five separate EML files. A message saved as a file is not an Outlook mailbox item. Use this packet for file-grounded practice if native mailbox seeding is unavailable.

## M01 Project Lighthouse | Weekly decisions and funding

**From:** Priya Natarajan <priya.natarajan@example.com>  
**To:** Jordan Reyes <jordan.reyes@example.com>  
**Date:** Wed, 21 Oct 2026 09:05:00 -0700  
**Matching file:** `01_Priya_Pilot_Priorities.eml`

Jordan,

Please keep the sponsor update tied to the October 12 charter. The agreed approach is a four-branch pilot, focused on digital access and first contact resolution alongside branch wait reduction. Our pilot candidates are Cedar Grove B001, Riverside B007, Brookstone B012, and Redwood Center B019. Wider rollout still depends on evidence from the pilot.

The third decision is to use aggregated, coded survey data in sponsor materials. Please do not add raw member comments to make the story more vivid. The Q3 survey extract should give us enough evidence to decide where to investigate first.

The next formal program milestone is the November 20 pilot readiness and sponsor go or no go review. It is not a committed launch date. Please cite the charter decisions rather than recasting proposals as approvals.

Priya

## M02 Re: Project Lighthouse | Weekly decisions and funding

**From:** Priya Natarajan <priya.natarajan@example.com>  
**To:** Jordan Reyes <jordan.reyes@example.com>  
**Date:** Mon, 26 Oct 2026 14:20:00 -0700  
**Matching file:** `02_Priya_Vendor_Delay.eml`

Jordan,

The vendor has moved delivery of the test connector from November 6 to November 13. I own risk R02, Digital vendor integration delay. I have reserved November 16 to 18 for integration tests, but there is little time to recover if the connector arrives late or critical defects remain.

We can run a stub based test while we wait. That will exercise our side of the workflow; it will not prove the production integration. Please do not describe the vendor dependency as resolved just because the stub test passes.

Devon still owns R01, Branch readiness and coaching coverage. He is confirming protected coaching time and queue routing checks for all four pilot branches. We should present those as two separate risks with separate owners.

Priya

## M03 Re: Project Lighthouse | Weekly decisions and funding

**From:** Priya Natarajan <priya.natarajan@example.com>  
**To:** Jordan Reyes <jordan.reyes@example.com>  
**Date:** Wed, 28 Oct 2026 10:10:00 -0700  
**Matching file:** `03_Priya_Budget_Update.eml`

Jordan,

The planning workbook now shows a $2.1M Lighthouse budget, including accelerated vendor integration support. Please use that number in the sponsor pack so the delivery discussion reflects the latest planning assumptions.

The extra support would protect the November test window, but I cannot promise it removes the vendor risk. I have included the supporting scope in CR-07 for the November 5 sponsor decision discussion.

I have not received an updated signed charter. If you need the approved baseline for a formal statement, please reconcile the planning number with the funding record before circulation. I would rather surface that question now than have Marcus find two different totals in the same pack.

Priya

## M04 Re: Project Lighthouse | Weekly decisions and funding

**From:** Priya Natarajan <priya.natarajan@example.com>  
**To:** Jordan Reyes <jordan.reyes@example.com>  
**Date:** Thu, 29 Oct 2026 11:15:00 -0700  
**Matching file:** `04_Priya_Sponsor_Request.eml`

Jordan,

For the sponsor discussion, please show the tradeoff between a phased pilot within the approved scope and accelerated integration support. The charter has a third option to defer the readiness gate if we cannot meet the critical dependency. No replacement date has been approved.

Please keep the November 5 sponsor decision deadline separate from the November 20 formal readiness milestone. The decision deadline is when we need direction on the delivery path and CR-07; it is not the milestone date for the pilot.

Use the Q3 survey to show where follow-up is warranted. Keep Digital web banking separate from Mobile App, and show response counts with percentages. A branch comparison can reflect the mix of respondents and channels, so please avoid claiming that a branch manager caused the difference.

Priya

## M05 Project Lighthouse | Check survey content before sharing

**From:** Elena Vasquez <elena.vasquez@example.com>  
**To:** Jordan Reyes <jordan.reyes@example.com>  
**Date:** Thu, 29 Oct 2026 11:40:00 -0700  
**Matching file:** `05_Elena_Data_Handling.eml`

Jordan,

Before the sponsor deck or workbook leaves the working group, can you confirm whether the survey export contains free-text member comments, member names, account numbers, or contact details? A field called CommentTheme could mean a controlled code or text copied from a comment. Please open the actual workbook and check the schema and populated fields.

If the extract contains only synthetic response IDs and coded themes, record that finding and use aggregated channel and branch summaries in the deck and email. Do not describe an unseen dataset as free of personal information. ResponseID in this training extract is a record key, not a member identifier.

Please also check the real recipient, attachments, and permissions before any send. A visible Training use only footer is a handling reminder; it is not proof that a Microsoft Purview sensitivity label or DLP policy has been applied.

Elena
