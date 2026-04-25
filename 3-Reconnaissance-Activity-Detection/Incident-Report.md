# 📄 Incident Report 

## Executive Summary

On investigation of network and endpoint telemetry, reconnaissance activity was identified originating from a Kali Linux virtual machine targeting a Windows host within a controlled lab environment.

The activity included ICMP probing, Nmap scanning, and SMB service enumeration attempts. These actions were successfully detected and validated using a combination of network packet analysis, endpoint telemetry, and SIEM log correlation.

The investigation utilised:
- Wireshark
- Sysmon
- Splunk Enterprise

No compromise or unauthorised access was achieved; the activity remained within the reconnaissance phase.

---
## Incident Classification

- Type: Network Reconnaissance Activity
- Severity: Low (Controlled Lab Environment)
- Stage: Pre-Attack / Reconnaissance
- Affected Asset: Windows Virtual Machine
- Source: Kali Linux Virtual Machine

---
## Timeline of Events

| Time | Event                                               |
| ---- | --------------------------------------------------- |
| T0   | ICMP ping initiated from Kali Linux                 |
| T1   | Nmap SYN scan performed against target host         |
| T2   | SMB service (port 445) identified as open           |
| T3   | SMB enumeration attempt executed                    |
| T4   | Access denied response received                     |
| T5   | Network traffic captured and validated in Wireshark |
| T6   | Logs correlated in Splunk using source IP           |

---
## Technical Analysis
 
### 1. Network Traffic Analysis

Network packet capture confirmed:
- ICMP echo request and reply traffic between hosts
- TCP SYN scan behaviour consistent with port scanning
- SMB connection attempts directed at port 445
- Authentication failure during SMB enumeration attempt
- All traffic was validated using Wireshark display filters during analysis.

### 2. Endpoint Telemetry (Sysmon)

- Sysmon provided endpoint-level visibility of network connections.
- Event ID 3 recorded external network connections
- Confirmed interaction between Kali Linux and Windows host
- Validated reconnaissance behaviour at endpoint level

### 3. SIEM Correlation (Splunk)

Splunk Enterprise was used to correlate logs using source IP filtering.

Key findings:
- All recon activity was traced to a single source IP (Kali VM)
- Event timeline reconstruction confirmed sequential scanning behaviour
- Limited structured port-level data was available, requiring IP-based correlation

---
## MITRE ATT&CK Mapping

The observed behaviour aligns with the following MITRE ATT&CK techniques:

- [T1595 – Active Scanning](https://attack.mitre.org/techniques/T1595/) - Network discovery and host identification via ping and Nmap
- [T1046 – Network Service Scanning](https://attack.mitre.org/techniques/T1046/) - Identification of open SMB service (port 445)
- [T1135 – Network Share Discovery](https://attack.mitre.org/techniques/T1135/) - Attempted SMB enumeration via service probing

---
## Analyst Assessment

The observed activity is consistent with early-stage reconnaissance behaviour commonly used prior to exploitation attempts.

Although no compromise occurred, the following risk indicators were identified:
- External host probing internal services
- SMB service exposure (port 445)
- Successful identification of open network services

This type of activity is typically associated with attackers performing target enumeration before attempting credential attacks or lateral movement.

---
## Conclusion
This investigation successfully demonstrated the detection and analysis of reconnaissance activity across network, endpoint, and SIEM layers.

The combined use of Wireshark, Sysmon, and Splunk enabled full visibility of attacker behaviour and confirmed the importance of layered monitoring in identifying pre-attack activity.

---
