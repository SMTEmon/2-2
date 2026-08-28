---
title: "Chapter 5: Network Layer - Control Plane"
course: "CSE 4411"
chapter: 5
tags:
  - cse4411
  - networking
  - control-plane
  - routing
  - final-exam
aliases:
  - Network Layer - Control Plane
  - Chapter 5 Routing
---

# Chapter 5: Network Layer — Control Plane

> [!abstract] Executive Summary & Roadmap
> The **Control Plane** coordinates the network-wide logic that determines how datagrams are routed end-to-end across routers.
> 
> This vault covers the core routing algorithms (**Dijkstra's Link-State** and **Bellman-Ford Distance-Vector**), modern routing protocols (**OSPF** and **BGP-4**), and network diagnostics via **ICMP and Traceroute**. *(Note: SDN Control Plane and Network Management Configuration are excluded from the Final Exam syllabus).*

---

## ✅ Chapter 5 Study Progress Checklist
- [ ] [[01 - Link-State Routing & Dijkstra's Algorithm]] — Shortest Path Tree, $O(|E|+|V|\log|V|)$, Oscillations
- [ ] [[02 - Distance-Vector Routing & Bellman-Ford]] — Bellman-Ford Eq, Count-to-Infinity, Poisoned Reverse
- [ ] [[03 - Intra-AS Routing & OSPF]] — Protocol 89, MD5 Auth, ECMP, Hierarchical Area 0
- [ ] [[04 - Inter-AS Routing & BGP-4]] — eBGP/iBGP, `AS-PATH`, Policy Tie-Breakers, Hot Potato Routing
- [ ] [[05 - ICMP & Traceroute Mechanics]] — Type/Code Matrix (0, 8, 3, 11), UDP Probe Walkthrough
- [ ] [[06 - Book Extras & Professor Traps]] — Gao-Rexford Valley-Free, 3-Node Loophole Proof
- [ ] [[07 - Comprehensive Worked Numericals & Exam Problems]] — Dijkstra Matrix, DV Tables, BGP Selection

---

## 🗺️ Master Visual Navigation Map

```mermaid
flowchart TD
    Ch5["Chapter 5: Final Exam Scope<br>(Routing & Control Plane)"]
    
    Ch5 --> Sec1["[[01 - Link-State Routing & Dijkstra's Algorithm]]<br>Global Topology, Dijkstra Math, SPT & Oscillations"]
    Ch5 --> Sec2["[[02 - Distance-Vector Routing & Bellman-Ford]]<br>Decentralized Updates, Count-to-Infinity & Poisoned Reverse"]
    Ch5 --> Sec3["[[03 - Intra-AS Routing & OSPF]]<br>Protocol 89, MD5 Security, ECMP & Hierarchical Areas"]
    Ch5 --> Sec4["[[04 - Inter-AS Routing & BGP-4]]<br>Path Vector, eBGP/iBGP, Policy & Tie-Breaking Priority"]
    Ch5 --> Sec5["[[05 - ICMP & Traceroute Mechanics]]<br>Error Reporting, Ping & Traceroute TTL Probing"]
    Ch5 --> Sec6["[[06 - Book Extras & Professor Traps]]<br>Gao-Rexford Valley-Free Routing & 3-Node Loopholes"]
    Ch5 --> Sec7["[[07 - Comprehensive Worked Numericals & Exam Problems]]<br>Step-by-Step Dijkstra, Distance-Vector & BGP Selection"]
```

---

## 📑 Detailed Note Registry

| # | Note Document | Core Question Answered | High-Yield Topics |
| :---: | :--- | :--- | :--- |
| **01** | [[01 - Link-State Routing & Dijkstra's Algorithm]] | *How does Dijkstra's algorithm find least-cost paths globally?* | Dijkstra Steps ($N', D(v), p(v)$), Complexity ($O(\|E\| + \|V\|\log\|V\|)$), Traffic Oscillations |
| **02** | [[02 - Distance-Vector Routing & Bellman-Ford]] | *How do decentralized routers converge without global topology?* | Bellman-Ford Eq ($d_x(y) = \min_v \{ c(x,v) + d_v(y) \}$), Count-to-Infinity, Poisoned Reverse (2-node vs 3-node) |
| **03** | [[03 - Intra-AS Routing & OSPF]] | *How does an Autonomous System route internal traffic securely?* | OSPF Features (MD5, ECMP), Hierarchical OSPF (Area 0 Backbone, ABRs, ASBRs) |
| **04** | [[04 - Inter-AS Routing & BGP-4]] | *How do competing global ISPs negotiate traffic routing?* | eBGP vs iBGP, Path Attributes (`AS-PATH`, `NEXT-HOP`), Policy Tie-Breakers, Hot Potato Routing |
| **05** | [[05 - ICMP & Traceroute Mechanics]] | *How does the Internet diagnose faults and map router hops?* | ICMP Type/Code Matrix (Type 0/8 Echo, Type 3 Unreachable, Type 11 TTL Exceeded), Traceroute |
| **06** | [[06 - Book Extras & Professor Traps]] | *What subtle edge cases appear on high-difficulty exams?* | Valley-Free Routing (Gao-Rexford), Poisoned Reverse Loophole, Hot-Potato vs BGP Policy |
| **07** | [[07 - Comprehensive Worked Numericals & Exam Problems]] | *How do you solve routing algorithm tables with zero errors?* | Full Dijkstra execution matrices, Distance-Vector routing tables with link cost changes |

---
#### Navigation
Next → [[01 - Link-State Routing & Dijkstra's Algorithm]]
