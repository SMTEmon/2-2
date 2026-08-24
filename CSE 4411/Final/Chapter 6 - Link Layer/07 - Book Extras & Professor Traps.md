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
  - Chapter 6 Book Extras
---

# 07 - Book Extras & Professor Traps (Link Layer & LANs)

> [!abstract] Key Takeaway
> This document details advanced textbook-specific topics from Kurose & Ross: **Cut-Through vs Store-and-Forward switching**, mathematical proofs for **CRC error-detection bounds**, and high-frequency exam traps.

---

## 1. Store-and-Forward vs Cut-Through Switching

```
Store-and-Forward Switching:
[ Frame Header ... Payload ... CRC Trailer ] ──► Fully Buffered ──► CRC Verified ──► Forwarded out Port
Latency = L / R (Full transmission delay of frame)

Cut-Through Switching:
[ Dst MAC (6B) ] ──► Lookup Table ──► IMMEDIATELY Start Transmitting out Port!
Latency = A few nanoseconds (Lookup delay only; rest of frame streams through switch fabric)
```

| Dimension | Store-and-Forward Switching | Cut-Through Switching |
| :--- | :--- | :--- |
| **Error Handling** | Drops all CRC-corrupted frames before forwarding. | **Propagates corrupt frames and collision fragments** across the network! |
| **Speed Conversion** | Supports switching between links of different speeds (e.g., 10G to 1G). | Can only cut-through if output link speed $\ge$ input link speed. |
| **Typical Deployment** | Standard campus enterprise switches. | High-frequency trading (HFT) and ultra-low-latency AI datacenter clusters. |

---

## 2. Mathematical Error Detection Bounds of CRC

For a Generator Polynomial $G(x)$ of degree $r$:

1. **Burst Errors of Length $\le r$:** Detected with **$100\%$ mathematical certainty**.
2. **Burst Errors of Length $= r + 1$:** Undetected with probability only $\left(\frac{1}{2}\right)^{r-1}$ (For CRC-32, $r=32 \implies \text{Error Miss Probability} \approx 4.65 \times 10^{-10}$).
3. **Burst Errors of Length $> r + 1$:** Undetected with probability $\left(\frac{1}{2}\right)^r \approx 2.33 \times 10^{-10}$.
4. **Any Odd Number of Bit Errors:** Detected with **$100\%$ certainty** provided $G(x)$ contains $(x+1)$ as a factor.

---

## 3. Top 5 Professor Traps in Link Layer & LANs

| # | Common Student Error | Ground Truth Reality | Exam Context |
| :---: | :--- | :--- | :--- |
| **1** | Thinking a switch learns ports from **Destination MAC**. | A switch learns and populates its table strictly from the **Source MAC** of arriving frames. | Trace table completion for an uninitialized switch. |
| **2** | Believing an ARP request contains the **destination MAC**. | The destination MAC of an ARP Request is `FF:FF:FF:FF:FF:FF` (Broadcast), because the sender does not know the destination MAC! | Protocol trace header analysis. |
| **3** | Forgetting to add $r$ zeros to data before CRC division. | Data $D$ must be shifted by $r$ bits ($D \cdot 2^r$) before performing Modulo-2 polynomial division. | CRC calculation problem. |
| **4** | Assuming Slotted ALOHA has $100\%$ efficiency. | Slotted ALOHA max efficiency is capped at **$1/e \approx 36.8\%$** due to idle slots and collisions. | MAC protocol efficiency derivation. |
| **5** | Confusing Collision Domain with Broadcast Domain. | A **Switch port** isolates Collision Domains; a **Router interface** or **VLAN** isolates Broadcast Domains. | Network topology design question. |

---
#### Navigation
← Previous: [[06 - Datacenter Networks & Day in the Life of a Web Request]] | Next: [[08 - Comprehensive Worked Numericals & Exam Problems]] →
