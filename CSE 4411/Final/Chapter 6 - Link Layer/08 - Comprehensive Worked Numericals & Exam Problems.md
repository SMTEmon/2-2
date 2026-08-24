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
  - crc
  - csma-cd
aliases:
  - Chapter 6 Practice Problems
  - Link Layer Numericals
---

# 08 - Comprehensive Worked Numericals & Exam Problems (Link Layer)

> [!abstract] Exam Objective
> This document contains full, step-by-step solutions for **CRC Modulo-2 polynomial divisions**, **CSMA/CD minimum frame size derivations**, and **ALOHA efficiency** problems.

---

## 🧮 Problem Set 1: CRC Long Division & Error Detection

### Problem Statement
Given:
- Data bitstream $D = 1010001101$ ($d = 10\text{ bits}$).
- Generator polynomial $G(x) = x^5 + x^4 + x^2 + 1 \implies G = 110101$ ($r = 5\text{ bits}$).

**Questions:**
1. Calculate the 5-bit CRC checksum $R$.
2. What is the transmitted bit sequence $T$?
3. If the channel corrupts the 4th bit from the right during transmission, show how the receiver detects the error.

---

### Step-by-Step Solution

#### 1. Calculate $R = \text{remainder}\left(\frac{D \cdot 2^5}{G}\right)$
- Append $r=5$ zeros to $D$: $D \cdot 2^5 = 101000110100000$.
- Perform Modulo-2 division by $G = 110101$:

```
               1110011010   (Quotient)
          ------------------
110101  ) 101000110100000
          110101
          ------
          0111011
           110101
           ------
           00111001
             110101
             ------
             00110000
               110101
               ------
               000101000
                 110101
                 ------
                 0111010
                  110101
                  ------
                  0011110   (Remainder R = 01110)
```

**CRC Checksum $R = \mathbf{01110}$**.

---

#### 2. Transmitted Sequence ($T$)
$$T = D \cdot 2^5 \oplus R = 1010001101\mathbf{01110}$$

---

#### 3. Error Detection on Bit Inversion
- Transmitted $T = 101000110101110$.
- 4th bit from right flipped: Received $T' = 10100011010\mathbf{0}110$.
- Receiver divides $T'$ by $G = 110101 \implies \text{Remainder} = 10001 \ne 00000 \implies$ **Error Detected! Frame is dropped.**

---

## 🧮 Problem Set 2: CSMA/CD Minimum Frame Size & Backoff Math

### Problem Statement
A $1\text{ Gbps}$ CSMA/CD local area network spans a total distance of $d = 1\text{ km}$ across a copper cable where propagation speed is $v = 2 \times 10^8\text{ m/s}$.

**Calculate:**
1. The one-way propagation delay ($t_{prop}$).
2. The minimum frame size ($L_{min}$) required to guarantee collision detection.
3. If two nodes experience their **2nd collision** ($m=2$), list all possible backoff delay times (in $\mu\text{s}$) that each node could choose.

---

### Step-by-Step Solution

#### 1. One-Way Propagation Delay ($t_{prop}$)
$$t_{prop} = \frac{d}{v} = \frac{1000\text{ m}}{2 \times 10^8\text{ m/s}} = 5 \times 10^{-6}\text{ s} = \mathbf{5\ \mu\text{s}}$$

#### 2. Minimum Frame Size ($L_{min}$)
$$t_{trans} \ge 2 \cdot t_{prop} \implies \frac{L_{min}}{R} \ge 2 \times 5\ \mu\text{s} = 10\ \mu\text{s}$$
$$L_{min} \ge 10 \times 10^{-6}\text{ s} \times 10^9\text{ bps} = 10,000\text{ bits} = \mathbf{1250\text{ Bytes}}$$

#### 3. Backoff Delay Choices for $m=2$
- $K \in \{0, 1, 2, \dots, 2^2 - 1\} = \{0, 1, 2, 3\}$.
- Slot time $= 2 \cdot t_{prop} = 10\ \mu\text{s}$.
- Possible wait times:
  - $K = 0 \implies \mathbf{0\ \mu\text{s}}$
  - $K = 1 \implies \mathbf{10\ \mu\text{s}}$
  - $K = 2 \implies \mathbf{20\ \mu\text{s}}$
  - $K = 3 \implies \mathbf{30\ \mu\text{s}}$

---
#### Navigation
← Previous: [[07 - Book Extras & Professor Traps]] | Next: [[00 - Index]] (Physical Layer) →
