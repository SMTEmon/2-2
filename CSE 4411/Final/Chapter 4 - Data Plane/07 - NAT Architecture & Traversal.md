---
title: "07 - NAT Architecture & Traversal"
course: "CSE 4411"
chapter: 4
section: 4.3
tags:
  - cse4411
  - networking
  - nat
  - rfc1918
  - nat-traversal
  - upnp
aliases:
  - Network Address Translation
  - NAT and Port Forwarding
---

# 07 - NAT Architecture & Traversal

> [!abstract] Key Takeaway
> **NAT (Network Address Translation - RFC 3022)** maps an entire private network of devices using RFC 1918 private IP addresses to a **single public IP address** by translating both IP addresses and Layer 4 **port numbers** (NAPT).

---

## 1. RFC 1918 Private IP Address Spaces

These address blocks are reserved strictly for private internal networks and are **never routed on the public Internet**:

| Private Prefix | Subnet Mask | IP Range | Total Available Addresses |
| :--- | :--- | :--- | :---: |
| **`10.0.0.0/8`** | `255.0.0.0` | `10.0.0.0` – `10.255.255.255` | $16,777,216$ ($16.7\text{M}$) |
| **`172.16.0.0/12`** | `255.240.0.0` | `172.16.0.0` – `172.31.255.255` | $1,048,576$ ($1.04\text{M}$) |
| **`192.168.0.0/16`** | `255.255.0.0` | `192.168.0.0` – `192.168.255.255` | $65,536$ |

---

## 2. NAT Translation Mechanism (Step-by-Step Flow)

```mermaid
sequenceDiagram
    autonumber
    actor Host as LAN Host (10.0.0.1)
    participant NAT as NAT Router (Public: 138.76.29.7)
    actor Server as Web Server (128.119.40.186:80)

    Note over Host: 1. Outgoing Datagram<br>Src: 10.0.0.1:3345 -> Dst: 128.119.40.186:80
    Host->>NAT: Sends IP Datagram

    Note over NAT: 2. NAT Rewrite & Table Entry<br>Replaces Src: 10.0.0.1:3345 with 138.76.29.7:5001<br>Logs mapping in NAT Table
    NAT->>Server: Src: 138.76.29.7:5001 -> Dst: 128.119.40.186:80

    Note over Server: 3. Server Responds<br>Src: 128.119.40.186:80 -> Dst: 138.76.29.7:5001
    Server->>NAT: Sends Reply Datagram

    Note over NAT: 4. Inbound Lookup & Rewrite<br>Matches 138.76.29.7:5001 -> 10.0.0.1:3345<br>Replaces Dst IP/Port and re-checksums
    NAT->>Host: Dst: 10.0.0.1:3345
```

### The NAT Translation Table

| WAN (Public) Side Address | LAN (Private) Side Address | Protocol |
| :---: | :---: | :---: |
| `138.76.29.7, 5001` | `10.0.0.1, 3345` | TCP |
| `138.76.29.7, 5002` | `10.0.0.2, 3345` | TCP |
| `138.76.29.7, 5003` | `10.0.0.3, 4421` | UDP |

- **Connection Capacity:** Since the source port field is 16 bits ($2^{16} = 65,536$), a single public IP address can theoretically support **$> 60,000$ simultaneous active TCP/UDP connections**.

---

## 3. The NAT Traversal Problem (P2P & Inbound Connections)

> [!warning] The Inbound Connection Failure
> If a client outside the home network wants to initiate a connection to a private server (e.g., `10.0.0.1:80`) behind NAT, the inbound packet arriving at `138.76.29.7:80` is **instantly dropped** by the router because no entry exists in the NAT table for that incoming port!

```mermaid
flowchart TD
    ExtClient["External Client"] -->|SYN to 138.76.29.7:80| NAT["NAT Router"]
    NAT -.->|No NAT Table Match!| Dropped["❌ Packet Dropped!"]
    NAT --- PrivHost["Private Host (10.0.0.1:80)"]
```

### Three Standard NAT Traversal Solutions

1. **Static Port Forwarding:**
   - Network administrator manually configures a permanent table entry: `(138.76.29.7:80) -> (10.0.0.1:80)`.
2. **UPnP (Universal Plug and Play) / IGD:**
   - Client software behind NAT uses UPnP to automatically lease and configure a public port mapping on the router without human intervention.
3. **Relay Servers (TURN / STUN):**
   - Both peers establish an outbound connection to an external intermediate server (e.g., Skype/Zoom relay server), which bridges the two streams.

---

## 4. Architectural Controversies: Why Purists Hate NAT

1. **Layer Inversion:** Routers are strictly **Layer 3 devices**; modifying Layer 4 TCP/UDP port fields violates strict layered architecture.
2. **Violation of the End-to-End Principle:** The Internet architecture assumes hosts can communicate directly without intermediary stateful translation boxes.
3. **Interferes with P2P Applications:** Breaks direct peer-to-peer applications (VoIP, BitTorrent, gaming).
4. **Delayed IPv6 Adoption:** By artificially solving the IPv4 address shortage, NAT delayed the urgent global transition to IPv6.

---
#### Navigation
← Previous: [[06 - DHCP Protocol Mechanics]] | Next: [[08 - IPv6 Protocol & Tunneling Transitions]] →
