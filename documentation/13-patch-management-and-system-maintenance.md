# 13 — Patch Management & System Maintenance

## 13.1 Purpose and Scope

Milestone 13 focuses on demonstrating controlled patch management and basic system maintenance practices within the existing enterprise lab environment.

By this stage of the project, the lab already includes:
- A hardened workstation (LAB-CLIENT01)
- Multiple Group Policy Objects enforcing security controls
- Restricted privileged access to workstations
- A dedicated break-glass workstation admin account (AuditAdmin)

Rather than introducing complex patching infrastructure (such as WSUS or SCCM), this milestone intentionally demonstrates policy-driven Windows Update management appropriate for a small enterprise or lab environment. The emphasis is on predictable update behavior, avoiding disruptive restarts, and verifying policy application.

---

## 13.2 Baseline Update State Assessment

Before making any changes, the existing Windows Update state was reviewed on both the workstation and the domain controller to establish a baseline.

### LAB-CLIENT01 (Workstation)

- Logged in as: LAB\auditadmin
- Location: Settings → Update & Security → Windows Update

Observed state:
- No pending updates
- No restart required
- Windows Update enabled and functional
- Message present: “Some settings are managed by your organization”

This indicated that Windows Update behavior was already being influenced by Group Policy, which is expected in a domain-managed environment.

### LAB-DC01 (Domain Controller)

- Logged in using the standard administrative account
- Location: Settings → Update & Security → Windows Update

Observed state:
- No updates available
- No restart required
- Update functionality operating normally

No changes were made at this stage.

---

## 13.3 Configure Windows Update Policy for Workstations

To demonstrate centralized patch management for workstations, Windows Update behavior was configured via Group Policy using an existing GPO.

### GPO Used
- Advanced Workstation Hardening
- Linked to the Lab Workstations OU

### Configuration Path

Computer Configuration  
→ Policies  
→ Administrative Templates  
→ Windows Components  
→ Windows Update

### Policy Settings Applied

**Configure Automatic Updates**
- Enabled
- Option selected: 3 – Auto download and notify for install

This ensures updates are downloaded automatically but are not installed without administrator or user awareness.

**No auto-restart with logged on users for scheduled automatic updates installations**
- Enabled

This prevents unexpected reboots on active workstations and reflects standard enterprise workstation hygiene.

All other Windows Update settings were intentionally left Not Configured.

---

## 13.4 Policy Application and Validation

After configuring the policy, validation was performed on LAB-CLIENT01 to confirm correct delivery and behavior.

### Force Policy Refresh

- Logged in as: LAB\auditadmin
- From an elevated command prompt, ran:

gpupdate /force

Both Computer and User policy updates completed successfully.

### Verify GPO Application

From the same elevated command prompt, ran:

gpresult /r

Under COMPUTER SETTINGS, the following GPOs were confirmed as applied:
- Advanced Workstation Hardening
- Baseline Workstation Policy
- Privileged Access – Block Domain Admins on Workstations
- Default Domain Policy

This confirms that the Windows Update configuration is being delivered via Group Policy as intended.

*Screenshot: gpresult /r output showing COMPUTER SETTINGS and applied GPOs* 

<img width="731" height="566" alt="Screenshot 2025-12-16 131841" src="https://github.com/user-attachments/assets/343c12be-513b-4cd1-86ba-d52baa3c9bfa" />

  

---

## 13.5 Windows Update Behavior and Maintenance Awareness

### Windows Update Status Verification

On LAB-CLIENT01:
- Navigated to Settings → Update & Security → Windows Update
- Confirmed:
  - No errors
  - Normal update status
  - “Some settings are managed by your organization” message present

*Screenshot: Windows Update main page showing policy-managed status*  

<img width="1639" height="1082" alt="Screenshot 2025-12-16 131738" src="https://github.com/user-attachments/assets/5d1af841-7b8d-44ca-af2b-e37e3476e8f3" />


### Update History Review

- Selected View update history
- Page loaded successfully
- No recent updates listed, which is acceptable in a lab environment

*Screenshot: Windows Update → View update history page*  

<img width="1199" height="548" alt="Screenshot 2025-12-16 131918" src="https://github.com/user-attachments/assets/92253d45-ca56-4fd6-a114-773662762ab7" />


### Event Viewer Sanity Check

- Opened Event Viewer
- Reviewed Windows Logs → System

Observed:
- DistributedCOM (Event ID 10016) warnings
- DNS Client Events warnings
- Kernel-Power entry consistent with VM shutdown or reset

No Windows Update–specific errors or servicing failures were present. These events are common in virtualized lab environments and require no remediation at this stage.

---

## 13.6 Domain Controller Update Strategy

For LAB-DC01, updates remain manually managed.

This is an intentional design choice:
- Domain Controllers host critical services (AD DS, DNS, DHCP)
- Automatic restarts are undesirable
- Update timing should align with maintenance windows

This demonstrates role-based update strategy rather than blanket automation.

---

## Milestone 13 Summary

Milestone 13 demonstrates controlled, policy-driven patch management without unnecessary complexity.

Key outcomes:
- Established a predictable Windows Update strategy for workstations
- Prevented disruptive automatic restarts
- Verified policy delivery using gpupdate and gpresult
- Confirmed system health through update status and event review
- Preserved hardened security posture and privileged access controls
- Applied different update strategies for workstations versus servers

