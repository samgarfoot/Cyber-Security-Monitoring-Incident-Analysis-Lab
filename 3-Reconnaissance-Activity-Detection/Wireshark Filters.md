# 🌐 Wireshark Filters Used

The following Wireshark display filters were used during network traffic analysis to identify and investigate reconnaissance activity between the Kali Linux and Windows virtual machines.

All analysis was performed using Wireshark.

## 1. ICMP Traffic (Connectivity Testing)
```
icmp
```

Purpose:
- Used to filter ICMP echo request and echo reply packets generated during the ping test between the Kali Linux and Windows VM.

What it shows:
- Network connectivity validation
- Source and destination IP communication
- Successful ICMP request/response pairs

---

## 2. SMB Traffic Analysis (Port 445)
```
tcp.port == 445
```

This filters all traffic associated with SMB (Server Message Block) communication over port 445.

What it shows:
- File-sharing service communication attempts
- SMB enumeration activity
- Connection attempts between Kali and Windows VM

---

## 3. SYN Scan Detection (Nmap Activity)

```
tcp.flags.syn == 1 && tcp.flags.ack == 0
```
Identifies TCP SYN packets, which are commonly used during port scanning activities such as Nmap SYN scans.

What it shows:
- Port scanning behaviour
- Half-open connection attempts
- Reconnaissance activity targeting open services

---

## 4. Host-Based Traffic Filtering (Kali Source IP)

```
ip.addr == <KALI_VM_IP>
```

Filters all traffic originating from or destined to the Kali Linux VM.

What it shows:
- Complete attack/recon traffic footprint
- Cross-protocol activity (ICMP, TCP, SMB)
- Full interaction timeline from attacker perspective

---

## 5. SMB Protocol Analysis

```
smb || smb2
```

Filters SMB protocol-level traffic for deeper inspection beyond just port-based filtering.

What it shows:
- SMB negotiation attempts
- Share enumeration requests
- Authentication failures or access denial responses

---

## Summary

These filters allowed structured analysis of:
- Network connectivity (ICMP)
- Reconnaissance activity (SYN scanning)
- Service exposure (SMB on port 445)
- Host-based traffic correlation (Kali IP filtering)
- Protocol-level SMB behaviour
