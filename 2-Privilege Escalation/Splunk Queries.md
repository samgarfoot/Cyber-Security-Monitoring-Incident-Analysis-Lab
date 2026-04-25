# Splunk Investigation Queries — Scenario 2

## 1. Detect User Account Creation (Event ID 4720)

```spl
index=* EventCode=4720
| table _time Account_Name TargetUserName Message
| sort - _time
```

## 2. Detect Privileged Group Membership Changes (Event ID 4732)

```
index=* EventCode=4732
| table _time Account_Name TargetUserName Message
| sort - _time
```

## 3. Detect Privileged Logons (Event ID 4672)

```
index=* EventCode=4672
| table _time Account_Name SubjectUserName Message
| sort - _time
```

## 4. Full Privilege Escalation Timeline

```
index=* (EventCode=4720 OR EventCode=4732 OR EventCode=4672)
| table _time EventCode Account_Name TargetUserName SubjectUserName Message
| sort - _time
```
