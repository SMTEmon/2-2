---
title: "01 - Control Plane Architecture & SDN vs Traditional"
course: "CSE 4411"
chapter: 5
section: 5.1
tags:
  - cse4411
  - networking
  - control-plane
  - sdn
  - per-router-control
aliases:
  - Control Plane Architecture
  - SDN vs Per-Router Control
---

# 01 - Control Plane Architecture: SDN vs Traditional

> [!abstract] Key Takeaway
> The Control Plane determines the end-to-end network paths taken by packets.
> - **Traditional (Per-Router) Approach:** Monolithic routing algorithms (OSPF, BGP) execute independently on every router, interacting via peer message exchanges.
> - **SDN (Logically Centralized) Approach:** A distinct remote software controller computes forwarding tables centrally and installs them on commodity switches via standard Southbound APIs (e.g., OpenFlow).

---

## 1. Architectural Comparison

```mermaid
flowchart TD
    subgraph Traditional ["1. Traditional Per-Router Control"]
        R1_CP["Routing Algo"] <-->|Peer Messages| R2_CP["Routing Algo"]
        R1_CP --> R1_DP["Forwarding Table (FIB)"]
        R2_CP --> R2_DP["Forwarding Table (FIB)"]
    end

    subgraph SDN ["2. Software-Defined Networking (SDN)"]
        Apps["Network Control Apps<br>(Routing, Access Control, Load Balancing)"]
        Apps <-->|Northbound API (REST/gRPC)| Controller["SDN Controller<br>(Logically Centralized Network OS)"]
        Controller -->|Southbound API (OpenFlow)| CA1["Control Agent (Switch 1)"]
        Controller -->|Southbound API (OpenFlow)| CA2["Control Agent (Switch 2)"]
        CA1 --> FT1["Flow Table 1"]
        CA2 --> FT2["Flow Table 2"]
    end
```

---

## 2. Technical Comparison Matrix

| Architectural Feature | Traditional Per-Router Control | Software-Defined Networking (SDN) |
| :--- | :--- | :--- |
| **Control Logic Execution** | Distributed inside every individual router's CPU | Centralized software controller running on commodity servers |
| **Data Plane / Control Plane Coupling** | **Tightly coupled** in proprietary hardware/OS (e.g., Cisco IOS) | **Decoupled**; open, standard programmable data plane (OpenFlow, P4) |
| **Network State Abstraction** | Implicit; pieced together by neighbor protocol exchanges | Explicit; controller maintains a global Network Graph database |
| **Management & Policy Changes** | Manual per-box CLI configuration (error-prone) | Automated programmatic configuration via Northbound APIs |
| **Innovation Velocity** | Slow (requires vendor support and IETF standardization) | Rapid (software deployment on software controllers) |

---

## 3. "Why" Questions & Exam Traps

> [!warning] Exam Trap: "Logically Centralized" vs "Physically Centralized"
> - The SDN controller is **logically centralized** because it presents a single, unified global view and single point of policy control to network applications.
> - In reality, it is **physically distributed** across a redundant cluster of servers (e.g., Raft/Paxos consensus clusters) to prevent a single point of failure (SPOF) and scale across massive datacenters.

---
#### Navigation
← Previous: [[00 - Index]] | Next: [[02 - Link-State Routing & Dijkstra's Algorithm]] →
