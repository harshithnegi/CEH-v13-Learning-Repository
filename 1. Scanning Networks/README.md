# 🔍 Scanning Networks

> **CEH v13 — Module 01**
>
> Network scanning is a key part of **Information Gathering** used to identify live systems, open ports, running services, operating systems, and other information about a target network.

---

## 📖 Introduction

**Network scanning** is the process of identifying:

- 🖥️ Hosts
- 🌐 IP addresses
- 🚪 Open ports
- ⚙️ Running services
- 🧩 Operating systems
- 🔎 Other information about systems in a network

It is an important component of **Information Gathering** and helps security professionals create a profile of a target organization's network.

### 🔄 Basic Idea

```text
Reconnaissance
      ↓
Information Gathering
      ↓
Network Scanning
      ↓
Identify Live Hosts
      ↓
Identify Ports & Services
      ↓
Identify OS / Versions
      ↓
Understand Attack Surface
```

---

# 🎯 Objectives of Network Scanning

The main objectives of network scanning are:

| Objective | Description |
|---|---|
| 🖥️ Host Discovery | Discover live/reachable systems |
| 🌐 IP Discovery | Identify IP addresses |
| 🚪 Port Discovery | Identify open, closed, and filtered ports |
| ⚙️ Service Discovery | Identify services running on open ports |
| 🔢 Version Detection | Determine service/software versions |
| 💻 OS Detection | Identify the operating system |
| 🏗️ Architecture | Gather information about system architecture |
| 🔎 Vulnerability Discovery | Identify potential weaknesses |
| 🗺️ Network Mapping | Understand the network structure and attack surface |

---

# 🚩 TCP Communication Flags

TCP communication is controlled using **flags in the TCP packet header**.

These flags are important during network scanning because different TCP responses can help determine whether a port is **open, closed, or filtered**.

## TCP Flags

| Flag | Name | Purpose |
|:---:|---|---|
| `URG` | Urgent | Indicates that urgent data is present |
| `ACK` | Acknowledgment | Acknowledges received data |
| `PSH` | Push | Requests immediate delivery of buffered data |
| `RST` | Reset | Abruptly terminates or rejects a connection |
| `SYN` | Synchronize | Used to initiate a TCP connection |
| `FIN` | Finish | Used to gracefully terminate a TCP connection |

### 💡 Important

TCP flags become particularly useful during **TCP port scanning**, where the response to a particular flag combination can provide information about the state of a port.

---

# 🛠️ Scanning Tools

Several tools can be used for network scanning and network testing.

## 🟢 Nmap

**Nmap (Network Mapper)** is one of the most widely used network scanning tools.

### What Nmap Can Do

- 🖥️ Discover live hosts
- 🚪 Scan ports
- ⚙️ Detect services
- 🔢 Detect service versions
- 💻 Perform OS detection
- 📜 Run NSE scripts
- 🔄 Perform different TCP and UDP scans

Nmap is commonly used by penetration testers and security professionals for **network discovery and enumeration**.

---

## 🔵 Hping3

**Hping3** is a command-line packet crafting and network testing tool.

It allows security professionals to create and send customized TCP/IP packets.

### Common Uses

- Test TCP/IP behavior
- Analyze packets
- Test firewall behavior
- Perform network troubleshooting
- Create customized packets
- Perform customized scanning techniques

> 💡 **Key Point:**  
> Nmap is mainly used for **scanning and enumeration**, while Hping3 is particularly useful when **custom packet creation and manipulation** is required.

---

# 🖥️ Host Discovery Techniques

**Host discovery** is used to determine which systems are **alive/reachable** on a network.

Different techniques can be used depending on:

- Network type
- Protocol availability
- Firewall rules
- IDS/IPS configuration
- ICMP filtering

---

## 🟠 ARP Ping Scan

ARP scanning uses **ARP requests** to determine whether hosts are active on a local Ethernet network.

It is particularly useful on a **local network** because ARP operates at the local network level.

### Key Point

> ARP scanning is mainly useful for discovering hosts on the local Layer 2 network.

---

## 🟣 UDP Ping Scan

UDP ping sends **UDP packets** to a target to determine whether the host is reachable.

Depending on the response, the scanner can determine whether the host is active.

UDP-based discovery can be useful when **ICMP traffic is blocked**.

---

# 📡 ICMP Ping

**ICMP (Internet Control Message Protocol)** can be used to determine whether a target host is reachable.

Several ICMP-based techniques exist.

---

## ICMP Echo Ping

An **ICMP Echo Request** is sent to the target.

If the target responds with an **ICMP Echo Reply**, the host is considered reachable.

This is the traditional method used by the `ping` command.

### Flow

```text
Scanner
   │
   │ ICMP Echo Request
   ▼
Target
   │
   │ ICMP Echo Reply
   ▼
Scanner
```

---

## ICMP Ping Sweep

An **ICMP ping sweep** sends ICMP Echo Requests to multiple IP addresses in a network range.

The responses can be used to identify live hosts.

```text
192.168.1.1  →  ?
192.168.1.2  →  ?
192.168.1.3  →  ?
192.168.1.4  →  ?
       ↓
Identify responding hosts
```

---

## ICMP Timestamp

**ICMP Timestamp** messages can be used to determine whether a host responds to ICMP timestamp requests.

---

## ICMP Address Mask

**ICMP Address Mask** requests can be used to obtain information about the subnet mask of a target in environments where this functionality is supported.

---

# 🔵 TCP Ping Scan

TCP-based host discovery sends TCP packets to determine whether a target system is reachable.

Two commonly discussed techniques are:

---

## TCP SYN Ping

A **TCP SYN** packet is sent to the target.

The response can indicate that the host is reachable.

```text
Scanner
   │
   │ TCP SYN
   ▼
Target
   │
   │ Response
   ▼
Scanner
```

---

## TCP ACK Ping

A **TCP ACK** packet is sent to the target.

The response can provide information about whether the target is reachable and how a firewall is handling the packet.

---

# 🌐 IP Protocol Ping Scan

An **IP protocol ping scan** sends packets using different IP protocol numbers to determine whether a target host responds.

This technique can be useful when common **ICMP or TCP-based discovery methods are filtered**.

---

# ⚙️ Service Version Detection

After discovering open ports, the next step can be determining **which service and version** is running on those ports.

Nmap provides service/version detection using:

```bash
nmap -sV <TARGET-IP>
```

### What does `-sV` do?

The `-sV` option attempts to determine:

- Service name
- Service version
- Software information

For example:

```text
Port 80 → Open
       ↓
Service Detection
       ↓
Web Server
       ↓
Server Version
```

Instead of simply knowing that port `80` is open, version detection may identify the web server software running on that port.

---

# 💻 OS Fingerprinting

**OS fingerprinting** is the process of identifying the operating system running on a remote target.

Nmap provides OS detection using:

```bash
nmap -O <TARGET-IP>
```

> ⚠️ **Important:**  
> `-O` (**capital O**) is used for OS detection.  
> `-o` (**lowercase o**) is used for Nmap output options.

OS detection can help identify whether a target is running:

- Windows
- Linux
- Unix-based systems
- Other supported operating systems

---

# 🏷️ OS Discovery & Banner Grabbing

**OS discovery** attempts to determine the operating system running on a remote system.

**Banner grabbing** is a technique used to collect information exposed by a service.

### Information that may be obtained

- Service name
- Software name
- Software version
- Server information
- Operating system-related information

### Possible Sources

- Service responses
- Error messages
- HTTP headers
- Application responses

---

# ⚡ Active OS Fingerprinting

In **active OS fingerprinting**, specially crafted packets are sent directly to the target system.

The scanner analyzes the responses received from the target and compares their characteristics with known operating-system behavior.

Different operating systems may respond differently because of differences in their **TCP/IP stack implementations**.

### 🔄 Basic Process

```text
┌─────────┐
│ Scanner │
└────┬────┘
     │
     │ Specially crafted packets
     ▼
┌─────────┐
│ Target  │
└────┬────┘
     │
     │ Response
     ▼
┌─────────┐
│ Scanner │
└────┬────┘
     │
     ▼
Analyze response characteristics
     │
     ▼
Possible OS identification
```

> ⚠️ Active fingerprinting directly interacts with the target and may therefore be detected by security monitoring systems such as IDS/IPS.

---

# 🕵️ Passive OS Fingerprinting

In **passive OS fingerprinting**, the tester does not actively send specially crafted packets to identify the operating system.

Instead, information is collected by observing **existing network traffic** or information already exposed by the target.

### Possible Sources

- Network traffic
- Packet captures
- Service banners
- Error messages
- HTTP headers
- Application responses

### Active vs Passive

| Active Fingerprinting | Passive Fingerprinting |
|---|---|
| Sends packets to target | Observes existing traffic |
| Direct interaction | No direct probing required |
| Can be more detectable | Generally less intrusive |
| Analyzes target responses | Analyzes observed traffic |

---

# ⏱️ TTL and OS Identification

**TTL (Time To Live)** is a value present in the IP header.

Different operating systems commonly use different default TTL values.

By observing the TTL value in captured packets, a security professional may obtain a **clue** about the operating system of the sending host.

### ⚠️ Important

TTL alone should **not** be considered definitive OS identification because TTL values can be modified or affected by:

- Network devices
- Routing
- Packet manipulation
- Operating system configuration

Therefore, TTL should be treated as **one characteristic** that can contribute to OS fingerprinting.

---

# 🕸️ Packet Sniffing for OS Discovery

Packet sniffing tools can capture network traffic generated by a target system.

By analyzing captured packets, a security professional may observe characteristics such as:

- TTL values
- TCP/IP behavior
- TCP options
- Packet structure
- Protocol behavior

These characteristics can provide clues about the operating system and its network stack.

---

# 📜 Nmap OS Discovery Script

Nmap's **NSE (Nmap Scripting Engine)** includes scripts that can gather additional information about a target.

For example:

```bash
nmap --script=smb-os-discovery <TARGET-IP>
```

The `smb-os-discovery` script can attempt to obtain operating-system-related information from systems exposing **SMB services**.

---

# 🛡️ IDS & Firewall Evasion

**Firewalls** and **IDS/IPS** systems are designed to monitor, filter, or block unwanted or malicious network traffic.

During authorized security testing, penetration testers may test whether these security controls can detect or handle different types of network traffic.

### Commonly Discussed Techniques

| Technique | Basic Idea |
|---|---|
| Packet Fragmentation | Split packets into smaller fragments |
| Source Routing | Influence the route taken by packets |
| Source Port Manipulation | Use a specific source port |
| Decoys | Make scanning appear to come from multiple sources |
| IP Spoofing | Modify the apparent source IP |
| MAC Spoofing | Modify the apparent Layer 2 address |
| Custom Packets | Create specially crafted packets |
| Proxy Servers | Route traffic through an intermediary |
| Anonymizers | Route traffic through intermediary systems |

---

## 🧩 Packet Fragmentation

**Packet fragmentation** divides an IP packet into smaller fragments.

Historically, fragmentation could make packet inspection more difficult for some poorly implemented security devices.

Modern firewalls and IDS/IPS solutions can often **reassemble fragments before inspection**, which can reduce the effectiveness of this technique.

---

## 🛣️ Source Routing

**Source routing** allows the sender to specify the route that packets should take through a network.

It has historically been discussed as an evasion technique because it can influence how packets travel through network security controls.

Modern networks commonly restrict or disable source routing because of its security implications.

---

## 🚪 Source Port Manipulation

**Source port manipulation** involves sending traffic using a specific source port.

Some poorly configured firewall rules may allow or trust traffic based on assumptions about commonly used source ports.

For example, a firewall might incorrectly trust traffic originating from a port normally associated with a trusted service.

> 💡 The effectiveness of this technique depends heavily on the configuration of the security device.

---

## 🎭 Decoys

**Decoy scanning** makes scan traffic appear to originate from multiple source addresses.

The purpose is to make it more difficult for a target or monitoring system to determine which system is the actual scanner.

The additional source addresses are called **decoys**.

```text
             ┌── Decoy 1
             │
Scanner ─────┼── Decoy 2 ───► Target
             │
             └── Decoy 3
```

---

## 🥸 IP Spoofing

**IP spoofing** involves placing a different source IP address in the IP packet.

This can make traffic appear to originate from another system.

However, spoofing is not always useful for normal two-way communication because responses are normally sent to the spoofed IP address rather than the actual sender.

---

## 🔗 MAC Address Spoofing

A **MAC address** can sometimes be changed or impersonated on a local network.

This can affect how a device appears at the **Layer 2** network level.

MAC address spoofing is mainly relevant to local network environments.

---

## 🧪 Custom Packets

**Custom packets** are packets created with specific or modified values in their headers.

Security professionals may use custom packets to test how:

- Firewalls
- IDS/IPS devices
- Network security controls

respond to unusual or specially crafted traffic.

Tools such as **Hping3** can be used to create customized packets.

---

## 🔀 Proxy Servers

A **proxy server** acts as an intermediary between a client and a destination.

Instead of communicating directly with the target, traffic passes through the proxy.

```text
┌────────┐
│ Client │
└───┬────┘
    │
    ▼
┌────────┐
│ Proxy  │
└───┬────┘
    │
    ▼
┌────────┐
│ Target │
└────────┘
```

From the target's perspective, the connection may appear to originate from the proxy rather than directly from the original client.

---

## 🕶️ Anonymizers

**Anonymizers** are services or systems that route traffic through intermediary systems to reduce direct exposure of the original source.

They can make attribution more difficult, but they do **not guarantee complete anonymity**.

---

# 🧠 Key Takeaways

> ### Remember

- **Network scanning** is an important part of Information Gathering.
- **Host discovery** identifies live/reachable systems.
- **Port scanning** identifies accessible ports.
- **Service detection** identifies services running on open ports.
- **Version detection** can identify software/service versions.
- **OS fingerprinting** attempts to identify the operating system.
- **Active fingerprinting** sends specially crafted packets.
- **Passive fingerprinting** observes existing traffic.
- **TTL** can provide clues about an operating system but is not definitive.
- **Nmap** is a major tool for scanning and enumeration.
- **Hping3** can be used for custom packet creation and network testing.
- Firewalls and IDS/IPS can affect the results of network scanning.

---

# 📌 Summary

Network scanning is an important part of **Information Gathering** and helps identify systems, ports, services, and other information exposed within a target environment.

The major areas covered in network scanning include:

- 🖥️ Host Discovery
- 🚪 Port Scanning
- 🚩 TCP Flags
- ⚙️ Service & Version Detection
- 💻 OS Fingerprinting
- 🏷️ Banner Grabbing
- ⚡ Active Fingerprinting
- 🕵️ Passive Fingerprinting
- 🕸️ Packet Analysis
- 🛡️ IDS/Firewall Evasion Concepts

### 🛠️ Common Tools

- **Nmap**
- **Hping3**
- **Packet Sniffing Tools**

> ⚠️ **Authorization Notice:**  
> All scanning and security testing should be performed only against systems for which you have explicit authorization.
