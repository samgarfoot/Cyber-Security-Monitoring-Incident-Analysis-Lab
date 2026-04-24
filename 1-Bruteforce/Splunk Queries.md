# Splunk Investigation Queries

## Failed Logon Analysis

### Investigation Approach

This investigation followed a pivot-based methodology:

1. Identify failed authentication activity
2. Review most recent events for anomalies
3. Isolate suspicious source IP address
4. Pivot analysis based on attacker IP
5. Expand scope to identify additional activity

---

### Step 1 — Find most recent activity (timeline view)

```spl
index=* EventCode=4625
| sort - _time
```
This query was used to review the most recent failed logon attempts in order to identify any unusual or suspicious authentication activity.

---

### Step 2 — Identify suspicious IP manually

I noticed repeated entries from:

192.168.x.x (My Kali VM IP)

---

### Step 3 - Pivot on suspicious IP

```spl
index=* EventCode=4625 Source_Network_Address="192.168.x.x"
| table _time Account_Name Source_Network_Address Failure_Reason Logon_Type
| sort - _time
```

This query was used to isolate all failed authentication attempts from the attacker IP.

Findings:
- Full attack timeline reconstructed
- Targeted account identified
- High-frequency authentication failures observed
- Behaviour consistent with brute force activity

---

### STEP 4 — Expand scope

```spl
index=* Source_Network_Address="192.168.x.x"
| stats count by EventCode
```

This query was used to determine whether the attacker IP generated any additional types of security events beyond failed authentication attempts.

---

### Summary
The investigation confirmed that a single source IP (Kali Linux VM) generated repeated failed authentication attempts against a Windows user account. The behaviour observed is consistent with a brute force attack pattern.

No successful authentication events were observed during the investigation period.


