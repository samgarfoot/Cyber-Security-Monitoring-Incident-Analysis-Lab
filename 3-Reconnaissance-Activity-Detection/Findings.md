# 🧾 Findings — Project 3

Multi-Source Detection and Analysis of Network Reconnaissance Activity

---

## Executive Summary

This investigation analysed network reconnaissance activity originating from a Kali Linux virtual machine targeting a Windows-based endpoint within a controlled lab environment. Using a combination of network packet analysis, endpoint telemetry, and SIEM correlation, the activity was successfully identified and reconstructed.


The investigation utilised:
- Wireshark for packet-level traffic analysis
- Sysmon for endpoint event logging
- Splunk Enterprise for log correlation and timeline reconstruction

---

## Investigation Objective

The objective of this investigation was to:
- Identify and analyse reconnaissance activity targeting a Windows VM
- Validate network-level evidence using packet capture
- Correlate endpoint logs with network behaviour
- Reconstruct attacker activity using SIEM queries

---

## Environment Overview

- Attacker VM: Kali Linux
- Target VM: Windows 11
- Monitoring Tools: Splunk, Sysmon, Wireshark
- Network: Shared Subnet

---

## Reconnaissance Activity (Kali Linux)

Initial reconnaissance was conducted from the Kali Linux VM using ICMP and Nmap scanning techniques.
Observed Activity:

- ICMP echo requests successfully reached the Windows host
- Nmap scan identified open port:
- 445 (SMB - Server Message Block)

This indicated that file-sharing services were exposed and reachable within the network.

---

## Network Traffic Analysis (Wireshark)

Packet capture analysis confirmed active reconnaissance behaviour.

Observed traffic included:
- ICMP echo request and reply between Kali and Windows VM
- TCP SYN scan behaviour consistent with Nmap probing
- Connection attempts targeting port 445 (SMB)

SMB enumeration attempts were observed but resulted in authentication denial, indicating that access control mechanisms were enforced.

---

## Endpoint Telemetry (Sysmon)

Endpoint monitoring via Sysmon confirmed network connection visibility on the Windows system.

Key observation:
- Event ID 3 (Network Connection) recorded outbound/inbound connection activity
- Traffic associated with external scanning attempts was visible at the endpoint level

Although specific port-level correlation was limited in Splunk, Sysmon confirmed that network events were being logged successfully.

---

## SIEM Analysis (Splunk Correlation)

Using Splunk Enterprise, logs were analysed based on the source IP address of the Kali VM.

Key Findings:
- All reconnaissance activity was successfully correlated using:
- Source_Network_Address (Kali IP)

Event timeline reconstruction showed:
- ICMP traffic (ping activity)
- TCP connection attempts associated with scanning behaviour
- SMB-related connection attempts (port 445 exposure)

---

## MITRE ATT&CK Mapping

The observed behaviour aligns with the following MITRE ATT&CK techniques:
- T1595 – Active Scanning
Network discovery and host identification via ping and Nmap
- T1046 – Network Service Scanning
Identification of open SMB service (port 445)
- T1135 – Network Share Discovery
Attempted SMB enumeration via service probing

---

## Conclusion

The investigation successfully identified and reconstructed reconnaissance activity against a Windows VM using multi-source telemetry.

While no system compromise occurred, the activity demonstrated:
- Effective network reconnaissance techniques
- Exposure of SMB services to scanning activity
- Correlation between packet-level, endpoint, and SIEM logs

This highlights the importance of layered monitoring using network, endpoint, and SIEM tools to detect early-stage attack behaviour before compromise occurs.

---
