---
title: "04 - Book Extras & Professor Traps"
course: "CSE 4411"
chapter: 4
tags:
  - cse4411
  - networking
  - exam-traps
  - textbook-extras
  - dhcp
  - nat
  - ipv6
aliases:
  - Chapter 4 Exam Traps
  - DHCP to IPv6 Extras
---

# 04 - Book Extras & Professor Traps (DHCP to IPv6)

> [!abstract] Key Takeaway
> This document highlights nuanced edge cases from the Kurose & Ross reading list on **DHCP, NAT, and IPv6** that frequently appear on final examinations.

---

## 1. Top 5 Professor Traps in Chapter 4 Scope

| # | Common Student Error | Ground Truth Reality | Exam Context |
| :---: | :--- | :--- | :--- |
| **1** | Believing DHCP Request is sent via **unicast** to the offering server. | DHCP Request is sent via **Broadcast (`255.255.255.255`)** so all other DHCP servers know their offers were rejected. | Handshake flow analysis. |
| **2** | Confusing IPv4 **Total Length** with IPv6 **Payload Length**. | IPv4 Total Length includes header ($20 + \text{data}$). IPv6 Payload Length counts **only bytes following the 40B base header**. | Header byte extraction problem. |
| **3** | Thinking routers fragment oversized IPv6 packets. | Routers **never fragment IPv6 packets**; they drop them and send ICMPv6 Type 2 ("Packet Too Big"). | IPv4 vs IPv6 comparison. |
| **4** | Forgetting NAT table translates **Layer 4 Ports**. | NAT rewrites both **IP addresses AND TCP/UDP port numbers** (NAPT) to support $>60,000$ connections. | NAT mechanism short answer. |
| **5** | Assuming IPv6 tunneling requires upgrading core IPv4 routers. | IPv4 core routers only see standard IPv4 datagrams with `Protocol = 41`; **zero core router upgrades required**. | Transition strategy design. |

---

## 2. IPv6 Extension Header Chaining Order

```
[ IPv6 Base Header ] ──► [ Hop-by-Hop Options ] ──► [ Routing Header ] ──► [ Fragment Header ] ──► [ TCP / Payload ]
 (Next Header = 0)         (Next Header = 43)        (Next Header = 44)       (Next Header = 6)
```

- Each extension header starts with a 1-byte **Next Header** field pointing to the subsequent header in the chain, ending with the transport protocol code (`6` for TCP, `17` for UDP).

---
#### Navigation
← Previous: [[03 - IPv6 Protocol & Tunneling Transitions]] | Next: [[05 - Comprehensive Worked Numericals & Exam Problems]] →
