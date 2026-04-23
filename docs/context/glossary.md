# Glossary

This page introduces the core concepts of an Active Directory (AD) environment.

In our project, we model Active Directory as a **graph**:

- **Nodes** represent objects (users, computers, groups, etc.)

- **Edges** represent relationships between these objects (permissions, memberships, etc.)

---

## Active Directory Nodes

**Analogy**: Think of AD as a **building complex** that contains everything : people, rooms, rules, and security systems.


| Node | Simple Definition | Concrete Example | Building Analogy |
|------|------------------|------------------|------------------|
| **Domain** | A logical boundary grouping users, computers, and policies under common rules | `corp.local` domain for all employees | A **building** within a larger complex |
| **Organizational Unit (OU)** | A container used to organize users, groups, and computers | OU "HR" containing HR employees | A **floor or department** |
| **Group** | A collection of users or computers with shared permissions | "IT Admins" group managing servers | A **team with shared access badges** |
| **User** | An individual account representing a person | Employee account `jdoe` | A **person in the building** |
| **Computer** | A device joined to the domain | Company laptop connected to the network | A **workstation or office desk** |
| **Group Policy Object (GPO)** | A set of rules applied to users or computers | Enforcing strong password policies | The **building rules** |

---

## Why nodes matter in cybersecurity ?

Understanding these objects is critical because attackers often:
- Compromise **users** to gain access

- Abuse **groups** for privilege escalation

- Move laterally between **computers**

- Exploit **GPOs** to deploy malicious configurations

- Navigate through **OUs and domains** to reach critical assets

In the next section, we will explore **edges**, which define how these nodes are connected and how attacks propagate through the system.

## 🔗 Active Directory Edges (Relationships)

In an Active Directory graph, **edges represent relationships between nodes**.  
They define **who can access what**, **who controls whom**, and ultimately **how an attacker can move inside the system**.


| Category | Edge | Simple Definition | Concrete Example | Building Analogy |
|----------|------|------------------|------------------|------------------|
| **Domain/OU → OU, Domain/OU → Security Principal** | **Contains** | Defines a hierarchical relationship where a container holds objects | A Domain contains an OU, or an OU contains users and computers | A **building contains floors**, and floors contain rooms |
| **Domain → Domain** | **TrustedBy** | Indicates that one domain trusts another for authentication | Domain A trusts Domain B, allowing users from B to access A | Two **buildings with a shared access agreement** |
| **Security Principal → Group** | **MemberOf** | A user or computer belongs to a group | User "jdoe" is a member of "IT Admins" | A **person is part of a team** |
| **Security Principal → Group** | **AddMember** | Permission to add members to a group | A user can add others to the "Admins" group | Someone who can **grant access badges to a team** |
| **Security Principal → Computer** | **AdminTo** | Full administrative control over a computer | A user has admin rights on a server | A **master key holder** for a room |
| **Security Principal → Computer** | **CanPSRemote** | Ability to execute commands remotely via PowerShell | Admin remotely runs commands on a machine | A **remote control panel** for a room |
| **Security Principal → Computer** | **CanRDP** | Ability to log in remotely via Remote Desktop | User connects to a server using RDP | A **remote entry door with full access** |
| **Security Principal → Computer** | **ExecuteDCOM** | Ability to execute remote commands via DCOM | Attacker executes code remotely on a machine | A **hidden maintenance access system** |
| **User → Computer** | **HasSession** | A user currently has an active session on a computer | Admin logged into a workstation | A **person currently inside a room** |
| **ACLs** | **AllExtendedRights** | Broad special permissions on an object | User has advanced control over another account | A **special override permission on a door** |
| **ACLs** | **AddAllowedToAct** | Can configure delegation rights on a computer | Attacker allows a machine to act on behalf of others | Ability to **authorize someone to act for others** |
| **ACLs** | **AllowedToAct** | A machine can act on behalf of another (delegation) | Service account impersonates users | A **proxy badge allowing impersonation** |
| **ACLs** | **AllowedToDelegate** | Can delegate credentials to other services | Account allowed to reuse credentials elsewhere | A **trusted courier allowed to carry identities** |
| **ACLs** | **ForceChangePassword** | Can reset another user’s password | Attacker resets admin password | Ability to **change someone’s badge access code** |
| **ACLs** | **GenericAll** | Full control over an object | Full control over a user or computer | A **master key + full authority** |
| **ACLs** | **GenericWrite** | Can modify attributes of an object | Modify user properties (e.g., add to group) | Ability to **edit someone's profile or permissions** |
| **ACLs** | **GetChanges** | Can read directory changes (basic replication) | Read AD changes for reconnaissance | Access to **security logs or updates** |
| **ACLs** | **GetChangesAll** | Can read all directory changes (including sensitive data) | Extract password hashes (DCSync attack) | Access to **all security records including secrets** |
| **ACLs** | **Owns** | Ownership of an object (full control potential) | User owns a group or object | Being the **legal owner of a room** |
| **ACLs** | **WriteDacl** | Can modify access permissions (ACL) | Change who has access to an object | Ability to **edit the access list of a room** |
| **ACLs** | **WriteOwner** | Can change ownership of an object | Take ownership of a resource | Ability to **transfer property ownership** |
| **GPO → Domain/OU** | **GpLink** | Links a GPO to a Domain or OU | A GPO applied to all computers in an OU | Applying **building-wide rules to a floor** |

---

In cybersecurity, **edges are what make attacks possible**:
- They define **attack paths**
- They enable **privilege escalation**
- They allow **lateral movement**