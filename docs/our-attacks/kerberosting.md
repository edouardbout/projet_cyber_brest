# Kerberos Adjusted Attack Path Analysis

## Definition

The **Kerberos Adjusted attack** is a simulated attack model that identifies constrained privilege escalation paths in an Active Directory graph.

In this project, we define a Kerberos Adjusted path as:

> A fixed-length attack chain (4 edges) starting from a standard user, passing through at least one SPN (Service Principal Name) account, and ending at an administrative account.

This model abstracts a simplified Kerberoasting-like escalation flow, focusing on structured graph traversal rather than cryptographic exploitation.

---

## Theoretical Background

Kerberos is the authentication protocol used in Active Directory to enable secure service authentication through tickets.

### SPN Accounts

A Service Principal Name (SPN) identifies a service instance running on a host within Active Directory.


![SPN Overview](SPN_overview.png)


SPN accounts are particularly interesting because they can be abused in **Kerberoasting attacks**, where:

- A service ticket is requested for an SPN account  
- The ticket is extracted from memory or network traffic  
- The ticket is cracked offline to recover credentials  

This makes SPN accounts a key pivot in privilege escalation scenarios.

---

## Dataset Specificity

In `Graph_0.json`, the Active Directory environment is simplified:

- Only a small number of SPN accounts exist (15 detected)
- The graph is reduced for simulation purposes
- Not all users are service-linked
- Administrative accounts are limited but clearly identifiable

This controlled structure allows deterministic analysis of attack paths.

---

## Attack Logic in the Simulator

The detected paths simulate a Kerberos-style attack chain:

1. Initial compromise of a standard user  
2. Discovery or access to SPN-enabled accounts  
3. Use or abuse of service ticket mechanisms (abstracted)  
4. Privilege escalation to an administrative account  

Each valid path corresponds to a realistic multi-step compromise scenario.

---

## Graph-Based Detection Model

The system loads the Active Directory graph from `Graph_0.json` using `networkx`.

- Nodes: Users, Computers, Groups, Service Accounts  
- Edges: Relationships between entities  

A directed graph is constructed to model attack propagation.

---

## Detection Constraints

A valid Kerberos Adjusted path must satisfy:

- Start from a **User node**
- Contain exactly **4 edges (5 nodes total)**
- Include at least one **SPN account**
- End at an **administrative node**

---

## Implementation Overview

ajout
