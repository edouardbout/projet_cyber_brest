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

## Why Nodes Matter in Cybersecurity

Understanding these objects is critical because attackers often:
- Compromise **users** to gain access

- Abuse **groups** for privilege escalation

- Move laterally between **computers**

- Exploit **GPOs** to deploy malicious configurations

- Navigate through **OUs and domains** to reach critical assets

In the next section, we will explore **edges**, which define how these nodes are connected and how attacks propagate through the system.