---
title: "06 - Synthesis: A Day in the Life of a Web Request"
course: "CSE 4411"
chapter: 6
section: 6.7
tags:
  - cse4411
  - networking
  - protocol-synthesis
  - day-in-the-life
  - end-to-end
  - final-exam
aliases:
  - Day in the Life of a Web Request
  - Protocol Synthesis
---

# 06 - Synthesis: A Day in the Life of a Web Request

> [!abstract] Key Takeaway
> This synthesis connects all 5 layers of the network architecture (**DHCP $\to$ ARP $\to$ DNS $\to$ TCP $\to$ HTTP**) into a single end-to-end trace from connecting a laptop to downloading a webpage.

---

## 1. The 6-Phase End-to-End Protocol Flow

```mermaid
sequenceDiagram
    autonumber
    actor Client as Arriving Laptop
    participant Switch as Local Switch
    actor DHCP as DHCP Server
    actor Router as Default Gateway Router
    actor DNS as Local DNS Server
    actor WebServer as Remote Web Server (google.com)

    Note over Client,DHCP: PHASE 1: Bootstrapping IP Configuration (DHCP)
    Client->>DHCP: DHCP Discover (UDP 67 Broadcast via Ethernet)
    DHCP->>Client: DHCP ACK (Delivers: Client IP, Netmask, Gateway IP, DNS IP)

    Note over Client,Router: PHASE 2: Finding Gateway Hardware MAC (ARP)
    Client->>Router: ARP Request Broadcast ("Who has Gateway IP?")
    Router->>Client: ARP Reply Unicast ("Gateway MAC is 00:22:6B:...")

    Note over Client,DNS: PHASE 3: Domain Name Resolution (DNS)
    Client->>DNS: DNS Query for "google.com" (UDP 53)
    DNS->>Client: DNS Reply (Delivers Web Server IP: 142.250.190.46)

    Note over Client,WebServer: PHASE 4: Transport Layer Connection (TCP 3-Way Handshake)
    Client->>WebServer: TCP SYN (Port 80/443)
    WebServer->>Client: TCP SYN-ACK
    Client->>WebServer: TCP ACK

    Note over Client,WebServer: PHASE 5: Application Layer Request (HTTP)
    Client->>WebServer: HTTP GET /index.html
    WebServer->>Client: HTTP 200 OK (Transfers HTML Page & Images)
```

---

## 2. Comprehensive Encapsulation Trace: The Initial HTTP GET

When the client browser issues `GET /index.html`, the operating system encapsulates data down through all 5 layers:

```
[ Application Layer (HTTP) ]
  GET /index.html HTTP/1.1\r\nHost: google.com\r\n\r\n
  
[ Transport Layer (TCP) ]
  +--------------------+--------------------+--------------------+
  | Src Port: 54321    | Dst Port: 80       | Seq: 1, ACK: 1     |
  +--------------------+--------------------+--------------------+

[ Network Layer (IP) ]
  +--------------------+--------------------+--------------------+
  | Src IP: 192.168.1.100 | Dst IP: 142.250.190.46 | Protocol: 6 (TCP) |
  +--------------------+--------------------+--------------------+

[ Link Layer (Ethernet) ]
  +--------------------+--------------------+--------------------+
  | Src MAC: Laptop NIC| Dst MAC: Gateway   | EtherType: 0x0800  |
  +--------------------+--------------------+--------------------+
  
[ Physical Layer ]
  Bits modulated as electrical/optical signals over the wire.
```

---
#### Navigation
← Previous: [[05 - Virtual LANs (VLANs & IEEE 802.1Q)]] | Next: [[07 - Book Extras & Professor Traps]] →
