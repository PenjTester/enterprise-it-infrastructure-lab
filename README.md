# Enterprise IT Infrastructure Lab (Virtualized)

This repository documents the design and implementation of a Windows-based **virtual enterprise network**, built for hands-on experience with real-world IT administration, identity management, and networking concepts.

The goal is to demonstrate practical skills in **Active Directory, DNS, user management, security groups, domain authentication, and enterprise networking** — illustrating not just theoretical knowledge, but actual application and problem-solving.

---

##  Lab Overview

| Component | Details |
|-----------|---------|
| Virtualization Platform | Oracle VirtualBox (Type 2 Hypervisor) |
| Domain Name | `lab.local` |
| Server OS | Windows Server 2019 (Domain Controller / DNS) |
| Client OS | Windows 10 Pro (Domain-joined workstation) |
| Network Mode | Internal Network (`lab-network`) |
| Lab Purpose | Simulate enterprise IT identity and access infrastructure with documentation as portfolio evidence |

---

##  Milestone Documentation

| Milestone | Status | Documentation |
|-----------|--------|---------------|
| 01 – Domain Controller Setup | ✔️ Completed | `/documentation/01-domain-controller-setup.md` |
| 02 – Windows Client Domain Join | ✔️ Completed | `/documentation/02-client-domain-join.md` |
| 03 – Active Directory Identity Management | ✔️ Completed | `/documentation/03-active-directory-identity-management.md` |
| 04 – Group Management & Access Control | ✔️ Completed | `documentation/04-group-management-and-access-control.md` |
| 05 – File Sharing, Permissions, and NTFS Management | ✔️ Completed | `documentation/05-file-sharing-permissions-and-ntfs-management.md`
| 06 – DHCP Configuration & Dynamic IP Assignment | Upcoming | Coming soon |

---

##  Skills Demonstrated

- Configure Active Directory Domain Services (AD DS) and DNS  
- Join client machines to a domain and authenticate using AD credentials  
- Design and implement AD Organizational Unit (OU) structure  
- Create domain users and apply login/password policy  
- Validate domain identity via command-line tools (`whoami`, `nslookup`, `systeminfo`)  
- Document each task clearly using screenshots, explanations, and configuration evidence  
- Build a portfolio-ready GitHub-based lab environment
- Configure NTFS and Share Permissions for secure departmental access
- Validate access using Effective Access and client-side testing  

---

##  Project Objectives

- Build a virtualized, enterprise-style IT infrastructure
- Practice user, group, and Organizational Unit (OU) management
- Configure DNS and domain-based name resolution
- Join Windows clients to a domain and enable centralized authentication
- Apply Group Policy Objects (GPO) to enforce system-wide settings
- Demonstrate troubleshooting, documentation, and validation accuracy
- Present work professionally through structured GitHub documentation
- Implement centralized file access using AD Security Groups and NTFS Share permissions

---

##  Current Status

| Status Item | Completion |
|-------------|------------|
| Domain Controller built and operational | ✔️ |
| Active Directory domain `lab.local` established | ✔️ |
| Client (Windows 10) successfully joined to domain | ✔️ |
| DNS fully resolving internal hostnames | ✔️ |
| Organizational Units (OUs) created with proper hierarchy | ✔️ |
| Domain users created in departmental OUs | ✔️ |
| Domain login and identity validation confirmed via client | ✔️ |
| Documentation stored and structured in GitHub | ✔️ |
| Folder access enforced using NTFS and Share Permissions | ✔️ |
| Verified access restrictions via client workstation | ✔️ |

**Next:** Configure DHCP for dynamic IP assignment, validate scope options, and test client address leasing (Milestone 06)

---

##  Documentation and Portfolio Value

Each milestone includes:

- Purpose and real-world relevance  
- Step-by-step configuration  
- Screenshots from ADUC, DNS, Server Manager, PowerShell, or Client Login  
- Command-line validation and identity tests  
- Troubleshooting notes when applicable  
- Screenshot and description-based verification  


---

> _"This project is built incrementally. Every completed milestone serves as evidence of practical IT administration skills — not just theory, but actual implementation."_

---

