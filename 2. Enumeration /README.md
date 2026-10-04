# 4. 🔎 Enumeration

## 📖 Introduction

Enumeration is the process of establishing a connection with a target and performing queries to gather detailed information about the target system.

It is generally performed after scanning and is commonly conducted in an **intranet environment**.

### 📌 Information That Can Be Enumerated

- 🌐 Network resources
- 📁 Shared resources and folders
- 🛣️ Routing tables
- ⚙️ Audit and service settings
- 📡 SNMP information
- 🌍 DNS information
- 🖥️ Machine names
- 👤 Users and groups
- 🧩 Applications
- 🏷️ Service banners

### 🛠️ Common Enumeration Techniques

- Extract usernames using email IDs
- Extract information using default passwords
- Brute-force Active Directory
- Extract information using DNS Zone Transfer
- Extract users and groups from Windows systems
- Extract usernames using SNMP

### 🔌 Common Ports and Services

| Port | Service |
|------|---------|
| 53 | DNS |
| 135 | MSRPC |
| 137 | NetBIOS Name Service |
| 139 | NetBIOS Session Service |
| 445 | SMB |
| 161 | SNMP |
| 162 | SNMP Trap |
| 389 | LDAP |
| 2049 | NFS |
| 25 | SMTP |
| 500 | IKE |
| 22 | SSH |

---

# 1. 🖥️ NetBIOS Enumeration

NetBIOS enumeration is used to gather information such as **computer names, workgroups, domains, and NetBIOS information** from a target.

### ▶️ Step 1 — Perform NetBIOS Enumeration

```bash
nbtscan 192.168.100.16
```

### 📤 Example Output

```text
Doing NBT name scan for addresses from 192.168.100.16

IP address       NetBIOS Name      Server
192.168.100.16   WIN-SERVER        <server>
```

### ▶️ Step 2 — Using Nmap

```bash
nmap -sU -p 137 --script nbstat 192.168.100.16
```

### 📤 Example Output

```text
137/udp open  netbios-ns

Host script results:
| nbstat:
|   NetBIOS name: WIN-SERVER
|   Workgroup: WORKGROUP
```

---

# 2. 📡 SNMP Enumeration

SNMP enumeration can provide information about **system names, interfaces, processes, and network configuration**, depending on the available access.

### ▶️ Step 1 — SNMP Information

```bash
nmap -sU -p 161 --script snmp-info 192.168.100.16
```

### 📤 Example Output

```text
161/udp open  snmp

Host script results:
| snmp-info:
|   enterprise: Microsoft
|   system name: WIN-SERVER
|   uptime: ...
```

### ▶️ Step 2 — Using snmpwalk

```bash
snmpwalk -v2c -c public 192.168.100.16
```

### 📤 Example Output

```text
SNMPv2-MIB::sysName.0 = STRING: WIN-SERVER
SNMPv2-MIB::sysDescr.0 = STRING: Windows Server
```

> 💡 `public` is a commonly encountered default community string in lab environments.

---

# 3. 📧 SMTP Enumeration

SMTP enumeration can be used to identify **valid usernames or email accounts** when the SMTP server supports enumeration methods.

### ▶️ Step 1 — Enumerate SMTP Users

```bash
nmap -p 25 --script smtp-enum-users 192.168.100.16
```

### 📤 Example Output

```text
25/tcp open  smtp

Host script results:
| smtp-enum-users:
|   administrator
|   user1
|   test
```

### ▶️ Step 2 — Detect SMTP Service

```bash
nmap -sV -p 25 192.168.100.16
```

### 📤 Example Output

```text
25/tcp open  smtp
Service Info: SMTP
```

---

# 4. 📂 NFS Enumeration

NFS enumeration is used to identify **NFS services and exported/shared directories** available from the target.

### ▶️ Step 1 — NFS Enumeration Using Nmap

```bash
nmap -p 2049 --script nfs-showmount 192.168.100.16
```

### 📤 Example Output

```text
2049/tcp open  nfs

Host script results:
| nfs-showmount:
|   /shared
|   /home
```

### ▶️ Step 2 — Using showmount

```bash
showmount -e 192.168.100.16
```

### 📤 Example Output

```text
Export list for 192.168.100.16:

/shared
/home
```

---

# 5. ⏱️ NTP Enumeration

NTP enumeration can provide information about the **NTP service and time synchronization configuration**.

### ▶️ Step 1 — NTP Enumeration

```bash
nmap -sU -p 123 --script ntp-info 192.168.100.16
```

### 📤 Example Output

```text
123/udp open  ntp

Host script results:
| ntp-info:
|   version: NTP
|   stratum: 2
```

---

# 6. 🌐 DNS Enumeration

DNS enumeration is used to gather information about **domains, hostnames, DNS records, and name servers**.

### ▶️ Step 1 — DNS Service Detection

```bash
nmap -sV -p 53 192.168.100.16
```

### 📤 Example Output

```text
53/tcp open  domain
Service Info: DNS
```

### ▶️ Step 2 — DNS Brute Force

```bash
nmap -p 53 --script dns-brute 192.168.100.16
```

### 📤 Example Output

```text
Host script results:
| dns-brute:
|   www.example.local
|   mail.example.local
|   server.example.local
```

### ▶️ Step 3 — DNS Lookup

```bash
nslookup 192.168.100.16
```

---

# 7. 🔄 DNS Zone Transfer

DNS Zone Transfer can provide a copy of DNS records if the DNS server incorrectly allows unauthorized zone transfers.

### ▶️ Step 1 — Perform Zone Transfer

```bash
dig axfr @192.168.100.16 example.local
```

### 📤 Example Output

```text
example.local.      IN  SOA
example.local.      IN  NS   ns1.example.local
www                 IN  A    192.168.100.20
mail                IN  A    192.168.100.25
server              IN  A    192.168.100.30
```

### 📌 Information Obtained

A successful zone transfer may reveal:

- 🌐 Hostnames
- 🔢 IP addresses
- 📧 Mail servers
- 🖥️ Name servers
- 🗂️ Internal DNS records

---

# 8. 🔐 DNSSEC Zone Walking

DNSSEC Zone Walking is associated with DNSSEC configurations using **NSEC records**, where sequential DNS names may be discoverable.

### ▶️ Step 1 — Perform DNSSEC Zone Walking

```bash
ldns-walk example.local
```

### 📤 Example Output

```text
example.local
mail.example.local
server.example.local
www.example.local
```

> 💡 NSEC3-based configurations can make this type of enumeration more difficult.

---

# 9. 📞 Telnet Enumeration

Telnet enumeration is used to identify whether a **Telnet service** is running and to inspect its service banner.

### ▶️ Step 1 — Detect Telnet Service

```bash
nmap -p 23 -sV 192.168.100.16
```

### 📤 Example Output

```text
23/tcp open  telnet
Service Info: Linux
```

### ▶️ Step 2 — Connect to Telnet

```bash
telnet 192.168.100.16 23
```

### 📤 Example Output

```text
Trying 192.168.100.16...
Connected to 192.168.100.16.

Ubuntu Server
login:
```

---

# 10. 🗂️ SMB Enumeration

SMB enumeration is used to gather information about **Windows shares, workgroups, domains, users, and SMB services**, depending on permissions.

### ▶️ Step 1 — Enumerate SMB Shares

```bash
nmap -p 445 --script smb-enum-shares 192.168.100.16
```

### 📤 Example Output

```text
445/tcp open  microsoft-ds

Host script results:
| smb-enum-shares:
|   IPC$
|   ADMIN$
|   Shared
```

### ▶️ Step 2 — Detect SMB Protocols

```bash
nmap -p 445 --script smb-protocols 192.168.100.16
```

### 📤 Example Output

```text
445/tcp open  microsoft-ds

Host script results:
| smb-protocols:
|   SMB 2.0.2
|   SMB 2.1
|   SMB 3.0
```

### ▶️ Step 3 — Using enum4linux

```bash
enum4linux -a 192.168.100.16
```

### 📤 Example Output

```text
Workgroup: WORKGROUP

Users:
administrator
guest
user1

Shares:
IPC$
Shared
```

---

# 📊 Enumeration Quick Reference

| Enumeration Technique | Port | Tool / Command |
|---|---:|---|
| 🖥️ NetBIOS | 137/139 | `nbtscan` |
| 📡 SNMP | 161 | `snmpwalk` / Nmap |
| 📧 SMTP | 25 | Nmap |
| 📂 NFS | 2049 | `showmount` / Nmap |
| ⏱️ NTP | 123 | Nmap |
| 🌐 DNS | 53 | `dig` / Nmap |
| 🔄 DNS Zone Transfer | 53 | `dig axfr` |
| 🔐 DNSSEC Zone Walking | 53 | `ldns-walk` |
| 📞 Telnet | 23 | `telnet` / Nmap |
| 🗂️ SMB | 445 | `enum4linux` / Nmap |

---

# 🎯 Key Takeaway

```text
Scanning
    ↓
Identify Open Ports
    ↓
Identify Services
    ↓
🔎 Enumeration
    ↓
Gather Detailed Information
    ↓
Users / Groups / Shares / Hosts / Services
```

Enumeration provides more detailed information about a target after the initial scanning phase.

> 🛡️ **Lab Safety:** Perform enumeration only against systems that you own or have explicit permission to test.
