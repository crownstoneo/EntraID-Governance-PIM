# Entra ID Project: Privileged Identity Management (PIM) & Standing Privilege Audit (INC-4021)

## Objective
Remediate standing **User Administrator** assignments in Microsoft Entra ID by converting tier-2 support accounts to time-bound **Just-In-Time (JIT)** eligible access governed by Azure MFA, mandatory ticket justification, and automated Microsoft Graph PowerShell verification.

## Environments & Prerequisites
- **Identity Provider:** Microsoft Entra ID Lab Tenant (`Crown Corp`)
- **Governance Controls:** Microsoft Entra Privileged Identity Management (PIM) & Identity Governance Audit Logs
- **Runtime:** PowerShell Core with the `Microsoft.Graph` PowerShell SDK
- **Target Scope:** Tier-2 Support Account (`Emma.VanDijk`) and `User Administrator` Directory Role
- **Required Microsoft Graph Permissions (Least Privilege):**
  - `RoleManagement.ReadWrite.Directory` (manage eligible/active role assignments and PIM role settings)
  - `RoleAssignmentSchedule.Read.Directory` (audit active/elevated role schedules)
  - `RoleEligibilitySchedule.Read.Directory` (audit eligible JIT role assignments)
  - `AuditLog.Read.All` (verify PIM activation and Identity Governance directory audit logs)

## Tools & Controls Used
- **Microsoft Entra PIM:** Active-to-Eligible role conversion, maximum activation duration (`1 hour`), `Azure MFA` requirement on activation, and mandatory justification input (`INC-4021`).
- **Identity Governance Audit Logs:** Verification of MFA registration, live JIT role activation events, and recorded justification strings (`INC-4021 - Tier 2...`).
- **Microsoft Graph PowerShell SDK:** `Get-MgRoleManagementDirectoryRoleEligibilitySchedule` and `Get-MgRoleManagementDirectoryRoleAssignmentSchedule` to programmatically verify Eligible (JIT) vs. Active (Standing) assignments.

## Incident & Policy Specification (INC-4021)
- **Incident ID:** `INC-4021`
- **Reported By:** Internal GRC / ISO 27001 Auditor
- **Priority:** High
- **Problem Statement:** Active tier-2 accounts holding standing **User Administrator** assignments in Entra ID, violating NIS2 and ISO 27001 least-privilege controls.
- **Target Role & Account:** `User Administrator` / `Emma.VanDijk`
- **Enforced Controls:** Eligible assignment, max 1-hour activation, Azure MFA required, justification with ticket reference required, and Graph PowerShell posture audit

## Methodology
- **Eliminate Standing Privileges (Active to Eligible):** Converted tier-2 support access (`Emma.VanDijk`) for **User Administrator** into an **Eligible** assignment so zero standing administrative privileges remain active at rest.
- **Enforce Time-Bound JIT & Activation Guards:** Configured PIM role settings for **User Administrator** with a maximum **1-hour** activation window, mandatory **Azure MFA** verification, and required justification on activation.
- **End-to-End JIT Validation:** Verified `Emma.VanDijk`'s security method registration, executed a live JIT elevation with justification `INC-4021`, and confirmed the `Success` event in the **Identity Governance | Audit logs**.
- **Automated Role Posture Audit:** Queried directory role eligibility and active assignment schedules via Microsoft Graph PowerShell to confirm `Emma VanDijk` holds `Eligible (JIT)` status while standing privileges are restricted strictly to `Global Administrator` (`Crown Stone`).

## Evidence

### 1. PIM Policy Configuration (1-Hour Cap, Azure MFA & Justification)
![PIM Policy Configuration](https://github.com/user-attachments/assets/a1802cb1-758c-4766-8ee0-2a8b7beddd6a)

### 2. Identity Governance Audit Log (Live JIT Activation & INC-4021 Justification)
![Identity Governance Audit Log](https://github.com/user-attachments/assets/732cfade-b96e-4955-bd77-f8daf2924ccd)

### 3. Automated Graph PowerShell Verification (Eligible JIT vs. Standing Active)
![PowerShell PIM Verification Output](https://github.com/user-attachments/assets/6cd62718-2a23-4b01-95fb-fb6e8e89c18c)

## Findings
Standing **User Administrator** assignments create unnecessary exposure if a tie
r-2 account is compromised outside active support work. Moving tier-2 staff to eligible JIT elevation with 1-hour limits, Azure MFA, and ticketed justification closes the standing-privilege gap without disrupting daily support operations.

## Security & Governance Value
- **Mitigates Standing Privilege Risk (Supports ISO 27001 / NIS2 Access Control):** Removes permanent elevated access at rest while preserving fast, auditable self-service elevation during business hours.
- **Enforces Least Privilege & Accountability (Supports GDPR / AVG Data Minimization):** Requires Azure MFA and a recorded justification/ticket (`INC-4021`) for every JIT activation, backed by immutable Identity Governance audit logs.
- **Continuous Audit Readiness:** Combines portal audit logs with repeatable Microsoft Graph PowerShell checks to prove control effectiveness during internal and external audits.

## Lessons Learned
- Converting standing role assignments to **Eligible** must be paired with clear activation rules (1-hour cap, Azure MFA, and ticket justification) so security improves without slowing down legitimate support workflows.
- Pairing Identity Governance audit logs with Graph PowerShell schedule queries provides independent, verifiable proof of both configuration state and live user elevation behavior.
