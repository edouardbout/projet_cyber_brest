# Shortest Path Attack

## Definition

A Shortest Path Attack models an attacker that follows the **minimum number of steps** required to reach a high-value target in Active Directory.

In this project, we define it as:

> The shortest valid path between a source node (User or Computer) and an interesting target in the AD graph.

---

## Theoretical Background

In real Active Directory environments, attackers often try to identify the **most efficient privilege escalation route**. Instead of exploring randomly, they look for the path that minimizes the number of actions needed to reach a privileged object.

This logic is commonly used in attack graph analysis tools such as BloodHound, where the objective is to identify the **shortest control path** from a compromised principal to a critical target.

### Shortest Privilege Escalation

A shortest path does not correspond to a specific attack primitive.  
It is a **graph-based view of attacker efficiency**.

The path may combine:
- Group memberships (`MemberOf`)
- ACL permissions (`GenericAll`, `WriteDACL`, `WriteOwner`, `GenericWrite`)
- Sessions (`HasSession`)
- Administrative rights on computers (`AdminTo`)
- Remote access capabilities (`CanRDP`, `CanPSRemote`, `ExecuteDCOM`)

The attacker’s goal is not to follow a specific strategy, but to reach the target with as few steps as possible.

### Real-World Context

This reflects realistic attacker behavior when:
- The environment has already been enumerated  
- The attacker has graph visibility  
- The goal is to minimize noise and time  
- The attacker wants the most direct route to privilege  

In that sense, the shortest path is not a distinct attack family, but an **optimization criterion** applied to AD attack paths.

### Shortest Path Concept

A Shortest Path Attack is therefore:

> The minimal sequence of reachable AD relationships connecting an entry point to a high-value target.

It captures the idea of:
- Fast escalation  
- Efficient movement  
- Minimal operational cost  

---

## Translation in the Simulator

In the graph model:
- Nodes represent AD objects  
- Edges represent relationships between them  

The attack is simulated by:
- Choosing a source node  
- Choosing a target of interest  
- Computing the shortest path in the graph  

This path can then be interpreted as the **most direct attack route** available in the environment.

---

## Security Impact

Shortest paths are important because:
- They reveal the **fastest escalation opportunities**  
- They highlight highly connected or weakly protected objects  
- Very short paths indicate critical exposure  

A target reachable in only one or two steps should be considered especially dangerous.

---

## Summary

- Models the **most direct route** to a target  
- Based on graph distance, not on a specific primitive  
- Can combine multiple AD relation types  
- Useful to measure attack efficiency  
- Helps identify the most exposed privileged targets
