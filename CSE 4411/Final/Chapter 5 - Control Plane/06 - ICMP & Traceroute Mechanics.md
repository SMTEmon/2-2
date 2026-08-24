---
title: "06 - ICMP & Traceroute Mechanics"
course: "CSE 4411"
chapter: 5
section: 5.5
tags:
  - cse4411
  - networking
  - icmp
  - traceroute
  - ping
  - network-diagnostics
aliases:
  - ICMP Protocol
  - Traceroute Mechanics
---

# 06 - ICMP & Traceroute Mechanics

> [!abstract] Key Takeaway
> **ICMP (Internet Control Message Protocol - RFC 792)** is used by hosts and routers to communicate network-layer diagnostic and error information. 
> ICMP messages are carried directly inside IP datagrams (**`IP Protocol = 1`**) and form the core operational foundation of **Ping** and **Traceroute**.

---

## 1. ICMP Message Architecture

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|     Type      |     Code      |          Checksum             |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                 Contents depend on Type & Code                |
|  (Includes IP Header + First 8 Bytes of Offending Datagram)   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

> [!info] Why ICMP copies the first 8 bytes of the offending payload
> The first 8 bytes of the Layer 4 payload contain the **Source and Destination Port numbers** (and TCP sequence number), allowing the sending host's OS to identify the exact application socket that generated the failed packet.

---

## 2. Master Table of High-Yield ICMP Types & Codes

| Type | Code | Description / Meaning | Associated Tool / Context |
| :---: | :---: | :--- | :--- |
| **0** | **0** | **Echo Reply** | `ping` response |
| **8** | **0** | **Echo Request** | `ping` request |
| **3** | **0** | Destination Network Unreachable | Routing failure |
| **3** | **1** | Destination Host Unreachable | Host offline / ARP failure |
| **3** | **2** | Destination Protocol Unreachable | Protocol not supported |
| **3** | **3** | **Destination Port Unreachable** | **Traceroute termination signal** |
| **3** | **4** | **Fragmentation Needed and DF Set** | **Path MTU Discovery (PMTUD)** |
| **11** | **0** | **TTL Expired in Transit** | **Traceroute intermediate hop signal** |
| **11** | **1** | Fragment Reassembly Time Exceeded | Reassembly timer expired at destination |

---

## 3. Traceroute Protocol Deep Dive

```mermaid
sequenceDiagram
    autonumber
    actor Source as Source Host
    participant R1 as Router 1 (Hop 1)
    participant R2 as Router 2 (Hop 2)
    actor Dest as Destination Host

    Note over Source: Send UDP to Port 33434 with TTL = 1
    Source->>R1: UDP Datagram (TTL = 1)
    Note over R1: TTL decremented to 0! Packet Dropped!
    R1->>Source: ICMP Type 11, Code 0 (TTL Expired in Transit)
    Note over Source: Source computes RTT 1 and records R1 IP

    Note over Source: Send UDP to Port 33435 with TTL = 2
    Source->>R1: UDP Datagram (TTL = 2)
    R1->>R2: UDP Datagram (TTL = 1)
    Note over R2: TTL decremented to 0! Packet Dropped!
    R2->>Source: ICMP Type 11, Code 0 (TTL Expired in Transit)
    Note over Source: Source computes RTT 2 and records R2 IP

    Note over Source: Send UDP to Port 33436 with TTL = 3
    Source->>R1: UDP Datagram (TTL = 3)
    R1->>R2: UDP Datagram (TTL = 2)
    R2->>Dest: UDP Datagram (TTL = 1)
    Note over Dest: Datagram reaches Dest Host!<br>Port 33436 is closed!
    Dest->>Source: ICMP Type 3, Code 3 (Destination Port Unreachable)
    Note over Source: Source sees Type 3 Code 3 -> TRACE COMPLETE!
```

---

## 4. "Why" Questions & Exam Traps

> [!question] Why does Traceroute target an unlikely high UDP port number (e.g., 33434+)?
> **Answer:**
> - Intermediate routers decrement TTL and drop packets solely because $\text{TTL} = 0$, sending back **ICMP Type 11 Code 0**.
> - When the packet finally reaches the destination host, the TTL will *not* expire. The destination host inspects the port and discovers no process listening on port 33434, triggering an **ICMP Type 3 Code 3 (Port Unreachable)**.
> - This distinct error code tells the sender that the packet has reached the target host, successfully ending the trace!

---
#### Navigation
← Previous: [[05 - Inter-AS Routing & BGP-4]] | Next: [[07 - Book Extras & Professor Traps]] →
