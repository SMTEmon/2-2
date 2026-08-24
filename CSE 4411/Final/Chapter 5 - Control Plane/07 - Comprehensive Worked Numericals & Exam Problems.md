---
title: "07 - Comprehensive Worked Numericals & Exam Problems"
course: "CSE 4411"
chapter: 5
tags:
  - cse4411
  - networking
  - practice-problems
  - worked-numericals
  - exam-prep
  - control-plane
aliases:
  - Chapter 5 Practice Problems
  - Routing Numericals
---

# 07 - Comprehensive Worked Numericals & Exam Problems (Control Plane)

> [!abstract] Exam Objective
> This document contains full, step-by-step solutions for **Dijkstra's Link-State tables**, **Distance-Vector Bellman-Ford convergence matrices**, and **BGP Route Selection** problems.

---

## 🧮 Problem Set 1: Dijkstra's Algorithm Execution Table

### Problem Statement
Given the 6-node network graph below, compute the shortest paths and forwarding table from source node **$u$** using Dijkstra's Algorithm.

```
          5
     v -------- w
   / | \      / | \
  2  |  3    1  |  5
 /   2   \  /   |   \
u -- x --- y ---+--- z
  1     1     2
```
*Link Costs:* $c(u,v)=2, c(u,x)=1, c(v,x)=2, c(v,w)=5, c(v,y)=3, c(x,y)=1, c(w,y)=1, c(w,z)=5, c(y,z)=2$.

---

### Step-by-Step Dijkstra Table

| Step | Confirmed Set $N'$ | $D(v), p(v)$ | $D(w), p(w)$ | $D(x), p(x)$ | $D(y), p(y)$ | $D(z), p(z)$ |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: |
| **0** | $\{u\}$ | $2, u$ | $\infty$ | **$1, u$** *(Min)* | $\infty$ | $\infty$ |
| **1** | $\{u, x\}$ | **$2, u$** *(Min)* | $\infty$ | $1, u$ | $2, x$ | $\infty$ |
| **2** | $\{u, x, v\}$ | $2, u$ | $5, v$ | $1, u$ | **$2, x$** *(Min)* | $\infty$ |
| **3** | $\{u, x, v, y\}$ | $2, u$ | **$3, y$** *(Min)* | $1, u$ | $2, x$ | $4, y$ |
| **4** | $\{u, x, v, y, w\}$ | $2, u$ | $3, y$ | $1, u$ | $2, x$ | **$4, y$** *(Min)* |
| **5** | $\{u, x, v, y, w, z\}$ | $2, u$ | $3, y$ | $1, u$ | $2, x$ | $4, y$ |

---

### Final Forwarding Table at Node $u$

| Destination | Shortest Path | Total Cost | Next-Hop Interface |
| :---: | :--- | :---: | :---: |
| **$v$** | $u \to v$ | **2** | $(u, v)$ |
| **$x$** | $u \to x$ | **1** | $(u, x)$ |
| **$y$** | $u \to x \to y$ | **2** | $(u, x)$ |
| **$w$** | $u \to x \to y \to w$ | **3** | $(u, x)$ |
| **$z$** | $u \to x \to y \to z$ | **4** | $(u, x)$ |

---

## 🧮 Problem Set 2: Distance-Vector Routing Iterations

### Problem Statement
A 3-node network has links: $c(x,y)=2$, $c(y,z)=1$, and $c(x,z)=7$. Show the distance vector tables at node $x$, node $y$, and node $z$ until full convergence.

```
       (x)
      /   \
    2/     \7
    /       \
  (y) ===== (z)
        1
```

---

### Step-by-Step Convergence

#### Iteration 0 (Initial State)
- **Node $x$ Table:** $D_x(x)=0, D_x(y)=2, D_x(z)=7$
- **Node $y$ Table:** $D_y(x)=2, D_y(y)=0, D_y(z)=1$
- **Node $z$ Table:** $D_z(x)=7, D_z(y)=1, D_z(z)=0$

#### Iteration 1 (After Vector Exchange)
- **Node $x$ computes $D_x(z)$ using Bellman-Ford:**
  $$D_x(z) = \min \{ c(x,y) + D_y(z), c(x,z) + D_z(z) \} = \min \{ 2 + 1, 7 + 0 \} = \mathbf{3} \quad (\text{via } y)$$
- **Node $z$ computes $D_z(x)$ using Bellman-Ford:**
  $$D_z(x) = \min \{ c(z,y) + D_y(x), c(z,x) + D_x(x) \} = \min \{ 1 + 2, 7 + 0 \} = \mathbf{3} \quad (\text{via } y)$$

#### Iteration 2 (Stable State)
- $x$ and $z$ announce their new vectors ($D_x(z)=3$, $D_z(x)=3$).
- No further path costs decrease. Algorithm terminates.

---

## 🧮 Problem Set 3: BGP Multi-Attribute Route Selection

### Problem Statement
Router $R1$ in AS 100 learns three BGP routes to destination prefix `192.0.2.0/24`:

| Route | `LOCAL_PREF` | `AS-PATH` | `NEXT-HOP` Router | OSPF Cost to `NEXT-HOP` | Learned Via |
| :---: | :---: | :--- | :---: | :---: | :---: |
| **Route A** | 100 | `AS 200 -> AS 300` (2 hops) | Gateway 1 | **10** | eBGP |
| **Route B** | 100 | `AS 400 -> AS 500` (2 hops) | Gateway 2 | **25** | eBGP |
| **Route C** | 150 | `AS 600 -> AS 700 -> AS 800` (3 hops)| Gateway 2 | 25 | iBGP |

**Question:** Which route will Router $R1$ select?

### Solution
1. Check `LOCAL_PREF` (Highest value wins):
   - Route A: 100, Route B: 100, **Route C: 150**.
2. **Decision:** **Route C is immediately selected!** (`LOCAL_PREF` overrides shorter AS-PATH and lower OSPF cost).

---
#### Navigation
← Previous: [[06 - Book Extras & Professor Traps]] | Next: [[00 - Index]] (Chapter 6) →
