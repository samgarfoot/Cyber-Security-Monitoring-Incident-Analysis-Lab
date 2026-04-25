# 🐧 Linux Commands Used

The following commands were executed on the Kali Linux VM to perform reconnaissance, network discovery, and service enumeration against the Windows target.

## 1. ICMP Connectivity Test (Ping)
```
ping -c 15 192.168.xx.xx
```
This command was used to verify network connectivity between the Kali Linux and Windows virtual machines by sending 15 ICMP echo requests.

Limiting the count (-c 15) ensured controlled traffic generation for analysis within Wireshark, rather than generating continuous ping traffic.

---

## 2. Network Reconnaissance Scan (Nmap)

```
nmap -sS -Sv 192.168.xx.xx
```
This SYN scan was used to identify open ports and running services on the target system.

The scan revealed exposed services, including SMB (port 445), which became the focus of further investigation.

---

## 3. SMB Service Enumeration

```
smbclient -L //192.168.xx.xx -N
```

This command attempted to enumerate available SMB shares on the Windows host over port 445 using anonymous authentication (-N).

The attempt was denied, indicating that authentication was required and anonymous access was not permitted.
