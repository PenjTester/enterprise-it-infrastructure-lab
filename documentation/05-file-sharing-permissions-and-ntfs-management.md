# 05 – File Sharing, Permissions, and NTFS Management

**Date Started:** 11/26/2025   
**Server:** LAB-DC01 (Windows Server 2019)  
**Client:** LAB-CLIENT01 (Windows 10 Pro)  

---

## Goal of This Milestone

Configure secure, department-based network file sharing using NTFS and Share permissions.  
Demonstrate centralized access control using Active Directory Security Groups (GG-HR, GG-Sales, GG-Management, GG-IT-Support),  
ensuring users can only access files intended for their department.

This milestone uses these principles:
- Least-privilege access (deny by default)
- AD Security Groups as the basis of folder access
- NTFS + Share permissions working together
- Validating folder access from a domain-joined client

---

## 1. Creating Department Shared Folders (Server-Side)

On **LAB-DC01**, four department folders were created in `C:\Shared`:

| Folder Name | Purpose |
|-------------|---------|
| C:\Shared\HR | HR department files |
| C:\Shared\Sales | Sales department files |
| C:\Shared\Management | Management files & documents |
| C:\Shared\IT-Support | IT department files |

*Screenshot — Departmental folders (HR, IT-Support, Management, Sales) visible in \\LAB-DC01\Shared from a domain-joined client*

<img width="1124" height="420" alt="image" src="https://github.com/user-attachments/assets/e446b82c-b326-45e2-ac4e-3411f3cbd060" />

---

## 2. NTFS Security Permissions Assignment

Each folder was configured using **NTFS permissions**, ensuring only the correct department security group has Modify access.

| Folder | Allowed Group | NTFS Permissions |
|--------|--------------|------------------|
| HR | GG-HR | Modify, Read, Write |
| Sales | GG-Sales | Modify, Read, Write |
| Management | GG-Management | Modify, Read, Write |
| IT-Support | GG-IT-Support | Modify, Read, Write |

🔹 Removed: `LAB\Users`, `Everyone`, and any other groups not needed  
🔹 Kept only: `SYSTEM`, `Administrators`, and correct **GG-department security group**  
🔹 **Applies to:** This folder, subfolders, and files

 *Screenshot: NTFS Advanced Security Settings for Sales*
 
<img width="764" height="514" alt="Screenshot 2025-11-26 144832" src="https://github.com/user-attachments/assets/13da3ff0-7084-494a-9429-1cb2dbe30e29" />

---

## 3. Share Permissions Configuration (Top-Level Folder Sharing)

Enabled sharing for `C:\Shared` at the top level.  
Share permissions were intentionally set to **Read-only** for all department security groups, while **NTFS permissions** control Modify or Full access inside each departmental folder.

| Security Group            | Share Permission | Purpose |
|--------------------------|------------------|---------|
| GG-Management            | Read             | Allows basic access to network share – NTFS controls actual modify rights inside Management folder |
| GG-HR                    | Read             | Allows basic access to network share – NTFS controls actual modify rights inside HR folder |
| GG-Sales                 | Read             | Allows basic access to network share – NTFS controls actual modify rights inside Sales folder |
| GG-IT-Support            | Read             | Allows basic access to network share – NTFS controls actual modify rights inside IT-Support folder |
| Domain Admins            | Full Control     | Administrative control for full share and NTFS management |

**Key Notes:**
- Share permissions are permissive (Read), while NTFS controls real access.
- Prevents accidental permission escalation using only Share-level settings.
- This design follows best practice: **“Share wide, secure with NTFS.”**

*Screenshot — Share Permissions for Shared (Top-Level Folder)*

<img width="359" height="444" alt="Screenshot 2025-11-26 150904" src="https://github.com/user-attachments/assets/d5f66262-9993-4228-a0ab-ffc56d9187bd" />


---

## 4. Final NTFS Effective Access Testing

Using **Effective Access** on LAB-DC01:  
We tested expected access results using a real user, *Michael Lee (GG-Sales)*.

### Expected Results:
| Folder | Access Expected |
|--------|------------------|
| HR |  Denied |
| Sales |  Allowed (Modify) |
| Management |  Denied |
| IT-Support |  Denied |

 *Screenshot: Effective Access results for MLee on Sales — Modify allowed*

<img width="854" height="911" alt="image" src="https://github.com/user-attachments/assets/b04c777b-67c2-41d2-8dcc-96d2544ddcfa" />

---

## 5. Live Access Testing from LAB-CLIENT01

Using real user accounts, we tested access from the **client workstation** to confirm centralized domain-based folder security.

| User | AD Group | Folder Allowed | Folder Denied |
|------|----------|----------------|---------------|
| John Miller | GG-Management | `Management` | HR, Sales, IT-Support |
| Michael Lee | GG-Sales | `Sales` | HR, Management, IT-Support |
| Sarah Davis | GG-HR | `HR` | Sales, Management, IT-Support |
| Emily Clark | GG-IT-Support | `IT-Support` | HR, Sales, Management |

---

 *Screenshot: Sales folder opened successfully from LabClient01 (MLee)*

<img width="1127" height="460" alt="Screenshot 2025-11-26 142415" src="https://github.com/user-attachments/assets/c119694a-0d21-4faa-8e95-c06a3faafc37" />

 *Screenshot: Access denied popup when attempting to open Management*
 
 <img width="1127" height="630" alt="Screenshot 2025-11-26 142446" src="https://github.com/user-attachments/assets/b76edbaa-e020-4c36-83c6-ea6b3afca5db" />


---

## 6. Results Summary

| Verification Check | Result |
|--------------------|--------|
| Department folders created in root Shared | ✔ |
| NTFS configured with correct group permissions | ✔ |
| Share permissions correctly enforced | ✔ |
| Effective Access validated on server | ✔ |
| Access testing confirmed via LAB-CLIENT01 | ✔ |
| Users restricted to department-specific folder | ✔ |

---

## Final Outcome

 Centralized, secure file access is now correctly enforced using:
- Active Directory Security Groups
- NTFS and Share Permissions
- Validated access via domain logons from client machine

 **Next Milestone: DHCP Configuration & Dynamic IP Assignment**

---
