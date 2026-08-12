---
title: "Chapter 5: Network Layer - Control Plane (Comprehensive Notes)"
course: "CSE 4411"
chapter: 5
tags:
  - cse4411
  - networking
  - control-plane
  - final
---

# Chapter 5: Network Layer - Control Plane

> [!info] Chapter Overview
> The Network Layer is responsible for moving datagrams from a sending host to a receiving host. While Chapter 4 covered the **Data Plane** (the local, per-router forwarding function), this chapter focuses on the **Control Plane** (the network-wide logic that determines the end-to-end path packets take).

---

## 1. Introduction: Data Plane vs. Control Plane

To understand the network layer, we must clearly separate its two main functions:

1. **Forwarding (Data Plane):** The process of moving packets from a router's input link to the appropriate output link. This is a local, per-router function implemented in hardware at nanosecond speeds.
2. **Routing (Control Plane):** The process of determining the route or path taken by packets as they flow from sender to receiver. This is a network-wide logic implemented in software at millisecond or second speeds.

### Two Approaches to Structuring the Control Plane

There are two primary architectures for the network control plane:

#### A. Per-Router Control (Traditional)
Individual routing algorithm components in **each and every router** interact in the control plane to compute forwarding tables.
- **Mechanism:** Routers talk to their neighbors using routing protocols (like OSPF or BGP).
- **Structure:** Monolithic. The routing algorithm, control plane, and data plane are tightly integrated within the router hardware (e.g., proprietary Cisco IOS).

#### B. Logically Centralized Control (Software-Defined Networking - SDN)
A distinct, **remote controller** computes and installs forwarding tables into the routers.
- **Mechanism:** The remote controller uses a protocol (like OpenFlow) to communicate with local Control Agents (CAs) situated on each router. The CAs have minimal intelligence and simply execute the controller's commands.
- **Structure:** Separated. The control plane is unbundled from the data plane, allowing for easier network management, greater flexibility in traffic flows, and open (non-proprietary) implementations.

---

## 2. Routing Algorithms

The goal of a routing protocol is to determine "good" paths (equivalently, routes) from sending hosts to receiving hosts through the network of routers. A "good" path typically means the least "cost", "fastest", or "least congested" path.

### Graph Abstraction and Link Costs
Networks are modeled as a graph $G = (N,E)$:
- **Nodes ($N$):** Represent the routers.
- **Edges ($E$):** Represent the physical links between routers.
- **Link Cost ($c_{a,b}$):** The cost of the direct link connecting node $a$ and node $b$. If there is no direct link, $c_{a,b} = \infty$.
  - Costs are defined by the network operator. They could be $1$ for all links (resulting in shortest-path/minimum-hop routing), or inversely related to bandwidth/congestion.

### Classification of Routing Algorithms

Routing algorithms can be classified in two main ways:

#### 1. By Information Availability: Global vs. Decentralized
*   **Global Information:** All routers have complete knowledge of the network topology and all link costs. 
    *   *Algorithm type:* **Link-State (LS)** algorithms.
*   **Decentralized Information:** Routers only know the costs of their directly attached links initially. Through an iterative process of computation and information exchange with immediate neighbors, they calculate the least-cost path.
    *   *Algorithm type:* **Distance-Vector (DV)** algorithms.

#### 2. By Adaptability: Static vs. Dynamic
*   **Static:** Routes change very slowly over time, often requiring manual human intervention to update link costs or paths.
*   **Dynamic:** Routes change automatically in response to changes in topology or traffic loads. They can be *load-sensitive* (costs reflect current congestion) or *load-insensitive* (modern protocols like OSPF/BGP are load-insensitive to prevent severe oscillations).

---

### 2.1 Link-State (LS) Routing Algorithm

Link-State algorithms, such as **Dijkstra’s Algorithm**, require global network information. 
- **Mechanism:** This is accomplished via a "link-state broadcast", where every node broadcasts its link-state information to *all* other nodes in the network.
- **Process:** Each node independently runs Dijkstra's algorithm to compute the least-cost paths from itself to all other nodes, resulting in its own forwarding table.
- **Characteristics:** It is a centralized algorithm (in terms of information requirement) but runs independently on each node.

> [!tip] Complexity and Issues
> - **Algorithm Complexity:** For $n$ nodes, the algorithm requires $O(n^2)$ comparisons (or $O(n \log n)$ with efficient heap implementations).
> - **Message Complexity:** Each router must broadcast to $n$ other routers, resulting in $O(n^2)$ message complexity.
> - **Oscillations:** If link costs are load-sensitive (depend on traffic volume), LS algorithms can suffer from severe route oscillations as traffic constantly shifts back and forth between paths trying to find the least congested route.

### 2.2 Distance-Vector (DV) Routing Algorithm

Distance-Vector algorithms are decentralized, iterative, asynchronous, and self-stopping. They are based on dynamic programming and the **Bellman-Ford equation**.

- **The Bellman-Ford Equation:** Let $d_x(y)$ be the cost of the least-cost path from $x$ to $y$. Then:
  $$d_x(y) = \min_v \{ c_{x,v} + d_v(y) \}$$
  *(The min is taken over all immediate neighbors $v$ of $x$)*

- **Mechanism:**
  1. Each node maintains its own **Distance Vector (DV)**, containing its estimated costs to all destinations.
  2. From time to time, each node sends its DV to its immediate neighbors.
  3. When a node receives a new DV from a neighbor, it updates its own DV using the Bellman-Ford equation.
  4. If its own DV changes as a result, it notifies its neighbors.
  5. The process is self-stopping; if no DVs change, no messages are sent.

> [!warning] Link Cost Changes and The Count-to-Infinity Problem
> - **Good news travels fast:** When a link cost decreases, the algorithm quickly converges.
> - **Bad news travels slow:** When a link cost increases significantly (or a link fails), it can lead to routing loops and the **Count-to-Infinity problem**. Nodes repeatedly bounce incorrect routing updates back and forth, slowly incrementing the cost until it reaches infinity.
> - **Poisoned Reverse:** A technique to solve specific 2-node loops. If $z$ routes through $y$ to get to $x$, $z$ will advertise to $y$ that its distance to $x$ is infinity ($D_z(x) = \infty$). This prevents $y$ from routing back through $z$. However, it does not solve loops involving 3 or more nodes.

### LS vs. DV: A Brief Comparison

| Feature | Link-State (LS) | Distance-Vector (DV) |
| :--- | :--- | :--- |
| **Message Complexity** | High. $O(n^2)$ messages; requires network-wide flooding. | Low. Exchanges only occur between immediate neighbors. |
| **Speed of Convergence** | Fast. $O(n^2)$ algorithm. May have oscillations. | Varies. Can be slow and suffer from routing loops/count-to-infinity. |
| **Robustness (Failure handling)** | **High.** If a router fails, it only advertises its own incorrect link costs. Each router computes its own table independently. | **Low.** If a router advertises an incorrect *path* cost, this error propagates through the entire network. |

## 3. Intra-ISP Routing: OSPF

To make routing scalable across the massive Internet (with billions of destinations) and to maintain administrative autonomy, routers are aggregated into regions known as **Autonomous Systems (AS)** or domains. 

- **Intra-AS (Intra-domain) Routing:** Routing among routers *within the same* AS. All routers in the AS must run the same intra-domain protocol.
- **Inter-AS (Inter-domain) Routing:** Routing *among* different ASes. Gateways perform inter-domain routing as well as intra-domain routing.

### Open Shortest Path First (OSPF)
OSPF is one of the most common intra-AS routing protocols.

- **Type:** It is a classic **Link-State** protocol.
- **Mechanism:** Each router floods OSPF link-state advertisements directly over IP (rather than using TCP/UDP) to all other routers in the entire AS.
- **Algorithm:** Each router builds a full topological map and uses Dijkstra's algorithm to compute its forwarding table.
- **Metrics:** Multiple link cost metrics are possible (e.g., based on bandwidth or delay).
- **Security:** All OSPF messages are authenticated to prevent malicious intrusion and ensure only trusted routers participate.

#### Hierarchical OSPF
For very large ASes, OSPF can be structured into a **two-level hierarchy**: local areas and a backbone area.
- **Local Area:** Link-state advertisements are flooded only within the area. Routers have detailed topology of their own area but only know the direction to reach other destinations.
- **Area Border Routers:** These sit at the edge of an area, "summarize" the distances to destinations within their own area, and advertise these summaries into the backbone.
- **Backbone Area:** The primary role of the backbone is to route traffic between the other areas in the AS. It contains all the area border routers and backbone routers.

---

## 4. Routing Among ISPs: BGP

The **Border Gateway Protocol (BGP)** is the *de facto* inter-domain routing protocol—the "glue that holds the Internet together." It allows a subnet to advertise its existence and the destinations it can reach to the rest of the Internet.

### BGP Basics
BGP provides each AS a means to:
1. **eBGP (External BGP):** Obtain destination network reachability info from neighboring ASes.
2. **iBGP (Internal BGP):** Propagate that reachability information to all AS-internal routers.
3. Determine "best" routes to other networks based on reachability information and **policy**.

BGP sessions involve two BGP routers ("peers") exchanging messages over semi-permanent TCP connections (port 179).

#### Path Attributes and BGP Routes
When a BGP router advertises a prefix, it includes several BGP attributes. A prefix + attributes is called a "route".
- **AS-PATH:** A list of the ASes through which the prefix advertisement has passed. This is crucial for loop detection; if a router sees its own AS in the path, it rejects the advertisement.
- **NEXT-HOP:** The specific IP address of the router interface that begins the AS-PATH.

### BGP Route Selection
A router may learn about multiple paths to a destination. It selects the best route based on the following elimination rules:
1. **Local Preference:** Select the route with the highest local preference value (this is a **policy decision** set by the network admin).
2. **Shortest AS-PATH:** Select the route that traverses the fewest number of ASes.
3. **Closest NEXT-HOP Router (Hot Potato Routing):** Select the route with the least *intra-domain* cost to the NEXT-HOP router. The goal is to get the packet out of the local AS as cheaply/quickly as possible, without worrying about the inter-domain cost.
4. **Additional Criteria:** BGP identifiers (tie-breakers).

### BGP Routing Policy
Unlike intra-AS routing, where performance (least cost) is the primary goal, inter-AS routing is dominated by **policy and economics**.
- **Provider Networks:** Want to route traffic only if it generates revenue (i.e., to or from their paying customers). They typically do not want to carry "transit traffic" between two other ISPs for free.
- **Customer Networks (Dual-homed):** If a customer network is attached to two provider networks, it will *not* advertise a route from one provider to the other. This enforces the policy that the customer does not want to act as a transit router for its providers.

| Feature | Intra-AS Routing | Inter-AS Routing |
| :--- | :--- | :--- |
| **Primary Goal** | **Performance** (focus on finding the fastest/least-cost path). | **Policy** (admin wants control over how traffic is routed and who routes through it). |
| **Scale** | Can be hierarchical to save table size (e.g., OSPF areas). | Massive scale is crucial (hierarchical routing saves table size, reduced update traffic). |

## 5. Software-Defined Networking (SDN) Control Plane

Traditional routing uses a distributed, per-router control approach where monolithic routers contain switching hardware and proprietary OS running internet standard protocols. SDN rethinks this by separating the control plane.

### Why Logically Centralized?
- **Easier Network Management:** Avoids router misconfigurations and offers greater flexibility of traffic flows.
- **Table-based Forwarding:** OpenFlow API allows "programming" of routers. Centralized programming is easier than distributed programming.
- **Open Implementation:** Fosters innovation by separating the hardware from the control software.

### Four Key Characteristics of SDN
1. **Flow-based forwarding:** Forwarding can be based on any number of header field values in the transport, network, or link layer headers.
2. **Separation of data plane and control plane:** Fast, simple switches (data plane) execute rules while a remote controller (control plane) computes them.
3. **Control plane functions external to data-plane switches:** The controller maintains network state and interacts with switches.
4. **Programmable control applications:** The "brains" of the network are implemented in control applications that use the API provided by the controller.

### Components of an SDN Controller
An SDN controller acts as a Network Operating System and is divided into three layers:
1. **Interface/Abstractions Layer (Northbound API):** Interacts with network control applications (e.g., routing, load balancing) via RESTful APIs or intent frameworks.
2. **Network-wide State Management Layer:** Maintains distributed, robust state (link-state info, host info, switch flow tables, and statistics).
3. **Communication Layer (Southbound API):** Communicates with the controlled devices (switches) using protocols like OpenFlow or SNMP.

### OpenFlow Protocol
Operates between the controller and the switch over TCP.
- **Controller-to-Switch Messages:** `features` (query switch), `configure` (set parameters), `modify-state` (add/delete flow entries), `packet-out` (send packet from switch port).
- **Switch-to-Controller Messages:** `packet-in` (transfer unmatched packet to controller), `flow-removed` (flow table entry deleted), `port-status` (change on a port).

---

## 6. Internet Control Message Protocol (ICMP)

ICMP is used by hosts and routers to communicate network-level information, primarily for error reporting. 
- It is considered part of the network layer but architecturally sits "above" IP, as ICMP messages are carried *inside* IP datagrams.

### ICMP Message Format
An ICMP message contains a **Type**, a **Code**, and the first 8 bytes of the IP datagram that caused the error (so the sender knows which packet failed).

| Type | Code | Description | Application Use |
| :--- | :--- | :--- | :--- |
| 0 | 0 | Echo reply | `ping` |
| 3 | 1 | Destination host unreachable | Error reporting |
| 3 | 3 | Destination port unreachable | `traceroute` termination |
| 8 | 0 | Echo request | `ping` |
| 11 | 0 | TTL expired | `traceroute` hop discovery |

### Traceroute and ICMP
The `traceroute` program works by sending a series of UDP segments to an unlikely destination port.
- It sets the Time-To-Live (TTL) of the first set to 1, the second to 2, and so on.
- When the $n$-th router receives a datagram with TTL=1, it decrements it to 0, discards it, and sends an ICMP `TTL expired` (Type 11, Code 0) message back to the source.
- When the datagram finally reaches the destination, the host returns an ICMP `port unreachable` (Type 3, Code 3) message, telling the source to stop.

---

## 7. Network Management & Configuration

Network management involves deploying, testing, polling, configuring, analyzing, and controlling network resources.

### Network Management Framework
- **Managing Server:** The application (with human operators) in the Network Operations Center (NOC).
- **Managed Device:** Equipment with manageable components (e.g., a router or switch).
- **Data:** Device "state", including configuration data, operational data, and device statistics.
- **Network Management Agent:** Process running in the managed device that communicates with the managing server.
- **Network Management Protocol:** Used by the managing server to query/configure the device.

### Approaches to Network Management

1. **CLI (Command Line Interface):** Operator issues scripts direct to individual devices. Prone to errors, hard to automate.
2. **SNMP (Simple Network Management Protocol):** 
   - Operates in Request/Response mode (`GetRequest`, `SetRequest`) or Trap mode (`Trap` message for exceptional events).
   - Queries/sets data defined in a **Management Information Base (MIB)**.
   - MIB objects are defined using the Structure of Management Information (SMI).
3. **NETCONF / YANG:**
   - Designed to overcome the limitations of SNMP for configuration management.
   - Provides an abstract, network-wide, holistic approach.
   - **YANG:** A data modeling language used to define structure/syntax of data.
   - **NETCONF:** Protocol using Remote Procedure Calls (RPC) encoded in XML over secure transport (TLS/SSH). Supports atomic-commit actions over multiple devices. Operations include `<get-config>`, `<edit-config>`, and `<lock>`.
