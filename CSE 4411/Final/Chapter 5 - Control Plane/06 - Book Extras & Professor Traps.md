---
title: "06 - Book Extras & Professor Traps"
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
  - Chapter 5 Extras
---

# 06 - Book Extras & Professor Traps (Control Plane)

> [!abstract] Key Takeaway
> This document details advanced theoretical edge cases from Kurose & Ross: **Gao-Rexford Valley-Free BGP Routing**, **3-Node Loop failures**, and high-frequency exam traps.

---

## 1. Commercial BGP Relationships: The Gao-Rexford Model

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
1. **Customer Routes:** An ISP will advertise routes learned from its customers to **everyone** (providers, peers, and customers) because customer traffic generates revenue.
2. **Provider & Peer Routes:** An ISP will advertise routes learned from a provider or peer **ONLY to its paying customers**.
3. **The "Valley-Free" Rule:** An ISP will **NEVER transit traffic between two providers or two peers**:
   $$\text{Provider}_1 \not\to \text{ISP } X \not\to \text{Provider}_2$$

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
7. Count-to-Infinity occurs despite poisoned reverse because the loop spans $\ge 3$ nodes!

---

## 3. Top 5 Professor Traps in Control Plane

| # | Common Student Error | Ground Truth Reality | Exam Context |
| :---: | :--- | :--- | :--- |
| **1** | Believing BGP always chooses the **shortest path**. | BGP chooses routes based on **Policy (`LOCAL_PREF`) first**; a longer customer path is chosen over a shorter provider path to save money! | Multi-attribute BGP selection. |
| **2** | Stating that OSPF uses **TCP or UDP**. | OSPF runs **directly on IP (`Protocol = 89`)**. | Protocol stack classification. |
| **3** | Thinking **Hot Potato Routing** minimizes total end-to-end delay. | Hot Potato only minimizes latency **within the local AS**. | Conceptual short answer. |
| **4** | Forgetting to update predecessor $p(v)$ during Dijkstra relaxation. | When $D(v)$ decreases during step $k$, $p(v)$ must be updated to current node $w$. | Dijkstra table completion. |
| **5** | Assuming ICMP is a **Transport Layer** protocol. | ICMP is strictly a **Network Layer** protocol carried inside IP datagrams (`Protocol = 1`). | Layer classification. |

---
#### Navigation
← Previous: [[05 - ICMP & Traceroute Mechanics]] | Next: [[07 - Comprehensive Worked Numericals & Exam Problems]] →
