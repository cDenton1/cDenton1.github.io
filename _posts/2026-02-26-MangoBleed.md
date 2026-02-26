---
title: "MangoBleed Writeup - HackTheBox"
tags: [hidden-1, hidden-2, hidden-3]
read_time: "__ min read"
---

# MangoBleed Writeup - HackTheBox

...

## Sherlock Scenario

> You were contacted early this morning to handle a high‑priority incident involving a suspected compromised server. The host, mongodbsync, is a secondary MongoDB server. According to the administrator, it's maintained once a month, and they recently became aware of a vulnerability referred to as MongoBleed. As a precaution, the administrator has provided you with root-level access to facilitate your investigation.
> 
> You have already collected a triage acquisition from the server using UAC. Perform a rapid triage analysis of the collected artifacts to determine whether the system has been compromised, identify any attacker activity (initial access, persistence, privilege escalation, lateral movement, or data access/exfiltration), and summarize your findings with an initial incident assessment and recommended next steps.

## Tasks

### Task 1

> What is the CVE ID designated to the MongoDB vulnerability explained in the scenario?

...

### Task 2

> What is the version of MongoDB installed on the server that the CVE exploited?

...

### Task 3

> Analyze the MongoDB logs to identify the attacker’s remote IP address used to exploit the CVE.

...

### Task 4

> Based on the MongoDB logs, determine the exact date and time the attacker’s exploitation activity began (the earliest confirmed malicious event)

...

### Task 5

> Using the MongoDB logs, calculate the total number of malicious connections initiated by the attacker.

...

### Task 6

> The attacker gained remote access after a series of brute‑force attempts. The attack likely exposed sensitive information, which enabled them to gain remote access. Based on the logs, when did the attacker successfully gain interactive hands-on remote access?

...

### Task 7

> Identify the exact command line the attacker used to execute an in‑memory script as part of their privilege‑escalation attempt.

...

### Task 8

> The attacker was interested in a specific directory and also opened a Python web server, likely for exfiltration purposes. Which directory was the target?

...

## Conclusion

...
