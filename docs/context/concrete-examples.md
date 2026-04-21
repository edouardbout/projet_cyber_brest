# Concrete examples to better understand cyberattacks

Welcome to this section dedicated to the practical understanding of cyberattacks. To effectively secure an infrastructure, it is crucial to understand the methodology used by attackers.

These two stories illustrate how default configurations or files deemed "harmless" can allow an internal user or an external attacker to compromise an entire network without ever triggering an alert.

---

## Story 1 — The History That Betrays Everything
**Theme: Secret extraction and lateral movement via shell history.**

This first example demonstrates that a successful intrusion does not always require the exploitation of complex software vulnerabilities (CVEs). A simple configuration error in the read permissions of a hidden file is enough to grant an attacker full control over the infrastructure.

![The history file that betrays everything](../images/context/concrete-examples/storytelling_bash_history_en.svg)

---

## Story 2 — The Network Map Handed Over in Plain Sight
**Theme: Passive reconnaissance and service account exploitation.**

This second scenario shows how a user with very limited privileges (an intern) can map an entire infrastructure by consulting standard system files, leading to the compromise of critical backups.

![The network map handed over in plain sight](../images/context/concrete-examples/storytelling_etc_passwd_hosts_en.svg)

---

## 💡 Executive Summary

These two case studies highlight a crucial point in modern cybersecurity: 
**The attacker didn't need to "break" the door; they simply used the keys left under the doormat.**

* **Zero network scans.**
* **No CVE exploitation.**
* **No SIEM alerts triggered.**

Securing an **Active Directory** or a Linux infrastructure starts with hardening local configurations and strictly applying the Principle of Least Privilege (PoLP).