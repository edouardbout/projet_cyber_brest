# Shadow Admin Attack

## Definition

A Shadow Admin attack refers to any attack path in Active Directory where a non-privileged entity can effectively gain administrative control without being explicitly assigned to an administrative group.

In this project, we use a simplified and operational definition:

> A Shadow Admin path is a path starting from a User or Computer, ending at a privileged group, containing at least one ACL-based control relation and at least one membership-related relation (MemberOf or AddMember).

---

## Theoretical Background

In Active Directory, privileges are not only determined by group membership. Control over objects can also be obtained through Access Control Lists (ACLs).

Key ACL-based permissions include:
- GenericAll  
- WriteDACL  
- WriteOwner  
- GenericWrite  

These permissions allow an attacker to:
- Modify group memberships  
- Change security descriptors  
- Take ownership of objects  
- Indirectly escalate privileges  

Thus, a user does not need to be a member of a privileged group to control it.

---

## Translation in the Simulator

In the simulation pipeline, the environment is represented as a graph composed of:
- Nodes: Users, Computers, Groups  
- Edges: Relationships such as ACL permissions and memberships  

A Shadow Admin is therefore not a specific node type, but a **path pattern** in the graph.

A valid Shadow Admin path must:
1. Start from a User or Computer  
2. End at a privileged group (e.g., Domain Admins)  
3. Include at least one ACL-based edge  
4. Include at least one membership-related edge (MemberOf or AddMember)  



In this example:
- User_A is not a member of Domain Admins  
- But can modify Group_B through an ACL  
- Which allows adding members to Domain Admins  

User_A is therefore a Shadow Admin.

---

## Interpretation in the Model

Within the generated graphs:
- Sources represent compromised users or entry points  
- Targets represent privileged groups such as Domain Admins  

A Shadow Admin corresponds to:
- A short and feasible path from a source to a privileged target  
- A path combining control (ACL) and privilege propagation (membership)  

Such paths are especially critical when:
- The distance is small (e.g., 1 or 2 steps)  
- The permissions involved allow direct control over groups  

```python
ACL_RELATIONS = {
    "GenericAll",
    "GenericWrite",
    "WriteDACL",
    "WriteOwner",
    "AllExtendedRights",
    "ForceChangePassword",
    "AddKeyCredentialLink"
}

MEMBERSHIP_RELATIONS = {
    "MemberOf",
    "AddMember"
}

PRIVILEGED_GROUPS = {
    "DOMAIN ADMINS@INSTANCE0.LOCAL",
    "ENTERPRISE ADMINS@INSTANCE0.LOCAL",
    "ADMINISTRATORS@INSTANCE0.LOCAL",
    "ACCOUNT OPERATORS@INSTANCE0.LOCAL",
    "SERVER OPERATORS@INSTANCE0.LOCAL",
    "BACKUP OPERATORS@INSTANCE0.LOCAL",
    "PRINT OPERATORS@INSTANCE0.LOCAL"
}

```



## Detection Strategy

A Shadow Admin can be identified by searching for paths that satisfy the definition.

Basic approach:
- Compute shortest paths from Users/Computers to privileged groups  
- Filter paths that:
  - contain at least one ACL relation  
  - contain at least one MemberOf or AddMember relation  

This turns the problem into a constrained path search in the graph.


---

## Example Diagram

![Shadow Admin Path](../assets/shadowtest.png)

---

## Security Impact

Shadow Admin paths are particularly dangerous because:
- They bypass traditional privilege checks  
- They are not visible through group membership alone  
- They enable stealthy and fast privilege escalation  

They represent hidden control structures in Active Directory that are often overlooked.

---

## Summary

- A Shadow Admin is defined as a **path**, not a role  
- It combines **ACL control + membership escalation**  
- It exists even when no direct admin membership is present  
- It can be detected through graph path analysis  
- It represents a critical attack vector in AD environments  




