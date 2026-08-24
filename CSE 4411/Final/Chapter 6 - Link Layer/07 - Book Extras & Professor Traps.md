---
title: "07 - Book Extras & Professor Traps"
course: "CSE 4411"
chapter: 6
tags:
  - cse4411
  - networking
  - exam-traps
  - textbook-extras
  - link-layer
aliases:
  - Link Layer Exam Traps
  - Chapter 6 Extras
---

# 07 - Book Extras & Professor Traps (Link Layer)

> [!abstract] Key Takeaway
> This document details advanced theoretical edge cases from Kurose & Ross: **Cut-Through vs Store-and-Forward switching**, **CRC burst detection mathematics**, and top link layer exam traps.

---

## 1. Cut-Through vs Store-and-Forward Switching

```
Store-and-Forward:
[ Receive Entire Frame (64-1518B) ] ──► [ Verify CRC Checksum ] ──► [ Forward ]

Cut-Through:
[ Receive First 6 Bytes (Dst MAC) ] ──► [ Start Transmitting Immediately! ]
```

| Dimension | Store-and-Forward Switching | Cut-Through Switching |
| :--- | :--- | :--- |
| **Forwarding Latency** | High (Proportional to frame length: $L/R$). | **Minimal (~microseconds)** (Begins forwarding after reading the 6-byte Dst MAC). |
| **Error Handling** | **$100\%$ Filtered:** Corrupted frames failing CRC are dropped immediately. | **Propagates Errors:** Corrupted frames are forwarded across the switch before the bad CRC arrives at the end! |
| **Speed Matching** | Supports different input/output link rates (e.g., 100M to 10G). | Requires identical input and output link speeds. |

---

## 2. Top 5 Professor Traps in Link Layer

| # | Common Student Error | Ground Truth Reality | Exam Context |
| :---: | :--- | :--- | :--- |
| **1** | Setting Dst MAC to remote web server when sending cross-subnet. | Dst MAC is set to the **First-Hop Router (Gateway)** MAC; Dst IP is the remote server. | Cross-subnet frame header tracing. |
| **2** | Forgetting to append $r$ zeros to data $D$ before CRC division. | You must compute $\frac{D \cdot 2^r}{G}$, appending $r$ zeros to $D$ where $r = \text{degree of } G$. | CRC long division. |
| **3** | Thinking switches modify MAC addresses. | Switches are **transparent Layer 2 devices**; they **NEVER** modify source or destination MACs. (Only routers rewrite MACs). | Switched LAN conceptual problem. |
| **4** | Using CSMA/CD backoff slot time of $51.2\ \mu\text{s}$ for Gigabit Ethernet. | In Gigabit Ethernet (1000Base-T), slot time is extended to **4096 bit times ($4.096\ \mu\text{s}$)** via Carrier Extension. | CSMA/CD calculation. |
| **5** | Believing Pure ALOHA has the same efficiency as Slotted ALOHA. | Pure ALOHA max efficiency is **$\frac{1}{2e} \approx 18.4\%$** because its vulnerable period ($2 \times T$) is double Slotted ALOHA ($\frac{1}{e} \approx 36.8\%$). | Efficiency derivation. |

---
#### Navigation
← Previous: [[06 - Synthesis: A Day in the Life of a Web Request]] | Next: [[08 - Comprehensive Worked Numericals & Exam Problems]] →
