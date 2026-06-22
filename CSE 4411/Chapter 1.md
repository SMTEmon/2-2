***

# Chapter 1: Introduction to Computer Networking

> [!abstract] Overview
> This chapter provides a "feel" and "big picture" of computer networking and the Internet. It covers the terminology, the edge and core of the network, performance metrics (delay, loss, throughput), layered architecture (OSI and TCP/IP), network security, and a brief history of the Internet.

---

## 1. Core Concepts: The Internet & Protocols

### 1.1 What is the Internet?
The Internet can be defined from two distinct perspectives:
*   **The "Nuts and Bolts" View (Hardware & Software):**
    *   **Hosts / End Systems:** Computing devices (PCs, servers, smartphones, IoT) running network applications at the Internet's "edge."
    *   **Communication Links:** Physical media connecting devices (fiber, copper, radio, satellite). Their transmission rate is called **bandwidth**.
    *   **Packet Switches:** Devices (Routers in core, Switches in access networks) that forward data chunks (packets).
    *   **Network of Networks:** Interconnected **ISPs** (Internet Service Providers).
    *   **Internet Standards:** Maintained by the **IETF** (Internet Engineering Task Force) in **RFCs** (Request for Comments).
*   **The "Services" View (Application Perspective):**
    *   **Infrastructure:** Provides services to distributed applications (Web, streaming, email).
    *   **Programming Interface (API):** Provides "hooks" for applications to connect to the Internet and send/receive data.

### 1.2 What is a Protocol?
> [!info] Protocol Definition
> A protocol defines the **format** and the **order** of messages exchanged between two or more communicating entities, as well as the **actions taken** on the transmission and/or receipt of a message or other event.
*   **Human Analogy:** Saying "Hello" and expecting a response.
*   **Computer Networks:** All communication is governed by protocols (e.g., TCP, IP, HTTP).

---

## 2. Network Structure: Edge, Core, and Interconnections

### 2.1 The Network Edge
The edge consists of hosts (clients and servers).

#### Access Networks
How end systems connect to edge routers:
*   **Residential Access:**
    *   *Cable (HFC - Hybrid Fiber Coax):* Uses TV infrastructure. Asymmetric (faster downstream). Uses **FDM**. Shared medium.
    *   *DSL (Digital Subscriber Line):* Uses telephone lines connected to a **DSLAM**. Dedicated line to central office.
*   **Enterprise Networks:** Institutional routers connecting wired (Ethernet) and wireless devices.
*   **Data Center Networks:** High-bandwidth links connecting massive server clusters.
*   **Wireless Access Networks:**
    *   *WLAN (Wi-Fi):* Local area, ~100 ft.
    *   *Wide-Area Cellular:* 4G/5G, tens of kilometers.

#### Physical Media
*   **Guided Media:** Signals propagate in solid media (Twisted Pair, Coaxial Cable, Fiber Optic Cable).
*   **Unguided Media:** Signals propagate freely (Wi-Fi, Cellular, Bluetooth, Satellite).

### 2.2 The Network Core
A mesh of interconnected routers. Moves data via **Packet Switching**.

#### Packet Switching vs. Circuit Switching
*   **Packet Switching:** Messages broken into **packets**.
    *   *Store-and-Forward:* Router must receive the *entire* packet before transmitting to the next link.
    *   *Queueing & Loss:* If arrival rate exceeds transmission rate, packets queue. If buffer is full, packets drop (loss).
    *   *Routing vs. Forwarding:* Routing determines the path (global); Forwarding moves packets to the output link (local).
    *   *Pros/Cons:* Resource sharing, handles bursty data well, but can have congestion and packet loss.
*   **Circuit Switching:** Dedicated end-to-end resources (e.g., traditional phone networks).
    *   *No sharing:* Guaranteed performance, but resources can sit idle.
    *   *Multiplexing:* **FDM** (Frequency bands) and **TDM** (Time slots).

### 2.3 Internet Structure: "Network of Networks"
Connecting millions of ISPs requires a stepwise architecture:
1.  **Tier-1 Commercial ISPs:** National/international coverage (e.g., AT&T).
2.  **Regional ISPs:** Connect access ISPs.
3.  **Access ISPs:** Connect end-users.
4.  **IXPs (Internet Exchange Points):** Where ISPs peer directly to save costs.
5.  **Content Provider Networks:** (e.g., Google, Netflix) Bypass Tier-1 to bring content closer to users.

---

## 3. Performance & Architecture

### 3.1 Performance Metrics
*   **Packet Delay ($d_{nodal}$):** $d_{nodal} = d_{proc} + d_{queue} + d_{trans} + d_{prop}$
    *   *$d_{proc}$ (Processing):* Checking errors, determining output link.
    *   *$d_{queue}$ (Queueing):* Waiting for transmission. Depends on congestion.
    *   *$d_{trans}$ (Transmission):* Time to push packet onto link. $L/R$ (Length / Bandwidth).
    *   *$d_{prop}$ (Propagation):* Time for signal to travel. $d/s$ (Distance / Speed).
*   **Packet Loss:** Occurs when packets arrive at a full router buffer.
*   **Throughput:** Rate of successful data transfer.
    *   Constrained by the **Bottleneck Link** on the end-to-end path.

### 3.2 Protocol Layers and Reference Models
Layering provides structure and modularization.

**TCP/IP Protocol Stack (5 Layers) vs. OSI Model (7 Layers):**
*   **Application:** Network applications (Messages) - *HTTP, DNS*
*   *(OSI Only) Presentation:* Data interpretation (encryption, compression).
*   *(OSI Only) Session:* Synchronization, recovery.
*   **Transport:** Process-to-process transfer (Segments) - *TCP, UDP*
*   **Network:** Source to destination routing (Datagrams) - *IP*
*   **Link:** Neighboring network elements transfer (Frames) - *Ethernet, Wi-Fi*
*   **Physical:** Bits "on the wire" (Bits)

**Encapsulation:** As data moves down, each layer adds a header (e.g., App Message $\rightarrow$ Transport Segment $\rightarrow$ Network Datagram $\rightarrow$ Link Frame). Decapsulation happens on the receiving end.

---

## 4. Security & History

### 4.1 Network Security
*   **Threats:** Packet Sniffing (intercepting), IP Spoofing (fake identity), Denial of Service / DoS (overwhelming resources).
*   **Defenses:** Authentication, Confidentiality (Encryption), Integrity Checks, Firewalls/VPNs.

### 4.2 Brief History
*   **1960s:** Packet switching theory, ARPAnet.
*   **1970s:** TCP/IP concept (minimalism, best-effort), Ethernet.
*   **1980s:** TCP/IP deployment, DNS, TCP congestion control.
*   **1990s:** Commercialization, World Wide Web (HTML/HTTP).
*   **2000s+:** Broadband, smartphones, Cloud, SDN (Software-Defined Networking).

---
<br>

# Exam Quick Reference

## Definitions Table
| Term | Definition |
| :--- | :--- |
| **Bandwidth** | The transmission rate of a communication link (bits per second). |
| **Bottleneck Link** | The link on a path that constrains the end-to-end throughput. |
| **Circuit Switching** | Dedicated end-to-end network resources reserved for the duration of a connection. |
| **Encapsulation** | The process of adding layer-specific headers to a payload as it moves down the protocol stack. |
| **FDM** | Frequency Division Multiplexing; dividing the spectrum of a link into different frequency bands. |
| **Forwarding** | A local router action moving a packet from an input link to the appropriate output link. |
| **Guided Media** | Physical media where signals propagate in solid materials (e.g., twisted pair, fiber optics). |
| **Hosts / End Systems** | Devices sitting at the "edge" of the Internet running network applications. |
| **ISP** | Internet Service Provider; provides network access to end systems and other networks. |
| **IXP** | Internet Exchange Point; physical infrastructure where ISPs peer to exchange traffic. |
| **Packet Switching** | Breaking data into chunks (packets) and forwarding them based on destination addresses; shares network resources. |
| **Protocol** | Defines the format, order, and actions of messages exchanged between network entities. |
| **Routing** | A global network action determining the path a packet takes from source to destination. |
| **TDM** | Time Division Multiplexing; dividing link time into slots and assigning slots to connections. |
| **Throughput** | The rate at which bits are successfully transferred from sender to receiver. |
| **Unguided Media** | Media where signals propagate freely (e.g., radio waves, Wi-Fi). |

<br>

## Formulas & Things to Remember
| Category | Formula / Concept | Details |
| :--- | :--- | :--- |
| **Nodal Delay** | $d_{nodal} = d_{proc} + d_{queue} + d_{trans} + d_{prop}$ | Total time a packet spends at a single router node. |
| **Transmission Delay** | $d_{trans} = \frac{L}{R}$ | $L$ = Packet length (bits), $R$ = Transmission rate/Bandwidth (bps). Time to push the packet onto the wire. |
| **Propagation Delay** | $d_{prop} = \frac{d}{s}$ | $d$ = physical distance, $s$ = propagation speed ($\sim 2 \times 10^8$ m/s). Time for a bit to travel through the medium. |
| **Traffic Intensity** | $I = \frac{L \cdot a}{R}$ | $a$ = average arrival rate. If $I \to 1$, $d_{queue}$ gets huge. If $I > 1$, packets drop. |
| **TCP/IP Layers** | **A**pplication, **T**ransport, **N**etwork, **L**ink, **P**hysical | Mnemonic: **A**ll **T**hose **N**etwork **L**inks **P**hysically connect. |
| **OSI Layers** | Application, Presentation, Session, Transport, Network, Link, Physical | Mnemonic: **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing (Data Link = Link). |
| **Data Units by Layer** | Application $\rightarrow$ **Message**<br>Transport $\rightarrow$ **Segment**<br>Network $\rightarrow$ **Datagram**<br>Link $\rightarrow$ **Frame**<br>Physical $\rightarrow$ **Bits** | Crucial terminology for what data is called at each protocol layer. |