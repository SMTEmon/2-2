***

# Chapter 1: Introduction to Computer Networking

> [!abstract] Overview
> This chapter provides a "feel" and "big picture" of computer networking and the Internet. It covers the terminology, the edge and core of the network, performance metrics (delay, loss, throughput), layered architecture (OSI and TCP/IP), network security, and a brief history of the Internet.

---

## 1. What is the Internet?
The Internet can be defined from two distinct perspectives: the **"Nuts and Bolts"** view and the **"Services"** view.

### The "Nuts and Bolts" View (Hardware & Software)
*   **Hosts / End Systems:** Billions of connected computing devices (PCs, servers, smartphones, IoT devices like smart fridges, web-enabled toasters, and cars). They run network applications at the Internet's "edge."
*   **Communication Links:** The physical media connecting devices. Includes fiber, copper, radio, and satellite. The transmission rate of these links is known as **bandwidth**.
*   **Packet Switches:** Devices that forward packets (chunks of data) through the network. The two primary types are **Routers** (typically used in the network core) and **Switches** (typically used in access networks).
*   **Network of Networks:** The Internet is fundamentally interconnected **ISPs** (Internet Service Providers).
*   **Protocols:** Control the sending and receiving of messages (e.g., TCP, IP, HTTP, Wi-Fi).
*   **Internet Standards:** Maintained by the **IETF** (Internet Engineering Task Force) in documents called **RFCs** (Request for Comments).

### The "Services" View (Application Provider Perspective)
*   **Infrastructure:** Provides services to distributed applications (e.g., Web, streaming, email, social media, games).
*   **Programming Interface (API):** Provides "hooks" that allow sending and receiving apps to connect to the Internet, analogous to the postal service offering options (express, registered, standard) to senders.

---

## 2. What is a Protocol?
> [!info] Definition
> A **protocol** defines the **format** and the **order** of messages exchanged between two or more communicating entities, as well as the **actions taken** on the transmission and/or receipt of a message or other event.

*   **Human Analogy:** "What's the time?", "Hello." There are expected rules and responses.
*   **Computer Networks:** All communication activity on the Internet is governed by protocols (e.g., TCP connection request $\rightarrow$ TCP connection response $\rightarrow$ HTTP GET request $\rightarrow$ File sent).

---

## 3. The Network Edge
The network edge consists of the hosts (clients and servers). Servers are often grouped in massive data centers.

### Access Networks and Physical Media
How do end systems connect to edge routers?

#### 1. Residential Access
*   **Cable-based Access (HFC - Hybrid Fiber Coax):** Uses existing cable television infrastructure. It uses **FDM (Frequency Division Multiplexing)** to transmit data and TV signals at different frequencies over a shared cable.
    *   *Asymmetric:* Downstream (to home) is faster (up to 1.2 Gbps) than upstream (30-100 Mbps).
    *   Homes *share* the access network to the cable headend.
*   **DSL (Digital Subscriber Line):** Uses existing telephone lines connected to a central office **DSLAM** (DSL Access Multiplexer).
    *   Data and voice are transmitted at different frequencies over a *dedicated* line to the central office.
*   **Home Networks:** Typically consist of a combined modem/router/firewall/Wi-Fi access point box connecting wired (Ethernet) and wireless devices to the ISP.

#### 2. Enterprise Networks
*   Used by companies and universities.
*   Mix of wired and wireless technologies (Ethernet switches and Wi-Fi access points) connecting to an institutional router, which connects to the ISP.
*   Speeds range from 100 Mbps to 10 Gbps.

#### 3. Data Center Networks
*   High-bandwidth links (10s to 100s of Gbps) connecting hundreds of thousands of servers together and to the Internet.

#### 4. Wireless Access Networks
*   **WLAN (Wireless Local Area Network):** Wi-Fi (802.11 b/g/n/ac/ax). Operates within a building (~100 ft). Speeds up to 1000s of Mbps (Wi-Fi 6/7).
*   **Wide-Area Cellular:** 4G/5G cellular networks. Provided by mobile operators over tens of kilometers.

### Physical Media
*   **Bit:** Propagates between transmitter/receiver pairs.
*   **Guided Media:** Signals propagate in solid media.
    *   *Twisted Pair (TP):* Two insulated copper wires (e.g., Cat 5, Cat 6 Ethernet).
    *   *Coaxial Cable:* Two concentric copper conductors, bidirectional, broadband.
    *   *Fiber Optic Cable:* Glass fiber carrying light pulses. Extremely high-speed, low error rate, immune to electromagnetic noise.
*   **Unguided Media:** Signals propagate freely (e.g., wireless radio).
    *   Affected by reflection, obstruction, and interference.
    *   Includes Wi-Fi, Cellular, Bluetooth, and Satellite (e.g., Starlink, which offers <100 Mbps downlink and ~270 ms delay for geostationary, though LEO satellites like Starlink have much lower latency).

---

## 4. The Network Core
The core is a mesh of interconnected routers. The fundamental mechanism for moving data is **Packet Switching**.

### Packet Switching vs. Circuit Switching

#### Packet Switching
Hosts break application-layer messages into smaller chunks called **packets** (length $L$ bits). These are transmitted into the access network at transmission rate $R$.
*   **Store-and-Forward:** The router must receive the *entire* packet before it can begin transmitting it to the next link.
*   **Queueing and Loss:** If packets arrive at a router faster than the router can forward them, they wait in a buffer (queue). If the buffer fills up, incoming packets are dropped (packet loss).
*   **Forwarding vs. Routing:**
    *   *Routing (Global Action):* Algorithms determine the end-to-end path through the network.
    *   *Forwarding (Local Action):* Also known as "switching." Moving an arriving packet from a router's input link to the correct output link based on a local forwarding table.

#### Circuit Switching
Dedicated end-to-end resources (link capacity, buffers) are allocated and reserved for a "call" between source and destination (e.g., traditional telephone networks).
*   **No sharing:** Guaranteed performance, but resources sit idle if not used by the call.
*   **Multiplexing Techniques:**
    1.  **FDM (Frequency Division Multiplexing):** The frequency spectrum of a link is divided into narrow frequency bands. Each call gets its own band.
    2.  **TDM (Time Division Multiplexing):** Time is divided into frames of fixed duration, and each frame is divided into a fixed number of time slots. Each call is assigned one time slot per frame.

> [!question] Is Packet Switching a "Slam Dunk Winner"?
> **Pros:** Excellent for "bursty" data because of resource sharing. Simpler (no call setup). Allows vastly more users on the network simultaneously compared to circuit switching.
> **Cons:** Excessive congestion is possible, leading to packet delay and loss. Requires complex protocols for reliable data transfer and congestion control.

---

## 5. Internet Structure: "Network of Networks"
Given millions of access ISPs, how are they connected? Directly connecting them all requires $O(N^2)$ connections (impossible to scale).

**Current Stepwise Architecture:**
1.  **Access ISPs:** Connect end-users.
2.  **Regional ISPs:** Connect access ISPs within a region.
3.  **Tier-1 Commercial ISPs:** (e.g., Level 3, AT&T, Sprint) National and international coverage. They interconnect with each other and connect regional ISPs to the global network.
4.  **IXPs (Internet Exchange Points):** Physical infrastructure where different ISPs peer (connect directly) with one another to keep local traffic local and save transit costs.
5.  **Content Provider Networks:** (e.g., Google, Meta, Netflix). Private networks that bypass Tier-1 and Regional ISPs to bring content closer to the end-user (edge caching).

---

## 6. Performance: Delay, Loss, and Throughput

### Four Sources of Packet Delay ($d_{nodal}$)
$$d_{nodal} = d_{proc} + d_{queue} + d_{trans} + d_{prop}$$

1.  **Nodal Processing Delay ($d_{proc}$):** Time taken to check bit errors and determine the output link. Usually `< microseconds`.
2.  **Queueing Delay ($d_{queue}$):** Time waiting at the output link for transmission. Depends on router congestion.
    *   **Traffic Intensity** $= \frac{L \cdot a}{R}$ *(where a = average packet arrival rate, L = packet length, R = bandwidth)*.
    *   If intensity $\sim 0$: delay is small.
    *   If intensity $\rightarrow 1$: delay becomes very large.
    *   If intensity $> 1$: more work arriving than can be serviced; average delay approaches infinity (packet loss occurs).
3.  **Transmission Delay ($d_{trans}$):** Time needed to "push" the packet onto the link.
    *   $d_{trans} = \frac{L}{R}$ *(Packet length / Link transmission rate)*.
4.  **Propagation Delay ($d_{prop}$):** Time for the signal to travel across the physical medium.
    *   $d_{prop} = \frac{d}{s}$ *(Distance / Propagation speed)*. Speed is usually $\sim 2 \times 10^8$ m/sec.

> [!warning] Transmission vs. Propagation Delay (The Caravan Analogy)
> *   **Transmission delay** is like the time it takes a toll booth to process a 10-car caravan (push them onto the highway).
> *   **Propagation delay** is the time it takes the cars to physically drive from one toll booth to the next.

### Packet Loss
Queue (buffer) preceding a link has finite capacity. If a packet arrives to a full queue, it is dropped (lost). It may be retransmitted by previous nodes, the source, or not at all.

### Throughput
The rate (bits/time unit) at which bits are being successfully sent from sender to receiver.
*   **Instantaneous:** Rate at a given point in time.
*   **Average:** Rate over a longer period.
*   **Bottleneck Link:** The link on the end-to-end path that constrains the end-to-end throughput. In a path with links $R_s$ and $R_c$, the throughput is $min(R_s, R_c)$.

---

## 7. Protocol Layers and Reference Models

Networks are highly complex. **Layering** provides an explicit structure that allows identification and relationship of the system's pieces. Modularization eases maintenance (changing the implementation of one layer does not affect the rest of the system).

### The TCP/IP Protocol Stack (5 Layers)

| Layer | Name | Function | Data Unit Name | Examples |
| :--- | :--- | :--- | :--- | :--- |
| **5** | **Application** | Network applications | Message | HTTP, SMTP, DNS, FTP |
| **4** | **Transport** | Process-to-process data transfer | Segment | TCP, UDP |
| **3** | **Network** | Routing of data from source to destination | Datagram | IP, Routing Protocols |
| **2** | **Link** | Data transfer between neighboring network elements | Frame | Ethernet, Wi-Fi (802.11) |
| **1** | **Physical** | Bits "on the wire" | Bits | Cables, Radio waves |

### The OSI Reference Model (7 Layers)
Developed by ISO, it includes two additional layers between Application and Transport:
*   **Presentation (Layer 6):** Allows applications to interpret meaning of data (encryption, compression, formatting).
*   **Session (Layer 5):** Synchronization, checkpointing, recovery of data exchange.
*   *Note:* In the TCP/IP stack, if these functions are needed, the application developer must implement them inside the Application layer.

### Encapsulation
As data moves down the protocol stack at the sending host, each layer adds its own header information (like Matryoshka nesting dolls).
1.  App layer creates a **Message** ($M$).
2.  Transport layer adds header $H_t$ $\rightarrow$ **Segment** $[H_t | M]$.
3.  Network layer adds header $H_n$ $\rightarrow$ **Datagram** $[H_n | H_t | M]$.
4.  Link layer adds header $H_l$ $\rightarrow$ **Frame** $[H_l | H_n | H_t | M]$.
*At the receiving end, the process is reversed (Decapsulation).*

---

## 8. Network Security
The Internet was originally designed with the vision of "a group of mutually trusting users attached to a transparent network." Today, designers play catch-up.

### Common Threats:
*   **Packet Interception ("Sniffing"):** On broadcast media (like open Wi-Fi), a promiscuous network interface can read/record all passing packets (e.g., using Wireshark).
*   **Fake Identity (IP Spoofing):** Injecting packets into the network with a false source IP address to masquerade as another user.
*   **Denial of Service (DoS):** Attackers make resources (server, bandwidth) unavailable to legitimate traffic by overwhelming the resource with bogus traffic (often utilizing compromised botnets in DDoS attacks).

### Lines of Defense:
*   **Authentication:** Proving you are who you say you are (SIM cards do this in cellular; it's harder on the raw Internet).
*   **Confidentiality:** Encryption of data.
*   **Integrity Checks:** Digital signatures to prevent/detect tampering.
*   **Access Restrictions:** VPNs and Firewalls (middleboxes that filter incoming packets to restrict senders/receivers).

---

## 9. History of the Internet (Brief Timeline)

*   **1961 - 1972: Early Packet Switching**
    *   1961: Kleinrock's queueing theory proves packet switching is effective.
    *   1969: First **ARPAnet** node operational.
    *   1972: ARPAnet public demo; first e-mail program.
*   **1972 - 1980: Internetworking & Proprietary Networks**
    *   1974: Vint Cerf and Bob Kahn publish architecture for interconnecting networks (TCP/IP concept).
        *   *Principles:* Minimalism, autonomy, best-effort service, stateless routing, decentralized control.
    *   1976: Ethernet invented at Xerox PARC.
*   **1980 - 1990: Proliferation of Networks**
    *   1983: Official deployment of TCP/IP; DNS defined.
    *   1988: TCP congestion control implemented to fix network collapse.
*   **1990 - 2000s: Commercialization and the Web**
    *   1991: NSF lifts restrictions on commercial use. ARPAnet decommissioned.
    *   Early 90s: Tim Berners-Lee creates HTML/HTTP (The World Wide Web).
    *   Late 90s: Killer apps emerge (Instant messaging, P2P file sharing).
*   **2005 - Present: Scale, SDN, Mobility, Cloud**
    *   Aggressive broadband deployment, smartphones, IoT (~15B devices).
    *   **SDN** (Software-Defined Networking) separates control plane from data plane.
    *   Rise of Cloud Computing (AWS, Azure) and massive private content networks (Google, Meta).