# Cyber Security Monitoring & Incident Analysis Lab

## Overview

This project is a hands-on cybersecurity home lab focused on **security monitoring, log analysis, and incident investigation** in a Windows-based environment.

The lab simulates real-world blue team scenarios where security events are generated, collected, and analysed using industry-standard tools. The goal is to develop practical skills in detecting, investigating, and responding to potential security incidents.

This environment is designed to replicate the type of work performed in Security Operations Centres (SOC), Incident Response teams, and Threat Detection roles.

---

## Objectives

- Develop practical experience in analysing Windows security logs
- Investigate simulated cyber security incidents
- Build detection logic using log-based queries
- Understand attacker behaviour through controlled simulations
- Improve incident response and reporting skills
- Gain familiarity with SIEM-style analysis workflows

---

## Tools Used

- Windows 11 Virtual Machine (Target Environment)
- Kali Linux Virtual Machine (Attack Simulation)
- Splunk Enterprise (Log ingestion and analysis)
- Powershell

---

## Lab Structure

The project is divided into multiple blue team scenarios, each simulating a real-world security incident.

### [Scenario 1 : Brute Force Authentication Detection](1-Bruteforce/findings.md)

- Simulated repeated failed login attempts from an Kali Linux VM.
- Analysis of Windows Event ID 4625 logs
- Identification of source IP, targeted account, and attack patterns
- Detection using Splunk queries

### [Scenario 2: Privilege Escalation via Local Account Creation](2-Privilege%20Escalation/Findings.md)
- Simulated creation of a new local administrator account
- Investigation of Event ID 4720, 4732 and 4672
- Analysis of privilege escalation behaviour, mapping to MITRE ATT&CK.

### Scenario 3: Suspicious PowerShell Activity
- Execution of potentially malicious PowerShell commands
- Detection of encoded or unusual script behaviour
- Analysis using Windows logs and Splunk correlation

---
