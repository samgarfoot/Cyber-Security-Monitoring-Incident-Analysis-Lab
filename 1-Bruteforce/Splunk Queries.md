# Splunk Investigation Queries

## Failed Logon Analysis

### Step 1 — Find most recent activity (timeline view)

```spl
index=* EventCode=4625
| sort - _time
```
Purpose:
- See latest events first
- Identify “something happening right now”
- Spot unusual IPs quickly

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

This gave me: 
- Full attack timeline
- Targeted accounts
- Frequency of attempts
- Behaviour pattern

---

## STEP 4 — Expand scope

```spl
index=* Source_Network_Address="192.168.x.x"
| stats count by EventCode
```

Purpose:
- See if attacker did anything else
- Not just logins
