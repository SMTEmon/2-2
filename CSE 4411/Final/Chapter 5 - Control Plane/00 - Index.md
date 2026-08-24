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
  - Kurose Chapter 5
---

# Chapter 5: Network Layer — Control Plane

> [!abstract] Executive Summary & Roadmap
> The **Control Plane** coordinates the network-wide logic that determines how datagrams are routed end-to-end among routers from source to destination.
> 
> This vault covers the two architectural paradigms (Per-Router vs SDN), the core routing algorithms (Dijkstra's Link-State and Bellman-Ford Distance-Vector), modern routing protocols (OSPF and BGP-4), and network diagnostics via ICMP and Traceroute.

---

## 🗺️ Master Visual Navigation Map

```mermaid
flowchart TD
    Ch5["Chapter 5: Network Layer - Control Plane"]
    
    Ch5 --> Sec1["[[01 - Control Plane Architecture & SDN vs Traditional]]<br>Per-Router vs Logically Centralized SDN"]
    Ch5 --> Sec2["[[02 - Link-State Routing & Dijkstra's Algorithm]]<br>Global Topology, Dijkstra Math & Oscillations"]
    Ch5 --> Sec3["[[03 - Distance-Vector Routing & Bellman-Ford]]<br>Decentralized Updates, Count-to-Infinity & Poisoned Reverse"]
    Ch5 --> Sec4["[[04 - Intra-AS Routing & OSPF]]<br>Link-State Flooding, Hierarchy & Areas"]
    Ch5 --> Sec5["[[05 - Inter-AS Routing & BGP-4]]<br>Path Vector, eBGP/iBGP, Policy & Tie-Breaking"]
    Ch5 --> Sec6["[[06 - ICMP & Traceroute Mechanics]]<br>Error Reporting, Ping & Traceroute TTL Expiry"]
    Ch5 --> Sec7["[[07 - Book Extras & Professor Traps]]<br>Valley-Free Routing, Hot Potato vs Cold Potato & Edge Cases"]
    Ch5 --> Sec8["[[08 - Comprehensive Worked Numericals & Exam Problems]]<br>Dijkstra & Distance-Vector Step-by-Step Problem Set"]
```

---

## 📑 Detailed Note Registry

| # | Note Document | Core Question Answered | High-Yield Topics |
| :---: | :--- | :--- | :--- |
| **01** | [[01 - Control Plane Architecture & SDN vs Traditional]] | *How do traditional per-router protocols differ from SDN?* | Per-Router vs SDN Control Plane, Southbound APIs, Control Agents |
| **02** | [[02 - Link-State Routing & Dijkstra's Algorithm]] | *How does Dijkstra's algorithm find least-cost paths globally?* | Dijkstra Steps ($N', D(v), p(v)$), Complexity ($O(\|V\|^2)$ vs $O(\|E\| + \|V\|\log\|V\|)$), Traffic Oscillations |
| **03** | [[03 - Distance-Vector Routing & Bellman-Ford]] | *How do decentralized routers converge without global topology?* | Bellman-Ford Eq ($d_x(y) = \min_v \{ c(x,v) + d_v(y) \}$), Count-to-Infinity, Poisoned Reverse (2-node vs 3-node) |
| **04** | [[04 - Intra-AS Routing & OSPF]] | *How does an Autonomous System route internal traffic securely?* | OSPF Features (MD5, ECMP), Hierarchical OSPF (Area 0 Backbone, ABRs, ASBRs) |
| **05** | [[05 - Inter-AS Routing & BGP-4]] | *How do competing global ISPs negotiate traffic routing?* | eBGP vs iBGP, Path Attributes (`AS-PATH`, `NEXT-HOP`), Policy Tie-Breakers, Hot Potato Routing |
| **06** | [[06 - ICMP & Traceroute Mechanics]] | *How does the Internet diagnose faults and map router hops?* | ICMP Type/Code Matrix (Type 0/8 Echo, Type 3 Unreachable, Type 11 TTL Exceeded), Traceroute |
| **07** | [[07 - Book Extras & Professor Traps]] | *What subtle edge cases appear on high-difficulty exams?* | Valley-Free Routing (Gao-Rexford), Poisoned Reverse Loophole, Hot-Potato vs BGP Policy |
| **08** | [[08 - Comprehensive Worked Numericals & Exam Problems]] | *How do you solve routing algorithm tables with zero errors?* | Full Dijkstra execution matrices, Distance-Vector routing tables with link cost changes |

---
#### Navigation
Next → [[01 - Control Plane Architecture & SDN vs Traditional]]
