# Incident Requirements (`INC-4021`)

- **Ticket ID:** INC-4021
- **Reported By:** Internal GRC / ISO 27001 Auditor
- **Priority:** High

## Problem Statement
During our quarterly pre-audit check, active tier-2 accounts were identified holding permanent **User Administrator** assignments in Microsoft Entra ID (`iamcrownstoneoutlook.onmicrosoft.com`), violating NIS2 and ISO 27001 least-privilege controls.

## Requirements & Constraints
- Eliminate standing **User Administrator** assignments immediately without locking out tier-2 staff from performing password resets or user updates during business hours.
- Elevation must be time-bound (**max 1 hour**), require **fresh MFA**, and mandate a **ticket number / justification** before activation.
- **Global Administrators** must receive automated email alerts whenever the role is activated.
- Periodic **Access Reviews** must verify ongoing privileged eligibility.

## Definition of Done
Convert a test tier-2 account (`Emma.VanDijk`) from **Active** to **Eligible**, execute an end-to-end Just-In-Time (JIT) activation test, and verify the PIM Audit Log and Access Review outcomes.
