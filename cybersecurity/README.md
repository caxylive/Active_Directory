[Active Directory - Generic](https://github.com/caxylive/Active_Directory/blob/main/generic/README.md)

# Active Directory (AD)

## Table of Contents
1. [Introduction to Active Directory](#introduction-to-active-directory)
2. [Active Directory Structure & Security Risks](#active-directory-structure--security-risks)
3. [Authentication Protocols & Weaknesses](#authentication-protocols--weaknesses)
4. [AD Hardening & Security Best Practices](#ad-hardening--security-best-practices)
5. [Common AD Attacks & Defense Strategies](#common-ad-attacks--defense-strategies)
6. [Active Directory Monitoring & Incident Response](#active-directory-monitoring--incident-response)
7. [AD Pentesting & Red Teaming Techniques](#ad-pentesting--red-teaming-techniques)
8. [Why AD Matters for Cybersecurity](#why-ad-matters-for-cybersecurity)

---

## Introduction to Active Directory
Active Directory (AD) is the **core identity and access management system** in most enterprise networks. Because it controls **authentication, authorization, and security policies**, it is a prime target for attackers.

### 🔹 Why is AD critical for cybersecurity?
- **A compromised AD = Full network control.**
- Attackers use AD misconfigurations to escalate privileges.
- AD attacks like **Pass-the-Hash, Kerberoasting, and Golden Ticket** are commonly exploited in real-world breaches.

---

## Active Directory Structure & Security Risks
AD consists of **logical** and **physical** components, each with potential security risks.

### 🔹 **Logical Structure & Attack Surfaces**
- **Forest & Domains** → A compromise at the **forest root** can impact all domains.
- **Organizational Units (OUs)** → Poor access control on OUs can lead to unauthorized GPO modifications.
- **Objects (Users, Groups, Computers, GPOs, etc.)** → Overprivileged accounts increase attack surface.

### 🔹 **Physical Structure Risks**
- **Domain Controllers (DCs)** → If an attacker gains access to a DC, they own the network.
- **Replication & Trusts** → Attackers abuse replication (DCShadow attack) to inject fake domain controllers.

#### ⚠️ **Security Risks in AD Design**
1. **Excessive Admin Privileges** → Attackers escalate privileges easily.
2. **Weak Password Policies** → Enables brute-force and credential-stuffing attacks.
3. **Unrestricted Kerberos Delegation** → Leads to Kerberoasting & ticket attacks.
4. **Unmonitored Service Accounts** → Often have excessive permissions and weak passwords.

---

## Authentication Protocols & Weaknesses
AD primarily uses **Kerberos** and **NTLM** for authentication, both of which have known vulnerabilities.

### 🔹 **Kerberos Authentication & Attacks**
- Uses **TGT (Ticket Granting Ticket)** & **TGS (Service Tickets)** for authentication.
- **Golden Ticket Attack** → Attackers forge TGTs to gain indefinite domain access.
- **Kerberoasting** → Extracts **service account** hashes for offline cracking.

### 🔹 **NTLM Authentication (Legacy & Insecure)**
- Susceptible to **Pass-the-Hash (PTH)** and **NTLM Relay Attacks**.
- Does **not use encryption**, making it easy to capture and replay credentials.
- **Mitigation**: Disable NTLM when possible and enforce **Kerberos-only authentication**.

---

## AD Hardening & Security Best Practices
### 🔹 **Group Policy & Access Control Hardening**
- **Enforce the principle of least privilege** (POLP).
- Restrict **who can modify Group Policies (GPOs)**.
- Disable **unnecessary features like LLMNR and Ne
