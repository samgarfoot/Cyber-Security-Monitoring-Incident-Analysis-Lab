# 📊 Splunk Queries Used

The following queries were executed in Splunk Enterprise to analyse, correlate, and investigate reconnaissance activity originating from the Kali Linux VM.

These searches focused on network activity, endpoint logs, and event correlation using source IP tracking and security event IDs.

---

## 1. Full Log Review (Initial Investigation)

```
index=* 
| sort 0 - _time
```

Used as an initial broad search to review all ingested logs and establish a baseline timeline of system activity.

What it shows:
- Overall event visibility
- Raw ingestion confirmation
- Event ordering across all sources

---

## 2. Source IP Correlation (Kali VM Activity)

```
index=* Source_Network_Address="<KALI_IP>"| sort 0 - _time
```

Filtered all events originating from the Kali Linux VM to isolate reconnaissance activity.
What it shows:

- Full attack/recon timeline
- Cross-event correlation from single source IP
- Network-based activity tracking

---

## 3. Security Event Filtering (Authentication & Privilege Events)

```
index=* (EventCode=4625 OR EventCode=4624 OR EventCode=4672 OR EventCode=4720 OR EventCode=4732)| sort 0 - _time
```

Focused on authentication, privilege usage, and user/group-related security events.

What it shows:

- Failed login attempts (4625)
- Successful logins (4624)
- Special privilege assignments (4672)
- User creation events (4720)
- Group membership changes (4732)

---

## 4. Privilege & User Activity Analysis

```
index=* (EventCode=4720 OR EventCode=4732)| table _time EventCode Account_Name TargetUserName Group_Name| sort 0 - _time
```

Used to analyse user account creation and administrative group modifications.

What it shows:

- Account creation activity
- Privilege escalation attempts
- Group membership changes

---

## Summary

These Splunk queries enabled:

- Timeline reconstruction of reconnaissance activity
- Source IP-based investigation of attacker behaviour
- Authentication and privilege event analysis
- Endpoint and network correlation via Sysmon logs
- Multi-source SIEM-based investigation workflow

---
