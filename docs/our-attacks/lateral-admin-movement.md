# Lateral Admin Chain Attack

## Definition

A Lateral Admin Chain refers to an attack path in Active Directory where an attacker moves from one machine to another by leveraging **local administrative privileges**, eventually reaching a high-value target.

In this project, we use the following operational definition:

> A Lateral Admin Chain is a path starting from a User or Computer, containing multiple `AdminTo` or remote access relations, allowing step-by-step lateral movement across machines.

---

## Theoretical Background and real use

In real Active Directory environments, attackers rarely gain domain-wide privileges immediately. Instead, they perform **lateral movement**, pivoting from one system to another by abusing local administrative rights.

Machines often trust multiple administrators, and users may have admin rights on several computers. This creates a network of implicit trust relationships.

### Lateral Movement via Admin Rights

If an attacker compromises a machine, they can:
- Extract credentials from memory (e.g., LSASS)
- Reuse tokens or hashes
- Access other machines where the compromised user has admin rights

Key relations include:
- `AdminTo` (local admin rights on a computer)
- `HasSession` (user session present on a machine)
- Remote access capabilities (`CanRDP`, `CanPSRemote`, `ExecuteDCOM`)

These allow an attacker to move laterally without escalating privileges immediately.

### Real-World Context

Such attack paths emerge due to:
- Reuse of admin accounts across machines  
- Overlapping administrative privileges  
- Users logging into multiple systems  
- Lack of segmentation between critical and non-critical assets  

This creates **chains of reachable systems**.

### Lateral Admin Chain Concept

A Lateral Admin Chain is therefore:

> A sequence of systems where administrative access on one leads to control over another.

This reflects real-world attacker behavior:
- Compromise a machine  
- Move laterally  
- Reach a more privileged context  

---

## Translation in the Simulator

In the graph model:
- Nodes represent Users and Computers  
- Edges represent administrative or access relationships  

A Lateral Admin Chain corresponds to a path where:
- Multiple `AdminTo` or remote access edges are chained  
- The attacker progressively gains control over new machines  

---

## Security Impact

Lateral Admin Chains are dangerous because:
- They allow stealthy progression through the network  
- They do not require immediate privilege escalation  
- They often rely on legitimate administrative configurations  

They represent a **realistic and common attack pattern** in Active Directory environments.

---

## Summary

- Based on **lateral movement between machines**  
- Uses **local admin privileges and sessions**  
- Does not require direct access to privileged groups  
- Exploits **trust relationships between systems**  
- Critical for reaching high-value targets indirectly