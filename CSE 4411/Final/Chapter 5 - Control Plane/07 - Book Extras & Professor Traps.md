---
title: "07 - Book Extras & Professor Traps"
course: "CSE 4411"
chapter: 5
tags:
  - cse4411
  - networking
  - exam-traps
  - textbook-extras
  - control-plane
aliases:
  - Control Plane Exam Traps
  - Chapter 5 Book Extras
---

# 07 - Book Extras & Professor Traps (Control Plane)

> [!abstract] Key Takeaway
> This document highlights nuanced edge cases, commercial BGP routing policies (**Gao-Rexford Model**), and algorithm failure modes that frequently separate top-scoring students on the CSE 4411 Final Exam.

---

## 1. Commercial BGP Relationships: The Gao-Rexford Model

In the global Internet, routing decisions are governed by **commercial contracts**, not shortest physical path!

```mermaid
flowchart TD
    Provider["Tier-1 Provider"]
    Peer1["ISP A (Peer)"] <===>|Settlement-Free Peering| Peer2["ISP B (Peer)"]
    
    Provider ===>|Transit (Paid)| Peer1
    Provider ===>|Transit (Paid)| Peer2
    
    Peer1 ===>|Transit (Paid)| Customer1["Customer Net C1"]
    Peer2 ===>|Transit (Paid)| Customer2["Customer Net C2"]
```

### The Rules of Valley-Free Routing

1. **Customer Routes:** An ISP will advertise routes learned from its customers to **everyone** (providers, peers, and other customers) because customer traffic generates revenue!
2. **Provider & Peer Routes:** An ISP will advertise routes learned from a provider or peer **ONLY to its paying customers**.
3. **The "Valley-Free" Rule:** An ISP will **NEVER transit traffic between two providers or two peers**:
   $$\text{Provider}_1 \not\to \text{ISP } X \not\to \text{Provider}_2 \quad (\text{ISP } X \text{ refuses to carry free transit})$$

---

## 2. The 3-Node Distance-Vector Loophole

> [!warning] Exam Trap: Why Poisoned Reverse Fails on 3-Node Topologies
> Consider a triangle of nodes $A$, $B$, and $C$, all connected to each other, with destination $X$ attached to $C$:

```
        (A)
       /   \
      /     \
    (B) === (C) ---- (X)
```

1. If link $(C, X)$ fails, $C$ sets $D_C(X) = \infty$.
2. Node $B$ routes to $X$ via $C$, so $B$ advertises $D_B(X) = \infty$ to $C$ (Poisoned Reverse).
3. **The Catch:** Node $B$ still advertises $D_B(X) = 2$ to node $A$!
4. Node $A$ sees $B$'s advertisement and updates $D_A(X) = 3$ (via $B$).
5. Node $A$ advertises $D_A(X) = 3$ to $C$.
6. Node $C$ sees $A$'s route and updates $D_C(X) = 4$ (via $A$).
7. Count-to-Infinity occurs despite poisoned reverse because loop spans $\ge 3$ nodes!

---

## 3. Top 5 Professor Traps in Control Plane

| # | Common Student Error | Ground Truth Reality | Where It Appears on Exams |
| :---: | :--- | :--- | :--- |
| **1** | Believing BGP always chooses the **shortest path**. | BGP chooses routes based on **Policy (`LOCAL_PREF`) first**; a 10-hop customer route is preferred over a 1-hop provider route to save transit money! | Multi-attribute BGP route selection question. |
| **2** | Stating that OSPF uses **TCP or UDP**. | OSPF runs **directly on IP (`Protocol = 89`)**. | Protocol stack matching table. |
| **3** | Thinking **Hot Potato Routing** minimizes total end-to-end delay. | Hot Potato only minimizes latency **within the local AS**; total global latency might be much worse. | Conceptual short-answer question. |
| **4** | Forgetting to update predecessor $p(v)$ during Dijkstra relaxation. | When $D(v)$ decreases during step $k$, $p(v)$ must be updated to current node $w$. | Step-by-step Dijkstra table completion. |
| **5** | Assuming ICMP is a **Transport Layer** protocol. | ICMP is strictly a **Network Layer** protocol carried inside IP datagrams (`Protocol = 1`). | Layer classification multiple choice. |

---
#### Navigation
← Previous: [[06 - ICMP & Traceroute Mechanics]] | Next: [[08 - Comprehensive Worked Numericals & Exam Problems]] →
