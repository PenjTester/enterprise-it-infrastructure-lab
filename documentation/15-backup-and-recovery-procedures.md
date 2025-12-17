## 15 – Backup Configuration & Validation (Windows Server Backup)

### Objective
Create and validate a **System State** backup for the domain controller (LAB-DC01) using **Windows Server Backup**, writing the backup to a dedicated backup disk to support recovery of core domain services (AD DS/SYSVOL/registry/boot state).

---

### Environment
- **Hypervisor:** Oracle VirtualBox
- **Domain Controller:** LAB-DC01 (Windows Server 2019)
- **Backup Tool:** Windows Server Backup
- **Backup Scope:** System State
- **Backup Target:** Dedicated virtual disk mounted as **DC_Backups (E:)**

---

## 15.1 – Provision Dedicated Backup Storage (Virtual Disk)
A dedicated virtual hard disk was attached to LAB-DC01 to serve as the backup destination. Using a separate disk mirrors enterprise practice by keeping backups off the OS volume.

---

## 15.2 – Initialize, Format, and Assign Backup Disk (E:)
The new disk was brought online in **Disk Management**, formatted as **NTFS**, labeled **DC_Backups**, and assigned drive letter **E:**.

*Screenshot: Disk Management showing DC_Backups (E:) healthy primary partition*  
<img width="1054" height="831" alt="Screenshot 2025-12-17 133451" src="https://github.com/user-attachments/assets/0f6c4a0a-69a1-4a43-88f9-34fc5c316741" />


---

## 15.3 – Run a One-Time System State Backup to E:
A one-time backup was executed to validate the backup workflow before considering any scheduling. The backup was configured as **System State** and written to **E:** (DC_Backups). The job completed successfully, transferring **8.04 GB**.

*Screenshot: Backup Once Wizard showing System state completed (8.04 GB transferred)*  
<img width="669" height="554" alt="Screenshot 2025-12-17 135747" src="https://github.com/user-attachments/assets/22cdd7fe-bfa6-4fd7-898c-66e07698a17f" />


---

## 15.4 – Confirm Successful Backup in Windows Server Backup Console
After completion, Windows Server Backup was used to confirm that the backup job registered correctly and shows as **Successful** in the Local Backup view.

*Screenshot: Windows Server Backup (Local Backup) showing Backup status successful*  
<img width="1666" height="838" alt="Screenshot 2025-12-17 135814" src="https://github.com/user-attachments/assets/d2b59546-df1d-4e41-9aa6-a9e92ebfd748" />


---

## 15.5 – Review Backup Details (Location, VSS Mode, Timestamps)
The backup details were opened to validate the specific metadata:
- **Backup location:** E:
- **VSS settings:** VSS Copy Backup
- **Status:** Successful
- **Start/End times:** Confirmed in details window

*Screenshot: Backup details window showing E:, VSS Copy Backup, Successful, timestamps*  
<img width="498" height="581" alt="Screenshot 2025-12-17 135932" src="https://github.com/user-attachments/assets/93a02626-3572-4de9-bdc7-93bea39a07d9" />


---

### Result
LAB-DC01 now has a verified **System State** backup stored on **DC_Backups (E:)**, providing a recoverable baseline for domain controller state and core services.

---

### Notes
- This milestone focused on **backup validation**, not automation/scheduling.
- Using a dedicated backup disk keeps the environment clean and reduces the risk of storing backups on the same volume being protected.
