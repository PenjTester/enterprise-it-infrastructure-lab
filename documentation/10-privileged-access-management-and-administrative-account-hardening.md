# 10 – Privilege Access Management and Administrative Account Hardening

## Overview

This milestone enforces enterprise-style privileged access boundaries in the `lab.local` domain.

**Goals:**

- Normal domain users (e.g., `jmiller`, `sdavis`, `mlee`, `eclark`) can log on to `LAB-CLIENT01` and work normally.
- Privileged accounts (`lab-admin`, `Domain Admins`, `Administrator`) are blocked from logging on to `LAB-CLIENT01` (locally and via RDP).
- Local Administrator–type accounts cannot be used for network logon.
- All controls are implemented via a GPO linked to the workstation OU.

This demonstrates real systems/network administrator hardening practices, not just default AD setup.

---

## 10.1 – Verify Administrative OU Structure and Privileged Account

On `LAB-DC01`, open **Active Directory Users and Computers** and confirm:

- The following OUs exist at the domain root:
  - `_IT_Admins`
  - `_Users`
  - `_Groups`
  - `_Computers`

- Under `_IT_Admins`, confirm your dedicated privileged account exists:
  - `lab-admin`

*Screenshot: ADUC top-level OU structure showing `_IT_Admins`, `_Users`, `_Groups`, `_Computers`;ADUC focused on `_IT_Admins` with `lab-admin` visible*
  
<img width="1191" height="796" alt="Screenshot 2025-12-08 133926" src="https://github.com/user-attachments/assets/647b2916-c878-4d4f-a201-7d3530d5bc67" />
   

---

## 10.2 – Confirm Workstation OU Placement

Still in ADUC, confirm the workstation is in the correct OU path:

- Expand:
  - `lab.local`
    - `_Computers`
      - `_Workstations`
        - `Lab Workstations`

Verify that:

- `LAB-CLIENT01` is located under `Lab Workstations`.

 *Screenshot: ADUC showing `LAB-CLIENT01` inside `_Computers → _Workstations → Lab Workstations`.*
<img width="1184" height="232" alt="Screenshot 2025-12-08 145036" src="https://github.com/user-attachments/assets/a38df667-6f65-4fd0-9a20-5db0aa058433" />


This OU is the scope where workstation-targeted hardening GPOs will apply.

---

## 10.3 – Create the Workstation Privileged Access GPO

On `LAB-DC01`:

1. Open **Group Policy Management**.
2. In the left pane, navigate to:
   - `Forest: lab.local`
     - `Domains`
       - `lab.local`
         - `_Computers`
           - `_Workstations`
             - `Lab Workstations`
3. Right-click `Lab Workstations` and select:
   - “Create a GPO in this domain, and Link it here…”
4. Name the new GPO (linked to `Lab Workstations`):

   `Privileged Access – Block Domain Admin Logon`

*Screenshot: GPM showing the GPO “Privileged Access – Block Domain Admin Logon” linked to `Lab Workstations`.*
<img width="1248" height="472" alt="image" src="https://github.com/user-attachments/assets/b5428eb6-f4e5-4c47-a4d3-1d6bb27e11e6" />


---

## 10.4 – Deny Local Logon for Privileged Accounts

Edit the new GPO:

1. In the GPO editor, navigate to:
   - Computer Configuration  
     → Policies  
     → Windows Settings  
     → Security Settings  
     → Local Policies  
     → User Rights Assignment

2. In the right pane, locate and open:
   - “Deny log on locally”

3. Enable and configure “Deny log on locally”:
   - Select “Define these policy settings”.
   - Add:
     - `Domain Admins`
     - `lab-admin`
     - `Administrator` (optional but recommended)

4. Apply and close the dialog.

*Screenshot: GPO editor showing “Deny log on locally” populated with `Domain Admins`, `lab-admin`, and (optionally) `Administrator`.*
<img width="1339" height="833" alt="Screenshot 2025-12-08 171959" src="https://github.com/user-attachments/assets/4e1bc63e-9900-4e14-aeb5-e9649975c32a" />


This prevents privileged accounts from logging on interactively at the workstation console.

---

## 10.5 – Deny Remote Desktop Logon for Privileged Accounts

In the same GPO (`Privileged Access – Block Domain Admin Logon`):

1. Still under:
   - Computer Configuration  
     → Policies  
     → Windows Settings  
     → Security Settings  
     → Local Policies  
     → User Rights Assignment

2. Locate and open:
   - “Deny log on through Remote Desktop Services”

3. Configure “Deny log on through Remote Desktop Services”:
   - Select “Define these policy settings”.
   - Add:
     - `Domain Admins`
     - `lab-admin`
     - `Administrator` (optional but recommended)

*Screenshot: GPO editor showing “Deny log on through Remote Desktop Services” with the same privileged identities listed.*
<img width="1339" height="831" alt="Screenshot 2025-12-08 172007" src="https://github.com/user-attachments/assets/2f6a43fb-b2f7-4b40-9c57-8e9f27a40bb6" />


This prevents privileged accounts from using RDP to log into workstations.

---

## 10.6 – Block Local Administrator from Network Access

Still in the same GPO and same node (**User Rights Assignment**):

1. Locate and open:
   - “Deny access to this computer from the network”

2. Configure “Deny access to this computer from the network”:
   - Select “Define these policy settings”.
   - Add:
     - `Local account`
     - `Local account and member of Administrators group`

*Screenshot: GPO editor showing “Deny access to this computer from the network” populated with `Local account` and `Local account and member of Administrators group`.*
<img width="1339" height="404" alt="image" src="https://github.com/user-attachments/assets/9a2537d1-ca56-47c7-8933-13e9e4addf27" />


This reduces lateral movement risk by preventing local Administrator–type accounts from being used for network logon.

---

## 10.7 – Apply the GPO and Refresh Policies

On `LAB-DC01`:

1. Open an elevated command prompt.
2. Run:
   - `gpupdate /force`

On `LAB-CLIENT01`:

1. Restart the workstation to ensure computer-side policy is fully applied.


---

## 10.8 – Validate Normal User Functionality

After the reboot of `LAB-CLIENT01`:

1. At the logon screen, log in as a **normal domain user**, for example:
   - `jmiller`
2. Confirm that:
   - The desktop loads normally.
   - Network access is functional (e.g., open Command Prompt and ping `LAB-DC01` and `lab.local`).
   - Access to the user’s departmental share still works via the UNC path you configured in earlier milestones (for example, the department-specific share for jmiller).

*Screenshot: `LAB-CLIENT01` logged in as `jmiller`, with a command prompt showing successful pings to `LAB-DC01` and `lab.local`.*
<img width="567" height="417" alt="image" src="https://github.com/user-attachments/assets/cc417128-3861-40d9-abe6-ba899a161b19" />
 
 *Screenshot: File Explorer on `LAB-CLIENT01` showing successful access to the "management" share for jmiller.*
 <img width="1120" height="434" alt="image" src="https://github.com/user-attachments/assets/38cf63ea-9e0d-4401-a8f2-daba952a959c" />


This confirms that standard user workflows are not broken by the new restrictions.

---

## 10.9 – Validate That Privileged Accounts Are Blocked on Workstations

On `LAB-CLIENT01`, at the Windows logon screen:

1. Click “Other user”.
2. Attempt to log on using:
   - `lab-admin`
3. Enter the correct password.
4. Observe the logon result.

Expected behavior:

- The logon attempt is denied with a message such as:
  - “The sign-in method you’re trying to use isn’t allowed.”
  - Or equivalent text indicating that the account is not permitted to sign in here.

Optionally, repeat the test with:

- `Administrator` (domain or local, depending on configuration)
- Any other privileged identity added to the deny lists above.

*Screenshot: `LAB-CLIENT01` showing the logon failure screen for `lab-admin`, with a message indicating that the sign-in is not allowed.*
<img width="856" height="542" alt="Screenshot 2025-12-08 135611" src="https://github.com/user-attachments/assets/104af088-6444-432d-a6f2-00f989c791da" />




Because the restriction is enforced locally via “Deny log on locally” and “Deny log on through Remote Desktop Services”, the workstation can block these logons before any full domain authentication occurs. As a result, the domain controller may not log a 4625 failed logon event for these specific denied workstation logons. This is expected given this hardening approach; behavioral evidence at the client (failed logon with “not allowed” message) is the primary validation.

---

## 10.10 – Milestone Completion Summary

At the end of Milestone 10:

- `_IT_Admins` exists and contains your dedicated privileged admin account (`lab-admin`).
- `LAB-CLIENT01` resides in `_Computers → _Workstations → Lab Workstations`, where workstation-hardening GPOs are scoped.
- GPO `Privileged Access – Block Domain Admin Logon` is linked to `Lab Workstations` and configured to:
  - Deny local logon for:
    - `Domain Admins`
    - `lab-admin`
  - Deny Remote Desktop logon for the same privileged identities.
  - Deny network access from:
    - `Local account`
    - `Local account and member of Administrators group`
- Normal domain users (e.g., `jmiller`) can still log in to `LAB-CLIENT01`, access the network, and reach their departmental shares.
- Privileged accounts are prevented from interactive use on workstations, aligning the lab with real-world privileged access management practices.

This milestone completes the first step of privileged access hardening and prepares the environment for subsequent server and domain controller–focused hardening in future milestones.
