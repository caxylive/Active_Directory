<a name="top"></a>

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

### 🔹 Key Features:
- Centralized user and computer management
- Authentication using **Kerberos and NTLM**
- Group Policy enforcement
- Integration with **cloud services (Azure AD)**

---

## Active Directory Structure
Active Directory consists of multiple layers, which help organize and manage network resources efficiently.

### 🔹 **Logical Structure**
- **Forest** → The highest-level structure that contains multiple domains.
```ascii
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
- **Domain** → A logical group of users, computers, and objects that share a security boundary.
- **Organizational Units (OUs)** → Subdivisions within a domain that help organize objects (e.g., Users, Groups, Computers).
- **Objects** → Individual entities like users, computers, and printers.

### 🔹 **Physical Structure**
- **Domain Controllers (DCs)** → Servers that store AD data and handle authentication.
- **Sites** → Physical locations with different network connections.
- **Replication** → Synchronization of AD data between domain controllers.

[Back to Top](#top)

---

## Active Directory Components
### 🔹 **Domain Controllers (DCs)**
- Store and manage the Active Directory database (`NTDS.dit`).
- Handle authentication and authorization.

### 🔹 **Group Policy Objects (GPOs)**
- Used to enforce security settings, configurations, and restrictions on users and computers.
- Examples:
  - **Enforcing password complexity**
  - **Restricting software installation**
  - **Mapping network drives**

### 🔹 **FSMO Roles (Flexible Single Master Operations)**
Active Directory has **five FSMO roles** responsible for managing critical operations:

1. **Schema Master** – Controls schema changes (data structure of AD).
2. **Domain Naming Master** – Manages domain additions/removals.
3. **RID Master** – Allocates security identifiers (SIDs) for objects.
4. **PDC Emulator** – Handles password changes, time sync, and legacy authentication.
5. **Infrastructure Master** – Manages inter-domain object references.

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
