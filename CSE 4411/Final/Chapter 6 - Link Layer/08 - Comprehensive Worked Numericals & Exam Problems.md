---
title: "08 - Comprehensive Worked Numericals & Exam Problems"
course: "CSE 4411"
chapter: 6
tags:
  - cse4411
  - networking
  - practice-problems
  - worked-numericals
  - exam-prep
  - link-layer
aliases:
  - Link Layer Practice Problems
  - Chapter 6 Numericals
---

# 08 - Comprehensive Worked Numericals & Exam Problems (Link Layer)

> [!abstract] Exam Objective
> This problem set provides step-by-step mathematical solutions for **CRC Modulo-2 division**, **CSMA/CD backoff slot math**, **ALOHA efficiency**, and **Multi-Switch Table learning traces**.

---

## 🧮 Problem Set 1: Modulo-2 Polynomial CRC Division

### Problem Statement
- **Data Payload:** $D = 1101011011$ ($10\text{ bits}$).
- **Generator Polynomial:** $G(x) = x^4 + x + 1 \implies G = 10011$ ($r+1 = 5\text{ bits} \implies r = 4\text{ bits}$).

**Questions:**
1. Compute the 4-bit CRC sequence ($R$).
2. Write the actual transmitted bitstream.
3. Show how the receiver detects an error if the 4th bit from the right is flipped during transit.

---

### Step-by-Step Solution

#### Part 1: Compute CRC Remainder $R$
- Append $r = 4$ zeros to $D$:
  $$D \cdot 2^4 = 11010110110000$$

```
               1100001010  (Quotient)
          -----------------
10011  )  11010110110000
          10011
          -----
          010011
           10011
           -----
           0000010110
                 10011
                 -----
                 0010100
                   10011
                   -----
                   001110  (Remainder R = 1110)
```
- **CRC Checksum:** $R = \mathbf{1110}$.

#### Part 2: Transmitted Bitstream
$$\text{Transmitted Frame} = D \cdot 2^4 \oplus R = 11010110110000 \oplus 1110 = \mathbf{11010110111110}$$

#### Part 3: Receiver Error Detection
- If the 4th bit from the right is flipped ($1 \to 0$), received frame is $11010110110110$.
- Dividing $11010110110110$ by $G = 10011$ yields a **non-zero remainder ($R = 1001 \ne 0000$)**.
- The receiver **detects the corruption and discards the frame**.

---

## 🧮 Problem Set 2: CSMA/CD Minimum Frame Size & Backoff Math

### Problem Statement
A shared local network has length $d = 1.5\text{ km}$, signal propagation speed $v = 2 \times 10^8\text{ m/s}$, and operates at $R = 100\text{ Mbps}$ (Fast Ethernet).

1. **Calculate the minimum Ethernet frame size ($L_{min}$)** to ensure reliable collision detection.
2. **Backoff Calculation:** If two nodes collide for the **4th time ($m=4$)**, compute the range of possible randomized backoff wait times in microseconds.

---

### Step-by-Step Solution

#### Part 1: Minimum Frame Size
- One-way propagation delay:
  $$t_{prop} = \frac{d}{v} = \frac{1500\text{ m}}{2 \times 10^8\text{ m/s}} = 7.5\ \mu\text{s}$$
- Round-trip propagation delay:
  $$2 \cdot t_{prop} = 2 \times 7.5\ \mu\text{s} = 15.0\ \mu\text{s}$$
- Minimum transmission time $t_{trans} \ge 2 \cdot t_{prop}$:
  $$L_{min} \ge 2 \cdot t_{prop} \times R = 15.0 \times 10^{-6}\text{ s} \times 100 \times 10^6\text{ bps} = \mathbf{1500\text{ bits}} = \mathbf{187.5\text{ Bytes}}$$

---

#### Part 2: 4th Collision Backoff Delay
- For $m = 4$, the random multiplier $K$ is chosen from:
  $$K \in \{0, 1, 2, \dots, 2^4 - 1\} = \{0, 1, 2, \dots, 15\}$$
- Each backoff slot time on 100 Mbps Ethernet is $512\text{ bit times}$:
  $$\text{Slot Time} = \frac{512\text{ bits}}{100 \times 10^6\text{ bps}} = 5.12\ \mu\text{s}$$
- **Range of Possible Backoff Delays:**
  - Minimum ($K=0$): $0 \times 5.12\ \mu\text{s} = \mathbf{0\ \mu\text{s}}$
  - Maximum ($K=15$): $15 \times 5.12\ \mu\text{s} = \mathbf{76.8\ \mu\text{s}}$

---

## 🧮 Problem Set 3: Pure ALOHA vs Slotted ALOHA Throughput

### Problem Statement
A broadcast radio channel operates at bandwidth $R = 56\text{ kbps}$. Each frame has length $L = 1000\text{ bits}$. 

**Question:** Compute the maximum usable throughput (in bits/sec) for:
1. Pure ALOHA
2. Slotted ALOHA

---

### Step-by-Step Solution

#### 1. Pure ALOHA
$$\text{Max Efficiency } \eta_{pure} = \frac{1}{2e} \approx 0.1839$$
$$\text{Max Throughput} = 56,000\text{ bps} \times 0.1839 = \mathbf{10,298.4\text{ bps}} \approx \mathbf{10.3\text{ kbps}}$$

#### 2. Slotted ALOHA
$$\text{Max Efficiency } \eta_{slotted} = \frac{1}{e} \approx 0.3679$$
$$\text{Max Throughput} = 56,000\text{ bps} \times 0.3679 = \mathbf{20,602.4\text{ bps}} \approx \mathbf{20.6\text{ kbps}}$$
*(Slotted ALOHA provides exactly double the throughput of Pure ALOHA!)*

---
#### Navigation
← Previous: [[07 - Book Extras & Professor Traps]] | Next: [[00 - Index]] (Physical Layer) →
