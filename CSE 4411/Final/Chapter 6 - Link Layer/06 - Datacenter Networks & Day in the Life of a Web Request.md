---
title: "06 - Datacenter Networks & Day in the Life of a Web Request"
course: "CSE 4411"
chapter: 6
section: 6.6 - 6.7
tags:
  - cse4411
  - networking
  - datacenter-networks
  - fat-tree
  - day-in-the-life
  - protocol-synthesis
aliases:
  - Datacenter Networks
  - A Day in the Life of a Web Request
---

# 06 - Datacenter Networks & Day in the Life of a Web Request

> [!abstract] Key Takeaway
> - **Datacenter Networks** use multi-tier **Fat-Tree (Clos) topologies** and **Equal-Cost Multi-Path (ECMP)** to provide massive bisection bandwidth between server blades.
> - **"A Day in the Life of a Web Request"** synthesizes all 5 layers: **DHCP** (config) $\to$ **ARP** (L2 binding) $\to$ **DNS** (name resolution) $\to$ **TCP Handshake** (transport connection) $\to$ **HTTP** (application data).

---

## 1. Datacenter Network Architecture

```mermaid
flowchart TD
    CR1["Border / Core Router 1"] --- AG1["Aggregation Switch 1"]
    CR1 --- AG2["Aggregation Switch 2"]
    CR2["Border / Core Router 2"] --- AG1
    CR2 --- AG2

    AG1 --- ToR1["Top-of-Rack Switch 1"]
    AG1 --- ToR2["Top-of-Rack Switch 2"]
    AG2 --- ToR1
    AG2 --- ToR2

    ToR1 --- S1["Server Blade 1"]
    ToR1 --- S2["Server Blade 2"]
    ToR2 --- S3["Server Blade 3"]
    ToR2 --- S4["Server Blade 4"]
```

- **Top-of-Rack (ToR) Switch:** Connects 20–40 servers within a single physical rack.
- **Fat-Tree / Clos Topology:** Multiple redundant links connect every ToR switch to all aggregation switches, providing **Equal-Cost Multi-Path (ECMP)** load balancing and preventing single points of failure.

---

## 2. Master Synthesis: "A Day in the Life of a Web Request"

```mermaid
sequenceDiagram
    autonumber
    actor Laptop as Connecting Laptop
    participant Switch as Local Switch
    actor Router as Default Gateway Router
    actor DNS as DNS Server (8.8.8.8)
    actor Web as Web Server (www.google.com)

    Note over Laptop,Router: Phase 1: DHCP Network Configuration
    Laptop->>Router: 1. DHCP Discover (UDP, IP Broadcast, Eth Broadcast)
    Router->>Laptop: 2. DHCP ACK (Gives IP: 68.85.2.101, Mask /24, Gateway: 68.85.2.1, DNS: 8.8.8.8)

    Note over Laptop,Router: Phase 2: ARP Layer 2 Resolution
    Laptop->>Switch: 3. ARP Request: "Who has Gateway IP 68.85.2.1?" (Eth Broadcast)
    Router->>Laptop: 4. ARP Reply: "68.85.2.1 is at Gateway MAC 00:22:6B:45:1F:B1"

    Note over Laptop,DNS: Phase 3: DNS Name Resolution
    Laptop->>Router: 5. DNS Query UDP (Dest: 8.8.8.8:53, Dest MAC: Gateway MAC)
    Router->>DNS: 6. Routed across Internet (BGP/OSPF) to DNS Server
    DNS->>Laptop: 7. DNS Reply: "www.google.com is at 142.250.190.46"

    Note over Laptop,Web: Phase 4: TCP 3-Way Handshake
    Laptop->>Web: 8. TCP SYN (Port 80 / 443)
    Web->>Laptop: 9. TCP SYN-ACK
    Laptop->>Web: 10. TCP ACK + HTTP GET /

    Note over Laptop,Web: Phase 5: HTTP Data Delivery
    Web->>Laptop: 11. HTTP 200 OK (Contains Web Page Payload)
    Note over Laptop: Browser renders HTML/CSS web page!
```

---

## 3. Comprehensive Protocol Encapsulation Map

| Protocol Step | Application Layer | Transport Layer | Network Layer | Link Layer |
| :--- | :--- | :--- | :--- | :--- |
| **1. DHCP** | DHCP Message (`yiaddr`) | UDP (`68` $\to$ `67`) | IP (`0.0.0.0` $\to$ `255.255.255.255`) | Ethernet (`Src MAC` $\to$ `FF:FF:FF:FF:FF:FF`) |
| **2. ARP** | — | — | Target IP: `68.85.2.1` | Ethernet (`Type: 0x0806`, `FF:FF:FF:FF:FF:FF`) |
| **3. DNS** | DNS Query (`google.com`) | UDP (`Src Port` $\to$ `53`) | IP (`68.85.2.101` $\to$ `8.8.8.8`) | Ethernet (`Src MAC` $\to$ `Gateway MAC`) |
| **4. TCP Handshake**| — | TCP (`SYN`, `SYN-ACK`, `ACK`) | IP (`68.85.2.101` $\to$ `142.250.190.46`) | Ethernet (`Src MAC` $\to$ `Gateway MAC`) |
| **5. HTTP Transfer**| HTTP GET / HTTP 200 OK | TCP (Port `80` / `443`) | IP (`68.85.2.101` $\to$ `142.250.190.46`) | Ethernet (`Src MAC` $\to$ `Gateway MAC`) |

---
#### Navigation
← Previous: [[05 - VLANs (802.1Q) & MPLS]] | Next: [[07 - Book Extras & Professor Traps]] →
