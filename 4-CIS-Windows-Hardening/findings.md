# Scenario 4: CIS Benchmark Audit and Hardening — Windows 11 Pro

## Overview

Following the attack simulations conducted in previous scenarios, this phase 
focuses on hardening the Windows 11 Pro target environment against the CIS 
Microsoft Windows 11 Benchmark v5.0.0. The objective was to identify 
non-compliant security controls, remediate identified gaps, and map each 
control to the relevant NIST Cybersecurity Framework (CSF) function.

This represents the defensive response to the vulnerabilities and attack 
vectors identified through earlier red team activity.

---

## Tools Used

- Windows 11 Pro Virtual Machine (Target Environment)
- PowerShell (Audit and Remediation)
- CIS Microsoft Windows 11 Benchmark v5.0.0
- NIST Cybersecurity Framework v2.0

---

## Audit Methodology

Each control was assessed using PowerShell commands to query the current 
system configuration. Findings were categorised as:

- **Compliant** — control already met the CIS benchmark recommendation
- **Non-Compliant** — control required remediation
- **Not Applicable** — control not relevant to this environment

---

## Findings and Remediations

### 1. SMB1 Protocol

**CIS Reference:** Disable legacy protocols  
**NIST CSF Function:** Protect  
**Status Before:** Not explicitly disabled  
**Finding:** SMB1 is a legacy file sharing protocol exploited in major 
ransomware attacks including WannaCry. Its presence increases the attack 
surface significantly.  
**Remediation:** Confirmed disabled via Windows Optional Features.  
**Status After:** Compliant

---

### 2. Password Policy

**CIS Reference:** Account and credential management  
**NIST CSF Function:** Protect — Identity Management  
**Status Before:** Non-Compliant  

| Setting | Before | CIS Requirement | After |
|---|---|---|---|
| Minimum password length | 0 | 14 characters | 14 characters |
| Minimum password age | 0 days | 1 day | 1 day |
| Maximum password age | 42 days | 42 days | 42 days |
| Password history | None | 24 passwords | 24 passwords |
| Lockout threshold | 10 attempts | 5 or fewer | 5 attempts |
| Lockout duration | 10 minutes | 15 minutes | 15 minutes |

**Remediation:** Applied using `net accounts` command via PowerShell.  
**Status After:** Compliant

---

### 3. Audit Logging Policy

**CIS Reference:** CIS Control 8 — Audit Log Management  
**NIST CSF Function:** Detect  
**Status Before:** Non-Compliant — majority of subcategories set to No Auditing  
**Finding:** Without comprehensive audit logging, malicious activity cannot 
be detected or investigated. Key event categories were not being recorded.  

**Remediations Applied:**

| Subcategory | Setting Applied |
|---|---|
| Security System Extension | Success and Failure |
| Special Logon | Success and Failure |
| Account Lockout | Success and Failure |
| Group Membership | Success and Failure |
| Sensitive Privilege Use | Success and Failure |
| Process Creation | Success and Failure |
| Audit Policy Change | Success and Failure |
| Authentication Policy Change | Success and Failure |
| User Account Management | Success and Failure |
| Computer Account Management | Success and Failure |
| Credential Validation | Success and Failure |
| Removable Storage | Success and Failure |

**Status After:** Compliant

---

### 4. Windows Defender

**CIS Reference:** Malware defences  
**NIST CSF Function:** Protect  
**Status Before:** Compliant  
**Finding:** All Defender components including real-time protection, 
behaviour monitoring, antivirus, and antispyware were enabled.  
**Remediation:** No action required.  
**Status After:** Compliant

---

### 5. Windows Firewall

**CIS Reference:** Network traffic filtering  
**NIST CSF Function:** Protect  
**Status Before:** Non-Compliant — DefaultInboundAction set to NotConfigured  
**Finding:** Without an explicit block rule, inbound traffic policy is 
ambiguous and potentially permissive. CIS requires inbound traffic to be 
blocked by default across all profiles.  
**Remediation:** Set DefaultInboundAction to Block and DefaultOutboundAction 
to Allow across Domain, Private, and Public profiles.  
**Status After:** Compliant

---

### 6. Remote Desktop Protocol (RDP)

**CIS Reference:** Secure configuration of enterprise assets  
**NIST CSF Function:** Protect  
**Status Before:** Compliant  
**Finding:** RDP was already disabled (fDenyTSConnections = 1). RDP is a 
common attack vector and should only be enabled when explicitly required.  
**Remediation:** No action required.  
**Status After:** Compliant

---

### 7. AutoRun and AutoPlay

**CIS Reference:** Removable media controls  
**NIST CSF Function:** Protect  
**Status Before:** Non-Compliant — registry key not configured  
**Finding:** AutoRun allows code to execute automatically when removable 
media is inserted, a common malware delivery vector.  
**Remediation:** Created registry key and set NoDriveTypeAutoRun to 255, 
disabling AutoRun for all drive types.  
**Status After:** Compliant

---

### 8. Default Local Accounts

**CIS Reference:** CIS Control 5 — Account Management  
**NIST CSF Function:** Protect — Identity Management  
**Status Before:** Compliant  
**Finding:** Both the built-in Guest and Administrator accounts were 
disabled. The Administrator account is a high value target as its username 
is known to attackers and it cannot be locked out by default.  
**Remediation:** No action required.  
**Status After:** Compliant

---

### 9. Screen Lock Timeout

**CIS Reference:** Physical security and access control  
**NIST CSF Function:** Protect  
**Status Before:** Non-Compliant — not configured  
**Finding:** Without a screen lock timeout, an unattended machine is 
vulnerable to physical access attacks.  
**Remediation:** Set screen lock timeout to 900 seconds (15 minutes) with 
password required on resume.  
**Status After:** Compliant

---

### 10. Windows Update Policy

**CIS Reference:** Patch and vulnerability management  
**NIST CSF Function:** Protect  
**Status Before:** Non-Compliant — not explicitly configured  
**Finding:** Without an enforced update policy, critical security patches 
may not be applied in a timely manner.  
**Remediation:** Configured AUOptions to 4 (automatic download and 
installation) via registry.  
**Status After:** Compliant

---

## Summary

| Control | Status Before | Status After |
|---|---|---|
| SMB1 Protocol | Compliant | Compliant |
| Password Policy | Non-Compliant | Compliant |
| Audit Logging | Non-Compliant | Compliant |
| Windows Defender | Compliant | Compliant |
| Windows Firewall | Non-Compliant | Compliant |
| RDP | Compliant | Compliant |
| AutoRun | Non-Compliant | Compliant |
| Default Accounts | Compliant | Compliant |
| Screen Lock | Non-Compliant | Compliant |
| Windows Update | Non-Compliant | Compliant |

**Controls assessed:** 10  
**Initially compliant:** 4  
**Remediated:** 6  
**Final compliance:** 10/10

---

## NIST CSF Mapping Summary

| NIST CSF Function | Controls Applied |
|---|---|
| Protect | Password policy, firewall, RDP, AutoRun, accounts, screen lock, patching, Defender |
| Detect | Audit logging across all key event categories |

---

## Key Learnings

- Default Windows 11 configurations are insufficient for a secure enterprise 
environment without deliberate hardening
- Audit logging is critical for the Detect function — without it, attacks 
like those simulated in previous scenarios would leave no forensic trail
- Many controls that appear enabled are not explicitly configured, leaving 
policy ambiguous — explicit configuration is always preferable
- The CIS benchmark provides a structured, repeatable methodology for 
assessing and improving security posture
