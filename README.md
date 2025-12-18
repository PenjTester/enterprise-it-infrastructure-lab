# Enterprise Windows Infrastructure Lab (VirtualBox)

This repository contains a fully documented Windows enterprise-style lab built in Oracle VirtualBox.

The environment simulates a small Active Directory domain, including a domain controller providing DNS and DHCP services, an organized Active Directory structure, and a Windows 10 domain-joined workstation. Each milestone represents a common administrative responsibility and is documented step by step with validation and screenshots.

The project emphasizes practical enterprise administration: centralized identity management, policy-driven configuration, security hardening, monitoring, and recovery. The documentation focuses on what was configured, why each decision was made, and how the environment behaves under real administrative constraints.


---

## Lab Environment

| Component | Details |
|----------|---------|
| Virtualization | Oracle VirtualBox |
| Domain Name | `lab.local` |
| Server | Windows Server 2019 (Domain Controller / DNS / DHCP) |
| Workstation | Windows 10 Pro (Domain-joined client) |
| Network Mode | Internal Network (`lab-network`) |

The lab runs fully isolated inside VirtualBox and is designed to model common enterprise administration workflows rather than ad-hoc configuration.

---

## Network Diagram

<img width="1536" height="1024" alt="labdiagram" src="https://github.com/user-attachments/assets/184c74dc-6057-47ec-bbde-a8931cdfbd18" />


---

## Milestones

Each milestone has its own detailed documentation file under `/documentation/`.

| Milestone | Status | Documentation |
|----------|--------|---------------|
| 01 – Domain Controller Setup |  Completed | `/documentation/01-domain-controller-setup.md` |
| 02 – Client Domain Join |  Completed | `/documentation/02-client-domain-join.md` |
| 03 – Active Directory Identity Management |  Completed | `/documentation/03-active-directory-identity-management.md` |
| 04 – Group & Access Control |  Completed | `/documentation/04-group-management-and-access-control.md` |
| 05 – File Sharing & NTFS Permissions |  Completed | `/documentation/05-file-sharing-permissions-and-ntfs-management.md` |
| 06 – DHCP Configuration (Dynamic IP Assignment) |  Completed | `/documentation/06-dhcp-configuration-dynamic-ip-assignment.md` |
| 07 – DNS Management & Domain Service Reliability |  Completed | `/documentation/07-dns-management-and-domain-service-reliability.md` |
| 08 – Workstation OU Structure & Baseline GPO Deployment |  Completed | `/documentation/08-workstation-OU-structure-and-baseline-GPO-deployment.md` |
| 09 – Advanced Workstation Hardening | Completed | `/documentation/09-advanced-workstation-hardening.md` |
| 10 – Domain Admin & Privileged Access Hardening | Completed | `/documentation/10-privileged-access-management-and-administrative-account-hardening.md` |
| 11 – User Lifecycle & Access Operations | Completed | `/documentation/11-user-lifecycle-and-access-operations.md` |
| 12 – Break-Glass Access for Hardened Workstations | Completed | `/documentation/12-break-glass-access-for-hardened-workstations.md` |
| 13 – Patch Management & System Maintenance | Completed | `/documentation/13-patch-management-and-system-maintenance.md` |
| 14 – Monitoring & Event Review | Completed | `/documentation/14-monitoring-and-event-review.md` |
| 15 – Backup & Recovery Procedures | Completed | `/documentation/15-backup-and-recovery-procedures.md` |
| 16 – Enterprise Administration Capstone & Final Documentation | Completed | `/documentation/16-enterprise-administration-capstone-and-final-documentation.md` |

---

## What This Project Covers

- Installing and configuring a Windows Server domain controller  
- Implementing Active Directory Domain Services (AD DS) as a centralized identity authority  
- Configuring DNS zones, records, scavenging, and domain service reliability  
- Deploying DHCP scopes, leases, reservations, and dynamic DNS registration  
- Joining and managing Windows 10 domain clients  
- Designing Organizational Unit (OU) structures aligned with administrative intent  
- Managing users, groups, and role-based access control using ADUC  
- Implementing NTFS and SMB share permissions with layered security models  
- Deploying baseline and advanced Group Policy Objects (GPOs)  
- Enforcing workstation hardening and credential protection via policy  
- Restricting administrative access and implementing privileged access separation  
- Enabling PowerShell logging, script block auditing, and command visibility  
- Monitoring authentication, policy processing, and system behavior via Event Viewer  
- Validating patch management behavior and update governance  
- Implementing and verifying backup and recovery procedures  
- Maintaining milestone-based documentation and configuration drift awareness  

*The documentation folder contains full walkthroughs for each milestone.*

---




