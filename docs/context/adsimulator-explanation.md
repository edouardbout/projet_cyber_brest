# ADSimulator – Explanation

## 1. What is ADSimulator ?

**ADSimulator is a tool that generates synthetic Active Directory environments as graphs.**

- It creates a realistic but fake Active Directory
- It stores the data in Neo4j
- It simulates:
  - Users
  - Groups
  - Computers
  - Permissions
  - Vulnerabilities

# ADSimulator - Explanation

## 1. What ADSimulator Is

**ADSimulator is a tool that generates synthetic Active Directory environments as graphs.**

- It creates a **realistic but fake Active Directory**
- It stores the data in **Neo4j**
- It simulates:
  - Users
  - Groups
  - Computers
  - Permissions
  - Vulnerabilities

The goal is to reproduce a realistic AD environment to test attacks and conduct security research.

---

## 2. Core Concept: Graph-Based Modeling

ADSimulator models Active Directory as a graph.

### Nodes
- Domain  
- Organizational Units (OU)  
- Users  
- Groups  
- Computers  
- GPO  

### Relationships (Edges)
- `MemberOf` → group membership  
- `AdminTo` → local admin rights on a machine  
- `HasSession` → active session  
- `TrustedBy` → domain trust relationship  
- `Contains` → AD hierarchy  
- `ACLs` → critical permissions (`GenericAll`, `WriteDacl`, etc.)

This model is similar to BloodHound’s data structure.

---

## 3. Key Features

### a) Configurable Generation

You can define:

- Number of users, groups, and computers  
- AD structure (OUs, GPOs, trusts)  
- Operating systems  
- Security policies  

Example:
- `nUsers = 1000`  
- `nComputers = 500`  
- `nOUs = 200`  

---

### b) Vulnerability Injection

ADSimulator allows you to inject security weaknesses into the environment.

Examples:
- Misconfigured ACLs (`GenericAll`, `WriteDacl`)  
- Kerberos misconfigurations (e.g., no pre-authentication)  
- Weak or missing passwords  
- Dangerous delegation settings  

You can generate a secure AD or a highly vulnerable one.

---

### c) Probabilistic Modeling

Many properties are defined using probabilities.

Examples:
- 20% of users have an SPN → Kerberoasting possible  
- 10% have `passwordnotreqd`  
- 5% have unconstrained delegation  

This enables realistic simulations and statistical analysis of attack paths  

---

## 4. Workflow

Typical usage:

1. Define a configuration file (JSON)  
2. Run ADSimulator  
3. The tool:
   - Generates the graph  
   - Loads it into Neo4j  
4. Analyze using:
   - Cypher queries  
   - Graph-based tools (e.g., BloodHound-like analysis)

---

## 5. Main Use Cases

### A. Cybersecurity Research
- Study attack paths  
- Evaluate privilege escalation strategies  

### B. Attack Simulation
- Kerberoasting  
- DCSync  
- ACL abuse  

### C. Defense Optimization
- Identify critical nodes  
- Test mitigation strategies  

---

## 6. Relevance for Graph-Based Security Models

ADSimulator is particularly useful for:

- Building realistic graph datasets 
- Testing attack path algorithms 
- Applying probabilistic models (e.g., Markov chains)  

It provides:
- Structured graph data  
- Realistic distributions of vulnerabilities  
- A controlled testing environment  

---

## 7. Summary

ADSimulator is:

**A graph-based Active Directory generator with probabilistic vulnerabilities, designed to simulate and analyze cyberattacks.**

- Generates realistic AD environments  
- Models permissions and relationships as a graph  
- Injects vulnerabilities using probabilities  
- Enables attack simulation and security analysis  
**Goal:** reproduce a realistic AD environment to test attacks and conduct security research.

---

