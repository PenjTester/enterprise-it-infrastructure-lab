# Lab Setup — Domain Controller Build (Milestone 1)

**Date:** 11/18/2025  
**Machine:** VirtualBox — Windows Server 2019 (GUI)  
**Host PC Specs:** Ryzen 9 9900X, 32 GB DDR5 RAM, 2TB NVMe SSD

---

##  Objectives of this Milestone

Set up the core identity infrastructure of a virtual enterprise network:

- Install Windows Server 2019
- Promote it to a Domain Controller
- Create Active Directory Domain Services (AD DS)
- Configure DNS for the new domain
- Assign permanent (static) IP for reliability and name resolution

---

##  1. Virtual Machine Configuration (VirtualBox)

Before installing Windows Server 2019, I configured the VM with essential system resources and an internal network adapter for domain networking.

| Setting          | Value |
|------------------|-------|
| OS Type          | Windows Server 2019 (GUI) |
| RAM              | 2.5 GB |
| CPU Cores        | 2 vCPU |
| Disk             | 60 GB (VDI) |
| Network Adapter  | Internal Network (`lab-network`) |
| Adapter Type     | Intel PRO/1000 MT Desktop (82540EM) |
| Guest Additions Installed | Yes |

 *Screenshot — VirtualBox VM Summary*  

<img width="1126" height="841" alt="Screenshot 2025-11-20 132823" src="https://github.com/user-attachments/assets/6a4d6c26-c866-49a0-8315-52df60f0a90e" />

 *Screenshot — VirtualBox Network Adapter Configuration*  

<img width="861" height="527" alt="Screenshot 2025-11-20 132656" src="https://github.com/user-attachments/assets/8d3c8dc4-8102-4f75-9622-05a38e5dc648" />

---

##  2. Domain Controller Setup (AD DS Installation and Promotion)

After the OS installation, I used **Server Manager → Add Roles and Features** to install:

- **Active Directory Domain Services (AD DS)**
- **DNS Server** (installed automatically with AD DS)

I then ran the **Post-Deployment Configuration Wizard** to promote this server to a **Domain Controller** for the new forest and domain:

> **Domain Name (FQDN):** `lab.local`  
> **Computer Name:** `LAB-DC01`  
> **Administrator Account Format:** `LAB\Administrator`

| Component | Value |
|-----------|-------|
| Computer Name | LAB-DC01 |
| Domain Name | lab.local |
| DNS Role | Installed with AD DS |
| Domain Functional Level | Windows Server 2019 |

---

###  Server Role Installation Confirmation

To validate successful configuration, I opened **Server Manager** to confirm that both **AD DS** and **DNS Server** roles were fully installed and active.

 *Screenshot — Server Manager showing AD DS and DNS roles installed*

<img width="1630" height="932" alt="Screenshot 2025-11-20 133351" src="https://github.com/user-attachments/assets/5aea71c2-19b9-47f9-b6bf-b9dd3f39490c" />

I also verified the **server identity and domain membership**, confirming the system is recognized as:

- Computer Name: LAB-DC01  
- Domain: lab.local  
- Assigned static IP: 10.0.2.10  
- Windows Server 2019 Standard Evaluation

 *Screenshot — Local Server Properties showing Domain, Hostname, and IP*

<img width="1214" height="347" alt="Screenshot 2025-11-20 133618" src="https://github.com/user-attachments/assets/e9d29238-02dc-4d33-a931-5e19cfe77bff" />

---

##  3. Static IP Configuration (Required for Domain Controllers)

Domain Controllers must use a **manually assigned static IP address** to ensure stable DNS responses and domain reliability.

| Setting | Value |
|---------|-------|
| IP Address | 10.0.2.10 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 10.0.2.2 |
| Preferred DNS | 10.0.2.10 |
| Alternate DNS | blank |

 *Screenshot — IPv4 static IP configuration on LAB-DC01*

<img width="401" height="450" alt="Screenshot 2025-11-20 141630" src="https://github.com/user-attachments/assets/b1cb6c8b-4872-41c6-b590-16040eac7651" />

---

##  4. Connectivity and DNS Resolution Testing

To ensure proper network communication and DNS functionality, I ran the following commands from Command Prompt:

| Test Command | Purpose | Expected Result |
|--------------|---------|------------------|
| ping 10.0.2.10 | Test local IP reachability | Success |
| ping lab.local | Test DNS resolution | Resolves to 10.0.2.10 |
| nslookup lab.local | Confirm DNS using DC | Shows DC as DNS server* |

 *Screenshot — Command Prompt showing successful ping but DNS timeout*

<img width="604" height="629" alt="Screenshot 2025-11-20 140916" src="https://github.com/user-attachments/assets/42fd5045-a8b2-435c-8af7-c0b7abf7002a" />

---


---

### 5. Reverse DNS Configuration (PTR Record and Reverse Lookup Zone)

While forward DNS resolution was working, the initial configuration did not include a Reverse Lookup Zone.  
This meant DNS could resolve **names to IP addresses**, but could not resolve **IP addresses back to hostnames**, which resulted in:

- `Server: Unknown` during nslookup  
- DNS timeout warnings  
- No PTR record for the Domain Controller  
- Missing AD, DHCP, and DNS health features  

To fix this, I manually created a Reverse Lookup Zone and added a PTR record.

---

#### Reverse Lookup Zone Setup

In **DNS Manager** → Reverse Lookup Zones → New Zone:

| Setting | Value |
|--------|-------|
| Zone Type | Primary Zone |
| Lookup Type | IPv4 Reverse Lookup Zone |
| Network ID | 10.0.2 |
| Dynamic Updates | Allow only secure dynamic updates |


 *Screenshot — Reverse Lookup Zone successfully created in DNS Manager before adding PTR record*
<img width="992" height="255" alt="Screenshot 2025-11-20 143114" src="https://github.com/user-attachments/assets/acaeeb21-947e-43c3-a9fd-2e33373fdd8b" />

---

#### PTR Record Creation (Domain Controller)

| Field | Value |
|-------|-------|
| Host IP (last octet) | 10 |
| Hostname (FQDN) | lab-dc01.lab.local |


 *Screenshot — Creating a PTR record for the Domain Controller, mapping 10.0.2.10 to hostname lab-dc01.lab.local in the Reverse Lookup Zone*
<img width="402" height="357" alt="Screenshot 2025-11-20 143524" src="https://github.com/user-attachments/assets/e4e63ad5-a427-4f20-928e-90210f7e6aeb" />

---

#### DNS Reverse Lookup Validation

To verify correct reverse DNS resolution, I ran:

    nslookup 10.0.2.10

Successful output:

    Server: lab-dc01.lab.local
    Address: 10.0.2.10

    Name: lab-dc01.lab.local
    Address: 10.0.2.10


 *Screenshot — Successful ping and nslookup confirming DNS resolution and correct server identity*
<img width="571" height="524" alt="Screenshot 2025-11-20 144003" src="https://github.com/user-attachments/assets/5a72af33-2576-48a9-8ab0-d1814a1cb6f7" />

---

#### Why This Matters

Reverse DNS (PTR records) enables:

- Domain join and secure DNS registration  
- Active Directory health (dcdiag, nltest)  
- DHCP dynamic updates  
- Clean hostname display in nslookup and monitoring tools  

---

With both Forward and Reverse Lookup Zones correctly configured, DNS is now fully functional for domain operations and ready for Milestone 2 client domain joins.

---


##  Milestone Outcome

By the end of this milestone, I successfully:

✔ Built a Windows Server 2019 VM  
✔ Promoted it to a Domain Controller  
✔ Created the Active Directory `lab.local` domain  
✔ Installed and confirmed DNS services  
✔ Configured and verified static IP addressing  
✔ Confirmed network and DNS functionality using `ping` and `nslookup`  
✔ Domain Controller is now functional for domain operations  

---

##  Next Steps (Milestone 2)

| Task | Purpose |
|------|---------|
| Create and configure Windows 10 Client VM | Build client-side domain participant |
| Join client to `lab.local` domain | Test authentication and trust |
| Use DNS from Domain Controller | Confirm internal DNS resolution |
| Document process in GitHub | Continue portfolio documentation |

---

>  *Note:* Some screenshots were captured after initial setup, based on the current system state. All settings shown reflect the actual configuration applied during this milestone.

