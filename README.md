# Privileged Identity Management (PIM) — Just-In-Time (JIT) Access Lab in Microsoft Entra ID

## Project Overview
This lab demonstrates how to eliminate **Standing Admin Access** and reduce the privileged attack window using **Microsoft Entra ID Privileged Identity Management (PIM)**. Instead of granting permanent administrative permissions, a standard test user (`Test-User`) is configured as **Eligible** for the **Helpdesk Administrator** role and must perform time-bound **Just-In-Time (JIT)** elevation with **MFA** and **business justification**, with every action tracked in **PIM Resource Audit Logs**.

---

## Lab Environment
* **Identity Provider:** Microsoft Entra ID
* **License:** Microsoft Entra ID P2 (Trial)
* **Test Identity:** Standard Non-Admin User (`Test-User`)
* **Privileged Role:** `Helpdesk Administrator` (Directory Scope)
* **Activation Rules:** Max 1-hour duration, MFA required, Justification mandatory

---

## Implementation Steps & Proof of Execution

### 1. Assigning an Eligible Privileged Role (Zero Standing Access)
Configured **Helpdesk Administrator** role settings in PIM to enforce a **1-hour maximum activation duration**, **Microsoft Entra MFA**, and **mandatory justification**. Then assigned `Test-User` as **Eligible** (rather than Active) so the user carries zero standing administrative privileges during normal daily operations.

<img width="1920" height="905" alt="ca03 add assingment" src="https://github.com/user-attachments/assets/5b2d6e7a-7e63-4b9b-a28c-d92d38afa877" />
<img width="1920" height="905" alt="ca03 settings of add assingments" src="https://github.com/user-attachments/assets/57212d95-e7c5-400a-81b1-f8f804658945" />

---

### 2. Self-Service Just-In-Time (JIT) Activation (`Test-User` View)
Signed in as `Test-User` in an Incognito session and navigated to **PIM → My roles → Microsoft Entra roles → Eligible assignments**, where `Helpdesk Administrator` appeared ready for on-demand activation.

<img width="1920" height="922" alt="ca03 jit activate in test user login" src="https://github.com/user-attachments/assets/89a27daf-4ed1-4811-af3a-eeff2b2e118b" />

Clicked **Activate**, verified identity via MFA, retained the **1-hour** maximum duration, and submitted the required business justification (`Testing JIT privilege elevation for user password reset`).

<img width="1920" height="949" alt="ca03 activatre" src="https://github.com/user-attachments/assets/58ca87dd-8c49-44b5-85b4-b014c754fcd2" />

---

### 3. Verifying Time-Bound Active Role & Auto-Expiration
Once activated, the role moved to the **Active assignments** tab with an exact expiration timestamp (1 hour from activation) and an option to manually **Deactivate** early once the task is complete.

<img width="1920" height="951" alt="ca03 activation 3" src="https://github.com/user-attachments/assets/248b09bf-7a99-4b25-86b4-adbd28b33c1a" />

---

### 4. Auditing & Accountability (Admin View)
Verified the complete governance trail from the Global Administrator portal under **PIM → Microsoft Entra roles → Activity → Resource audit**. The log captured the exact **Requestor** (`Test-User`), **Action** (`Add member to role completed (PIM activation)`), **Target Role** (`Helpdesk Administrator`), **Reason/Justification**, **Status** (`Succeeded`), and **Expiration Timestamp**.

<img width="1920" height="917" alt="ca03 audit 2" src="https://github.com/user-attachments/assets/415149e8-84fc-4ebb-ba83-c3e61e5e8928" />

---

## Key IAM & Governance Concepts Demonstrated
* **Zero Standing Access:** Eliminated 24/7 permanent admin privileges to drastically shrink the attack window if a user account is compromised.
* **Just-In-Time (JIT) Elevation:** Enabled self-service, time-bound privilege activation (1 hour max) only when needed.
* **Least Privilege & Defense-in-Depth:** Enforced fresh MFA verification and mandatory business justification on every elevation attempt.
* **Audit & Compliance:** Established end-to-end accountability (Who, What, Why, and When) via PIM Resource Audit logs.

  ---

## Production Hardening Considerations
While this lab focuses on core PIM JIT mechanics in a single-tenant test environment, an enterprise deployment would add:
* **Scoped Delegation (Administrative Units):** Restricting `Helpdesk Administrator` assignments to specific regional or departmental Administrative Units instead of Directory-wide scope.
* **Approval Workflows for Tier-0 Roles:** Enforcing mandatory manager/IAM Lead approval and ITSM ticket validation for high-impact roles (e.g., `Global Administrator`, `Privileged Role Administrator`).
* **Continuous Monitoring:** Configuring PIM Alerts and forwarding audit logs to a SIEM (e.g., Microsoft Sentinel or Splunk) to detect anomalous elevation frequency.
