# 03 – Active Directory Identity Management

**Date Started:** 11/21/2025  
**Server:** LAB-DC01 (Windows Server 2019)  
**Client:** LAB-CLIENT01 (Windows 10 Pro)  

---

##  Goal of This Milestone

Design and implement a realistic Active Directory identity structure to support:

- Organizational Units (OUs) for users, computers, and admin accounts  
- Proper OU nesting and hierarchy  
- Future Group Policy targeting (GPO)  
- Preparation for domain user creation and group-based access control  

---

## 1️. Accessing Active Directory Users and Computers (ADUC)

Opened **Active Directory Users and Computers (ADUC)** on LAB-DC01 to begin structuring the domain.

 *Screenshot — ADUC open, showing lab.local before any OU organization*

<img width="825" height="181" alt="Screenshot 2025-11-21 125849" src="https://github.com/user-attachments/assets/b569d69c-6b49-418e-9977-440432daedb4" />


---

## 2️. Creating Organizational Units (All OUs Created — Flat Structure)

All required OUs were created at this stage, but were still at the same level (not yet nested into their parent OUs).

| OU Name Created |
|------------------|
| _IT_Admins |
| _Users |
| _Computers |
| _Groups |
| _Management |
| _HR |
| _Sales |
| _IT_Support |
| _Workstations |
| _Servers |

 *Screenshot — ADUC displaying all OUs created under lab.local, before nesting*

<img width="352" height="407" alt="Screenshot 2025-11-21 131434" src="https://github.com/user-attachments/assets/23edfaa7-326f-4cd5-86bc-7156f8cec3c7" />


---

## 3️. OU Hierarchy (After Nesting)

After temporarily disabling **Protect object from accidental deletion**, OUs were properly nested into the intended structure:

```
lab.local
│
├── _IT_Admins
│
├── _Users
│   ├── _Management
│   ├── _HR
│   ├── _Sales
│   └── _IT_Support
│
├── _Computers
│   ├── _Workstations
│   └── _Servers
│
└── _Groups
```

Protection was re-enabled once final placement was complete.

 *Screenshot — Final OU hierarchy correctly nested under lab.local*

<img width="350" height="234" alt="image" src="https://github.com/user-attachments/assets/2d97e102-16c9-46e5-85f2-cc9337f666dd" />


---

##  Note on Object Protection

Some OUs could not be moved initially due to **Protect object from accidental deletion** being enabled.  
Protection was temporarily disabled to allow nesting and re-enabled after structure was finalized.

---

##  Current Milestone Status

| Task | Status |
|------|--------|
| Create all required OUs | ✔️ |
| Nest OUs into hierarchy | ✔️ |
| Re-enable object protection | ✔️ |
| Prepare for user account creation | ⏳ Next |

---

###  Next Step

Begin creating domain user accounts within their appropriate departmental OUs.

