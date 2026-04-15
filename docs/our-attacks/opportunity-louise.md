# Opportuniste Louise Attack

## Definition

The Opportuniste Louise Attack models an attacker that navigates an Active Directory environment **without a predefined objective or strategy**, by simply following available relationships until reaching a valuable target.

In this project, we define it as:

> A random path starting from a User or Computer, following existing AD relationships step by step, stopping when a high-value target is reached.

---

## Theoretical Background

In real Active Directory environments, attackers do not always operate with perfect knowledge of the infrastructure. Especially in early stages, they often act **opportunistically**, leveraging whatever access or relationship is immediately available.

Active Directory inherently exposes a rich set of relationships:
- Group memberships (`MemberOf`)
- Administrative rights (`AdminTo`)
- Sessions (`HasSession`)
- Remote access capabilities (`CanRDP`, `CanPSRemote`, `ExecuteDCOM`)
- ACL-based permissions  

These relationships form a **connected graph of reachable objects**, where each step can potentially reveal new opportunities.

### Opportunistic Movement

Instead of targeting a specific path, an attacker may:
- Compromise a user or machine  
- Enumerate accessible objects  
- Move to any reachable node (user, group, computer)  
- Repeat until a valuable asset is encountered  

This behavior reflects a **trial-and-error exploration** of the environment.

### Real-World Context

This type of behavior is common when:
- The attacker has limited visibility  
- No full graph knowledge is available  
- Automated tools or scripts explore the environment  
- The attacker relies on enumeration rather than planning  

It is especially relevant in:
- Early intrusion phases  
- Post-exploitation discovery  
- Automated attack simulations  

### Opportunistic Attack Concept

The Opportuniste Louise Attack is therefore:

> A non-deterministic exploration of Active Directory, where each step follows an existing relationship without prioritization.

It reflects a realistic attacker mindset:
- No optimization  
- No shortest path  
- Only **reachable actions at each step**

---

## Translation in the Simulator

In the graph model:
- Nodes represent AD objects  
- Edges represent all possible relationships  

The attack is simulated as:
- A random walk starting from a User or Computer  
- At each step, selecting a random outgoing edge  
- Stopping when an interesting target is reached  

Targets include:
- Privileged groups  
- Domain objects  
- Critical accounts or machines  

---

## Detection Strategy

Unlike structured attacks, this pattern is not defined by specific relations, but by behavior:

- Start from realistic entry points (Users/Computers)  
- Follow valid edges only  
- Observe paths that eventually reach high-value targets  

Detection focuses on:
- Reachability of sensitive assets  
- Frequency of successful random paths  
- Distance between entry points and targets  

---

## Security Impact

This attack highlights that:

- Even without strategy, sensitive targets may be reachable  
- The AD graph structure itself enables escalation  
- Many environments are overly connected  

It demonstrates the **intrinsic attack surface** of Active Directory.

---

## Summary

- Models a **realistic opportunistic attacker**  
- No predefined path or optimization  
- Based entirely on **existing AD relationships**  
- Reveals hidden reachability in the graph  
- Useful to evaluate overall exposure rather than specific misconfigurations