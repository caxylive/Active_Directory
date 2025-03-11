# Active Directory Domain Services (AD DS) Notes

## AD DS Forests and Domains

### What is an AD DS Forest?
An AD DS forest is a collection of one or more AD DS trees that contain one or more AD DS domains. Domains in a forest share:
- A common root.
- A common schema.
- A global catalog.

A forest is a top-level container in AD DS. Each forest is a collection of one or more domain trees that share a common directory schema and a global catalog. The forest root domain is the first domain created in the forest.

The forest root domain contains objects that don't exist in other domains in the forest, such as:
- The schema master role.
- The domain naming master role.
- The Enterprise Admins group.
- The Schema Admins group.

An AD DS forest is often described as:
- **A security boundary** – No users from outside the forest can access resources inside the forest by default. All domains within a forest automatically trust each other.
- **A replication boundary** – The forest is the replication boundary for the configuration and schema partitions in the AD DS database and the global catalog.

### What is an AD DS Domain?
An AD DS domain is a logical container for managing user, computer, group, and other objects. Each domain controller stores a copy of the AD DS database. 

Common domain objects include:
- **User accounts** – Contain user authentication details.
- **Computer accounts** – Represent domain-joined computers.
- **Groups** – Organize users and computers for easier management.

An AD DS domain is often described as:
- **A replication boundary** – Changes to objects replicate across all domain controllers within the domain.
- **An administrative unit** – Each domain has a built-in Administrator account and a Domain Admins group with full control over domain objects.

### Trust Relationships
AD DS trusts allow access to resources across multiple domains or forests. 

Types of trusts:

| Trust Type            | Transitivity | Direction | Description |
|-----------------------|-------------|-----------|-------------|
| Parent and child     | Transitive   | Two-way   | Created when adding a new domain to an existing tree. |
| Tree-root           | Transitive   | Two-way   | Created when adding a new tree to an existing forest. |
| External            | Nontransitive | One-way or Two-way | Connects to an AD DS domain in another forest or an NT 4.0 domain. |
| Realm               | Transitive or Nontransitive | One-way or Two-way | Links an AD DS domain with a Kerberos v5 realm. |
| Forest              | Transitive   | One-way or Two-way | Enables resource sharing across forests. |
| Shortcut            | Nontransitive | One-way or Two-way | Reduces authentication time between domains in different parts of a forest. |

## Organizational Units (OUs)

### What is an OU?
An Organizational Unit (OU) is a container object within a domain used to:
- Consolidate users, computers, groups, and other objects.
- Apply Group Policy Objects (GPOs) directly to contained objects.
- Delegate administrative control.

### Why Create OUs?
1. **Group objects for easier management** – Apply GPOs to OUs to enforce policies on users and computers.
2. **Delegate administrative control** – Assign specific users or groups permission to manage OU objects.

OUs can represent organizational structures such as:
- Departments
- Geographic locations
- Functional roles

### Generic Containers in AD DS
AD DS includes several built-in containers that store system objects or serve as default locations for newly created objects. Unlike OUs, containers have limited management capabilities and cannot have GPOs applied to them.

**Default AD DS Containers:**
- **Domain** – Top level of the domain hierarchy.
- **Builtin** – Stores default security groups.
- **Computers** – Default location for new computer accounts.
- **Foreign Security Principals** – Stores trusted objects from external domains.
- **Managed Service Accounts** – Stores managed service accounts with automatic password management.
- **Users** – Default location for new user accounts and groups.
- **Domain Controllers** – Default location for domain controllers.

**Advanced Containers (Hidden by Default):**
- **LostAndFound** – Stores orphaned objects.
- **Program Data** – Holds AD data for Microsoft applications.
- **System** – Stores built-in system settings.
- **NTDS Quotas** – Holds directory service quota data.
- **TPM Devices** – Stores recovery information for Trusted Platform Module (TPM) devices.

### OU Hierarchical Design
Organizations should design their OU structure based on administrative needs. Best practices include:
- Limiting the OU hierarchy to **five levels or fewer** for manageability.
- Using **geographic, departmental, or functional structures** to organize objects.
- Placing computers with similar configuration needs into the same OUs for efficient policy application.

**Example OU Structure:**
```
Contoso.com
│
├── Corporate (OU)
│   ├── IT (OU)
│   ├── HR (OU)
│   ├── Finance (OU)
│
├── BranchOffice (OU)
    ├── IT (OU)
    ├── Sales (OU)
```

### OU Management Tools
You can create OUs using:
- **Active Directory Administrative Center**
- **Active Directory Users and Computers**
- **Windows Admin Center**
- **Windows PowerShell** with the Active Directory module

### Key Notes
- **GPOs cannot be applied to containers**, only to OUs.
- **OUs allow better administrative delegation and organization** than generic containers.
- **A well-planned OU structure simplifies management and policy enforcement.**

