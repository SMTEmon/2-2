---
title: "03 - IPv6 Protocol & Tunneling Transitions"
course: "CSE 4411"
chapter: 4
section: 4.4
tags:
  - cse4411
  - networking
  - ipv6
  - tunneling
  - dual-stack
  - header-format
  - final-exam
aliases:
  - IPv6 Protocol
  - IPv6 Tunneling
---

# 03 - IPv6 Protocol & Tunneling Transitions

> [!abstract] Key Takeaway
> **IPv6** expands address space to **128 bits** ($3.4 \times 10^{38}$ addresses) and streamlines routing with a **fixed 40-byte base header**, eliminating the **Header Checksum** and **router fragmentation**. 
> Global deployment uses **Dual-Stack** hosts and **IPv6-in-IPv4 Tunneling**.

---

## 1. IPv6 Header Architecture

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version| Traffic Class |           Flow Label                  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Payload Length        |  Next Header  |   Hop Limit   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                     Source IP Address                         +
|                         (128 bits)                            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                  Destination IP Address                       +
|                         (128 bits)                            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                            Payload                            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

### Field-by-Field Breakdown

| Field Name | Size (Bits) | Description & Purpose |
| :--- | :---: | :--- |
| **Version** | 4 | IP Version (`0110` for IPv6). |
| **Traffic Class** | 8 | Equivalent to IPv4 TOS / DiffServ (QoS priority). |
| **Flow Label** | 20 | Identifies packets belonging to a specific real-time audio/video stream. |
| **Payload Length** | 16 | Number of bytes in datagram *following* the 40-byte base header (excludes base header). |
| **Next Header** | 8 | Identifies transport protocol (`6` TCP, `17` UDP) or next Extension Header (`0` Hop-by-Hop, `43` Routing, `44` Fragment). |
| **Hop Limit** | 8 | Decremented by 1 at each router; packet dropped if 0 (replaces IPv4 TTL). |
| **Source / Dest IP** | $128 + 128$ | 16-byte IPv6 addresses written in hexadecimal (e.g., `2001:0db8:85a3::8a2e:0370:7334`). |

---

## 2. Major Architectural Differences: IPv4 vs IPv6

| Feature | IPv4 | IPv6 | Engineering Rationale |
| :--- | :--- | :--- | :--- |
| **Address Space** | 32 bits ($4.3 \times 10^9$) | **128 bits ($3.4 \times 10^{38}$)** | Solves global address exhaustion permanently. |
| **Base Header Size** | Variable (20 – 60 bytes) | **Fixed 40 bytes** | Fixed offset enables pipelined hardware processing. |
| **Header Checksum** | Present (Recalculated every hop) | **REMOVED** | Redundant with L2 CRC and L4 checksums; speeds up forwarding. |
| **Fragmentation** | Performed by **routers & hosts** | **REMOVED from routers** | Routers drop oversized packets and send ICMPv6 "Packet Too Big"; source host performs PMTUD. |
| **Options** | Inside base header | **Extension Headers** | Base header stays fixed; extensions processed only when needed. |

---

## 3. IPv4 to IPv6 Transition: Dual-Stack vs Tunneling

```mermaid
flowchart LR
    A["IPv6 Host (Node A)"] -->|Native IPv6| B["IPv6/IPv4 Router (Node B)"]
    
    subgraph IPv4Cloud ["IPv4-Only Core Internet (Legacy Routers)"]
        B -->|IPv4 Encapsulated Packet<br>Src: B, Dst: E, Proto: 41| C["IPv4 Router C"]
        C --> D["IPv4 Router D"]
        D --> E["IPv6/IPv4 Router (Node E)"]
    end
    
    E -->|Native IPv6 Datagram| F["IPv6 Host (Node F)"]
```

```
Tunneling Wire Encapsulation:
+-------------------+-------------------+-------------------------+
| IPv4 Outer Header | IPv6 Inner Header | TCP / Payload           |
| (Src: B, Dst: E)  | (Src: A, Dst: F)  |                         |
+-------------------+-------------------+-------------------------+
|<--- IPv4 Outer Payload (Tunnel) ----->|
```

- **Dual-Stack:** Nodes run both IPv4 and IPv6 stacks simultaneously.
- **Tunneling (IPv6 inside IPv4):** The ingress dual-stack router (B) puts the complete IPv6 datagram inside the payload field of an IPv4 datagram (`IPv4 Protocol = 41`). The egress router (E) strips the outer IPv4 header and forwards the native IPv6 packet.

---
#### Navigation
← Previous: [[02 - NAT Architecture & Traversal]] | Next: [[04 - Book Extras & Professor Traps]] →
