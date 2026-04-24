# Scenario 1: Brute Force Authentication Detection

## Overview

This scenario simulates a brute force authentication attack against a Windows endpoint from a remote Kali Linux machine. The objective was to generate repeated failed login attempts and analyse the resulting security logs using Splunk to identify malicious behaviour patterns.

The investigation focuses on detecting failed authentication activity, identifying the source of the attack, and assessing whether any successful compromise occurred.

---

## Attack Simulation

A brute force login attempt was simulated from a Kali Linux VM targeting a Windows 11 VM.

The attack involved repeated authentication attempts against a local Windows user account using incorrect credentials over SMB.

This activity generated multiple failed logon events on the Windows system.

---

## Detection Method

Detection was performed using **Splunk Enterprise** by analysing Windows Security Event Logs.

The primary event type used for detection was:

- **Event ID 4625 – Failed Logon Attempt**

---

## Splunk Investigation Queries

### Failed Logon Analysis

```spl
index=* EventCode=4625
| stats count by Account_Name Source_Network_Address
| sort - count
