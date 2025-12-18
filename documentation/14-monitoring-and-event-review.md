# 14 – Monitoring & Event Review
**Date: 12/16/2025**



## Purpose

The purpose of this milestone is to validate that the hardened domain environment can be **monitored and operationally assessed** using native Windows tooling. Rather than introducing new security infrastructure or reconfiguring audit policy, this milestone focuses on **verifying visibility** into authentication activity, policy enforcement, and system health across both workstations and domain controllers.

This milestone represents the operational phase that follows hardening and patch management. In an enterprise environment, administrators spend far more time **observing and validating system behavior** than continuously modifying configuration. The goal here is to demonstrate familiarity with where critical logs reside, how to interpret them at a high level, and how to distinguish normal background activity from events that would warrant investigation.

No configuration changes were made during this milestone.

---

## 14.1 Establishing Monitoring Visibility on the Workstation

Monitoring began on the hardened Windows 10 workstation (**LAB-CLIENT01**) to confirm that logging is enabled, accessible, and populated.

### Actions Performed
- Logged into **LAB-CLIENT01** using the break-glass administrative account.
- Opened **Event Viewer**.
- Verified that core log categories are present and populated:
  - **Windows Logs → Security**
  - **Windows Logs → System**

This confirms that the workstation is generating authentication, system, and policy-related telemetry and that an administrator can access this data for troubleshooting or review.

### Outcome
- Security and System logs were populated with recent events.
- Individual events could be opened and reviewed successfully.
- No errors prevented access to logs or event details.

*Screenshot: Event Viewer showing populated Security log on LAB-CLIENT01*  
<img width="1877" height="1180" alt="Screenshot 2025-12-16 152632" src="https://github.com/user-attachments/assets/f9161f1f-fadc-46f1-80d6-5350f7a01ed8" />


---

## 14.2 Reviewing Workstation Authentication & Policy Activity

With logging confirmed, authentication-related activity was reviewed on the workstation to verify that access controls and Group Policy enforcement are observable.

### Actions Performed
- Filtered the **Security** log for authentication-related events (Event IDs such as 4624 and 4625).
- Reviewed multiple recent logon events.
- Observed a mix of:
  - System-generated logons (e.g., SYSTEM, DWM, UMFD processes)
  - User session-related activity resulting from interactive logons.
- Reviewed **System** log entries related to Group Policy processing.

Modern Windows workstations generate multiple logon-related events per session due to session isolation, desktop management, and service initialization. The presence of these events confirms that authentication and session creation are being logged, even when user identities are abstracted across multiple processes.

### Outcome
- Authentication activity is observable on the workstation.
- Group Policy processing events are present, confirming that policy enforcement is visible and auditable.
- No unexplained or repeated authentication failures were observed during normal operation.

*Screenshot: System event log showing normal operating system and service activity.*  
<img width="1878" height="1181" alt="image" src="https://github.com/user-attachments/assets/f2e6cdf2-463b-48bb-88c1-1b13eab3cab4" />


---

## 14.3 Domain Controller Monitoring & Centralized Authentication Visibility

Monitoring then shifted to the Domain Controller (**LAB-DC01**) to validate centralized identity visibility, which is a core responsibility of domain infrastructure.

### Actions Performed
- Logged into **LAB-DC01** using a domain administrative account.
- Opened **Event Viewer**.
- Reviewed:
  - **Windows Logs → Security** for authentication events.
  - **Windows Logs → System** for domain service health (Netlogon, DNS, Group Policy).

Unlike workstations, Domain Controllers log authentication events in a centralized and identity-focused manner. Successful domain logons were visible with clear, human-readable domain user accounts.

### Outcome
- Successful authentication events (Event ID 4624) were observed for domain users.
- Domain authentication activity is centrally logged and reviewable.
- Domain services appeared operational, with no indicators of identity service failure.

*Screenshot: Successful domain user logon recorded on the domain controller.*  
<img width="1929" height="1143" alt="image" src="https://github.com/user-attachments/assets/609f13c6-3f88-47dd-8fab-226ef93bbfb9" />



---

## 14.4 Interpreting Noise vs Actionable Events

The final step of this milestone focused on **event interpretation**, which is critical for operational effectiveness.

### Observed Event Categories

**Expected / Benign Noise**
- DistributedCOM warnings (Event ID 10016)
- Desktop Window Manager (DWM) and font driver (UMFD) logons
- SYSTEM account activity

These events are common on healthy Windows systems and do not indicate security incidents when observed in isolation.

**Monitor but Do Not Immediately Act**
- DNS Client warnings
- Transient service warnings during startup or network changes

These events warrant awareness and trend observation but are not actionable unless persistent or correlated with service disruption.

**Actionable Events**
- Repeated authentication failures (Event ID 4625)
- Group Policy processing failures
- Account lockouts or unexpected privilege usage
- Domain authentication errors on the Domain Controller

These categories define what would require investigation in an operational environment.

### Outcome
- Established a clear mental framework for interpreting Windows events.
- Confirmed that the environment provides sufficient telemetry to detect operational or authentication issues.
- Demonstrated the ability to distinguish signal from background noise without overreacting to benign warnings.

---

## Milestone 14 Summary

Milestone 14 confirms that the enterprise lab environment is **operationally observable** following hardening and patch management. Authentication activity, policy enforcement, and system health can be reviewed using native Windows tools on both workstations and the Domain Controller.

This milestone does not introduce new configuration but instead validates that previously implemented controls can be monitored and assessed, which is a critical skill for infrastructure and network administrators. It serves as a bridge between system hardening and the final recovery-focused milestone.

The environment is now ready for **Milestone 15 – Backup & Recovery Procedures**.

