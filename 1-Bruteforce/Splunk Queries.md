# Splunk Investigation Queries

## Failed Logon Analysis

```spl
index=* EventCode=4625
| stats count by Account_Name Source_Network_Address
| sort - count
```
