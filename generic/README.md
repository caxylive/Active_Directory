<a name="top"></a>
[Active Directory for Cybersecurity](https://github.com/caxylive/Active_Directory/blob/main/cybersecurity/README.md)

# Active Directory (AD) – Detailed Guide

## Table of Contents
1. [Introduction to Active Directory](#introduction-to-active-directory)
2. [Active Directory Structure](#active-directory-structure)
3. [Active Directory Components](#active-directory-components)
4. [Active Directory Security & Hardening](#active-directory-security--hardening)
5. [Active Directory Federation & Hybrid Integration](#active-directory-federation--hybrid-integration)
6. [Active Directory Management & Troubleshooting](#active-directory-management--troubleshooting)
7. [Common AD Attacks & Defenses](#common-ad-attacks--defenses)
8. [Why AD Matters for IT & Cybersecurity](#why-ad-matters-for-it--cybersecurity)

---

## Introduction to Active Directory
Active Directory (AD) is a **centralized directory service** developed by Microsoft for **user authentication, authorization, and resource management** in Windows-based environments. It is essential for managing users, computers, and security policies in corporate networks.

### 🔹 **Key Features**:
- Centralized user and computer management
- Authentication using **Kerberos and NTLM**
- Group Policy enforcement
- Integration with **cloud services (Azure AD)**

### **Accounting**:
- The industry widely uses **RADIUS** for network authentication and accounting, especially for remote access scenarios like **VPNs** and **Wi-Fi**. It's considered the standard due to its compatibility with most network devices and its ability to integrate seamlessly with Active Directory.
- For managing **administrative access to network devices**, TACACS+ is still a popular choice because of its **granular control over authorization** and detailed accounting capabilities.
- Additionally, many organizations are adopting **cloud-based identity and access management** (**IAM**) solutions, such as **Azure Active Directory** or **Okta**, which provide advanced features like multi-factor authentication (**MFA**), single sign-on (**SSO**), and detailed activity logs for accounting.
- For **network traffic monitoring and accounting**, protocols like **NetFlow** and **sFlow** are commonly used, alongside tools like **SolarWinds** or **Splunk** for detailed analysis and reporting.

---

## Active Directory Structure
Active Directory consists of multiple layers, which help organize and manage network resources efficiently.

### 🔹 **Logical Structure**
* **Forest** → The highest-level structure that contains multiple domains.
``` Example
                      [Forest Root Domain]
                               |
       -------------------------------------------------
      |                        |                       |
[Domain A]               [Domain B]              [Domain C]
      |                        |                       |
[OU (A1)]                 [OU (B1)]               [OU (C1)]
      |                        |                       |
[Users, Computers]    [Users, Computers]     [Users, Computers]

```
* **Domain**
  * A logical group of users, computers, and objects that share a security boundary.
  * Separate entities in the forest
  * They can represent different regions, departments, or any organizational unit
  * They are linked by a trust relationship
* **Organizational Units (OUs)**
  * Subdivisions within a domain that help organize objects (e.g., Users, Groups, Computers).
* **Objects** → Individual entities like users, computers, and printers.
* **Shared Schema**
  * All domains within a forest share the same schema ; they follow the same rules for data organization.

### 🔹 **Physical Structure**
```ascii
                          [Site 1]
                             |
       -----------------------------------------------
      |                      |                        |
[DC1 (Domain A)]        [DC2 (Domain A)]         [DC3 (Domain B)]
      |                      |                        |
 Replication <---------> Replication <---------> Replication
                             |
                         [WAN Link]
                             |
                         [Site 2]
                             |
       --------------------------------------------
      |                      |                     |
[DC4 (Domain C)]      [DC5 (Domain C)]      [DC6 (Domain D)]

```

- **Domain Controllers (DCs)** → Servers that store AD data and handle authentication.
- **Sites** → Physical locations with different network connections.
- **Replication** → Synchronization of AD data between domain controllers.
  * Intra-site
    * Within the same site
    * Between DCs like DC1, DC2, and DC3
    * Frequent and optimized for speed
  * Intersite
    * Across sites
    * Over WAN links like between Site 1 and Site 2
    * Uses compression to save bandwidth

[Back to Top](#top)

---

## Active Directory Components
### 🔹 **Domain Controllers (DCs)**
- Store and manage the Active Directory database (`NTDS.dit`).
- Handle authentication and authorization.
- `NTDS.dit
  - It is the **Active Directory database file** where all directory data is stored.
  - Stores user group information, computer accounts, security groups, attributes, and directory schema.
  - Also contains password hashed. Therefore, is accessed only by authorized personnel.
  - By default, it resides in the `%SystemRoot%\NTDS\` folder on a Domain Controller (e.g., `C:\Windows\NTDS\NTDS.dit`).
  - Uses a **Jet (Joint Engine Technology) Database Engine** to manage data efficiently.
    - Lightweight and powerful engine primarily used for managing and accessing structured data.
    - Ensures efficient storage and retrieval of directory data.

### 🔹 **Group Policy Objects (GPOs)**
- Used to enforce security settings, configurations, and restrictions on users and computers.
- Applied to **Organizational Units** (**OUs**), sites, or domains, and they affect all objects (e.g., users and computers) within those containers.
- Examples:
  - **Setting a Desktop Wallpaper**
  - **Enforcing Password Policies**
  - **Deploying Software via GPO**
  - **Mapping network drives**
  - **Blocking USB Devices Except Specific Ones**

### 🔹 **FSMO Roles (Flexible Single Master Operations)**
Active Directory has **five FSMO roles** responsible for managing critical operations:

1. **Schema Master**
    * Controls schema changes (data structure of AD).
    * Ensures schema changes are replicated to all DCs

2. **Domain Naming Master**
    * Ensures unique naming of domains across the forest.
    * Handles adding or removing domains from the forest.

3. **RID Master**
    * Allocates unique pools of Relative Identifiers (RIDs) to Domain Controllers in the domain.
    * RIDs are used to create unique Security Identifiers (SIDs) for objects.
    * If a user is deleted and another user is created with the same name, they will have different SIDs because the RID part will be unique. This ensures that permissions assigned to the old user don't automatically transfer to the new one.

4. **PDC Emulator** – Handles password changes, time sync, and legacy authentication.
    * Acts as a Primary Domain Controller (PDC) for legacy systems.
    * Handles password changes and account lockout.
    * Synchronizes time for all devices in the domain.

5. **Infrastructure Master** – Manages inter-domain object references.
    * Updates cross-domain object references, like when a user in one domain is a member of a group in another domain.

### 🔹 **Read-Only Domain Controllers (RODCs)**
- Provide **read-only copies** of AD for **remote offices** or **low-security locations**.
- Prevent unauthorized changes to AD.

[Back to Top](#top)

---

## Active Directory Security & Hardening
### 🔹 **Authentication Protocols**
- **Kerberos Authentication** – Secure, ticket-based authentication method.
- **NTLM Authentication (Legacy)** – Older and **vulnerable to pass-the-hash attacks**.

### 🔹 **Enabling Recycle Bin**
- Allows recovery of accidentally deleted objects.
- Can be enabled via **PowerShell** or the **Active Directory Admin Center**.

### 🔹 **Best Practices for AD Security**
- **Implement Least Privilege** – Restrict admin access.
- **Enable Multi-Factor Authentication (MFA)**.
- **Use LAPS (Local Administrator Password Solution)** – Secures local admin credentials.
- **Enable SMB signing** – Prevents relay attacks.
- **Monitor Logon Events** – Use Windows Event Viewer (`Event ID 4624, 4740, etc.`).

[Back to Top](#top)

---

## Active Directory Federation & Hybrid Integration
### 🔹 **Azure AD vs. On-Prem AD**
| Feature         | On-Prem AD | Azure AD |
|----------------|-----------|----------|
| Authentication | Kerberos, NTLM | OAuth, SAML, OpenID |
| Management     | Group Policies | Conditional Access Policies |
| Infrastructure | Physical DCs | Cloud-based |

### 🔹 **Active Directory Federation Services (ADFS)**
- Provides **Single Sign-On (SSO)** for hybrid cloud environments.

### 🔹 **LDAP & Active Directory Lightweight Directory Services (AD LDS)**
- **LDAP (Lightweight Directory Access Protocol)** → Allows external apps to interact with AD.
- **AD LDS** → A lightweight directory service without a full domain setup.

[Back to Top](#top)

---

## Active Directory Management & Troubleshooting
### 🔹 **Backup & Recovery**
- **Use Windows Server Backup** or third-party tools for AD backup.
- **Authoritative vs. Non-Authoritative Restore**:
  - **Authoritative Restore** → Restores deleted objects and prevents them from being overwritten.
  - **Non-Authoritative Restore** → Restores AD but allows normal replication.

### 🔹 **Monitoring & Auditing**
- **Enable auditing policies** to track logins, permission changes, and object modifications.
- **Tools for AD monitoring**:
  - **Event Viewer (Security Logs)** → Tracks failed/successful logins.
  - **SIEM Solutions (Splunk, Graylog, ELK)** → Advanced log analysis.

[Back to Top](#top)

---

## Common AD Attacks & Defenses
### 🔹 **Common Attacks**
1. **Pass-the-Hash (PTH)** → Attackers steal NTLM password hashes and use them to access systems.
2. **Golden Ticket Attack** → Attackers forge Kerberos tickets using stolen credentials.
3. **Lateral Movement** → Attackers move from one system to another inside a network.

### 🔹 **Mitigation Techniques**
- **Use strong password policies** to prevent easy cracking.
- **Enable logging and auditing** to detect suspicious activity.
- **Implement tiered admin accounts** (separate daily-use and admin accounts).
- **Monitor Kerberos ticket requests** for anomalies.

[Back to Top](#top)

---

## Why AD Matters for IT & Cybersecurity
### 🔹 **For IT Professionals**
- AD is the backbone of **enterprise network management**.
- Essential for **cloud computing (Azure AD)** and hybrid environments.

### 🔹 **For Ethical Hackers & Cybersecurity**
- AD **security vulnerabilities** are major attack targets.
- **Pentesting AD environments** is a key skill in Red Teaming.
- Understanding **common attack techniques** helps in **defensive security**.

[Back to Top](#top)

---

## Conclusion
Active Directory is a **fundamental component** of enterprise IT infrastructure. Understanding its **logical & physical structure, security risks, and hybrid integration** is critical for **IT admins, cybersecurity professionals, and ethical hackers**.  

Would you like a more in-depth guide on **AD security and penetration testing techniques**? 🚀

[Back to Top](#top)

---
