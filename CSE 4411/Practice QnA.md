## Chapter 1: Introduction to Computer Networking

> [!question]- Q: Briefly explain the service model of each layer of the Internet Protocol stack.
> **Answer:**
> The Internet Protocol (TCP/IP) stack consists of 5 layers:
> 1. **Application Layer:** Provides network services to applications (e.g., HTTP, DNS). The service model is to exchange messages between distributed application processes.
> 2. **Transport Layer:** Provides logical communication between processes (e.g., TCP, UDP). Its service model includes connection-oriented reliable delivery (TCP) or connectionless best-effort delivery (UDP).
> 3. **Network Layer:** Provides logical communication between hosts. Its service model is "best-effort" delivery of datagrams from source to destination without guaranteed delivery, timing, or order.
> 4. **Link Layer:** Moves packets from one node to the next over a single link (e.g., Ethernet, Wi-Fi). Its service model is the transfer of frames between neighboring network elements.
> 5. **Physical Layer:** Moves individual bits "on the wire" across the physical medium.

> [!question]- Q: What is the Denial of Service (DoS) attack? Explain the process of different categories of DoS attacks. Is Distributed Denial of Service (DDoS) attack different from a DoS attack?
> **Answer:**
> - **DoS Attack:** A Denial of Service attack is a malicious attempt to overwhelm a target network, server, or service with a flood of Internet traffic, preventing legitimate users from accessing it.
> - **Categories of DoS Attacks:**
>   - *Vulnerability Attack:* Sending a few well-crafted messages to a vulnerable application or OS to crash it.
>   - *Bandwidth Flooding:* Sending a massive amount of packets to the target's access link, exhausting its bandwidth so legitimate packets cannot pass.
>   - *Connection Flooding:* Establishing a huge number of half-open or fully open TCP connections to exhaust the host's resources (like memory).
> - **DDoS vs. DoS:** Yes, they are different. A DoS attack typically originates from a single source. A Distributed Denial of Service (DDoS) attack originates from multiple, distributed, coordinated sources (often a botnet), making it much harder to block and capable of generating significantly more traffic.

> [!question]- Q: Consider sending a packet from a source host to a destination host over a fixed route. List the delay components in the end-to-end delay. Which of these delays are constant and which are variable?
> **Answer:**
> The total nodal delay ($d_{nodal}$) consists of four components:
> 1. **Processing Delay ($d_{proc}$):** Time to check for bit errors and determine the output link. (Largely **Constant**)
> 2. **Queueing Delay ($d_{queue}$):** Time waiting at the output link for transmission. Depends on network congestion. (**Variable**)
> 3. **Transmission Delay ($d_{trans}$):** Time to push the packet onto the link ($L/R$, where $L$ is packet length and $R$ is bandwidth). (Usually **Constant** for a given packet and link)
> 4. **Propagation Delay ($d_{prop}$):** Time for the signal to travel across the physical medium ($d/s$, where $d$ is distance and $s$ is propagation speed). (**Constant**)

> [!question]- Q: Explain how the "man-in-the-middle" attack occurs.
> **Answer:**
> A man-in-the-middle (MitM) attack occurs when an attacker secretly intercepts and possibly alters the communication between two parties who believe they are directly communicating with each other. The attacker inserts themselves into the path, acting as a proxy. For example, they may use packet sniffing to read the traffic or spoofing to masquerade as the legitimate destination, allowing them to steal sensitive information like passwords or inject malicious content.

> [!question]- Q: Show the hierarchical organization of the Internet using a figure with clear notation.
> **Answer:**
> The Internet is a "Network of Networks". Its hierarchical structure is organized to connect millions of Access ISPs together efficiently:
> - **Tier-1 Commercial ISPs:** (e.g., AT&T, Sprint) Global coverage, forming the "backbone" of the Internet. They connect directly to each other without paying peering fees.
> - **Content Provider Networks:** (e.g., Google, Microsoft) Private networks that connect directly to lower-tier ISPs to bypass Tier-1 networks and bring content closer to users.
> - **Regional ISPs:** Connect access ISPs to Tier-1 ISPs.
> - **IXPs (Internet Exchange Points):** Physical infrastructure where multiple ISPs peer (connect) directly to exchange traffic without paying Tier-1 providers.
> - **Access ISPs:** Provide edge connectivity to end-systems (residential, enterprise, cellular networks).

> [!question]- Q: How many bits can fit on a link with a 3-ms delay if the bandwidth is: (i) 2 Mbps, (ii) 10 kBps, and (iii) 150 Mbps?
> **Answer:**
> The number of bits that can fit on a link at any given moment is called the Bandwidth-Delay Product (BDP) = $R \times d_{prop}$
> Given: $d_{prop} = 3 \text{ ms} = 0.003 \text{ seconds}$.
> 
> - **(i) 2 Mbps:**
>   $BDP = 2,000,000 \text{ bits/sec} \times 0.003 \text{ sec} = 6,000 \text{ bits}$.
> - **(ii) 10 kBps:**
>   $BDP = 10 \text{ kBps} = 80 \text{ kbps} = 80,000 \text{ bits/sec}$. *(Assuming B stands for Bytes, $10 \times 8 \times 1000$)*.
>   $BDP = 80,000 \text{ bits/sec} \times 0.003 \text{ sec} = 240 \text{ bits}$.
>   *(Note: if kBps meant kilobits per sec, $10,000 \times 0.003 = 30 \text{ bits}$)*.
> - **(iii) 150 Mbps:**
>   $BDP = 150,000,000 \text{ bits/sec} \times 0.003 \text{ sec} = 450,000 \text{ bits}$.

> [!question]- Q: Draw the TCP/IP protocol suite mentioning protocols in each layer.
> **Answer:**
> 1. **Application Layer:** HTTP, SMTP, DNS, FTP
> 2. **Transport Layer:** TCP, UDP
> 3. **Network Layer:** IP, ICMP, Routing Protocols (OSPF, BGP)
> 4. **Link Layer:** Ethernet, Wi-Fi (802.11), PPP
> 5. **Physical Layer:** Bits over wires/fiber/radio

> [!question]- Q: Write down differences between circuit-switched and packet-switched networks.
> **Answer:**
> - **Circuit Switching:** Dedicated end-to-end resources are reserved for the duration of a connection (e.g., traditional phone networks). It offers guaranteed performance but wastes resources when the connection sits idle.
> - **Packet Switching:** Messages are broken into packets and sent over a shared medium. Routers use a store-and-forward mechanism. It offers better resource sharing and handles bursty data well, but can suffer from queueing delays and packet loss due to congestion.

> [!question]- Q: Explain Bandwidth, Throughput, Delay, and Bandwidth-Delay Product.
> **Answer:**
> - **Bandwidth:** The maximum transmission rate of a physical communication link (bits per second).
> - **Throughput:** The actual rate at which bits are successfully transferred from sender to receiver. It is constrained by the "bottleneck link" on the path.
> - **Delay:** The total time it takes for a packet to travel from source to destination (includes processing, queueing, transmission, and propagation delays).
> - **Bandwidth-Delay Product (BDP):** Calculated as `Bandwidth × Propagation Delay`. It represents the maximum number of bits that can be "in transit" on the link at any given time (filling the pipe).


## Chapter 2: Application Layer

> [!question]- Q: Deduce a quantitative model to show the performance in distribution time of Peer-to-Peer architecture and Server-Client architecture. Which architecture will provide more scalability?
> **Answer:**
> Let $F$ be the file size, $N$ be the number of peers, $u_s$ be the server's upload capacity, $d_{min}$ be the lowest download capacity among peers, and $u_i$ be the upload capacity of peer $i$.
> 
> **Server-Client Architecture:**
> The server must sequentially upload $N$ copies of the file. The client with the slowest download speed limits the minimum possible time.
> Distribution Time ($D_{cs}$):
> $D_{cs} = \max\left\{ \frac{N \cdot F}{u_s}, \frac{F}{d_{min}} \right\}$
> *This time increases linearly with $N$.*
> 
> **Peer-to-Peer (P2P) Architecture:**
> The server only needs to upload one copy of the file initially. The total upload capacity of the system grows as more peers join and share their upload bandwidth.
> Distribution Time ($D_{p2p}$):
> $D_{p2p} = \max\left\{ \frac{F}{u_s}, \frac{F}{d_{min}}, \frac{N \cdot F}{u_s + \sum u_i} \right\}$
> 
> **Scalability:**
> The **Peer-to-Peer architecture** provides vastly superior scalability. In Client-Server, distribution time scales linearly with $N$ (making it slow for many users). In P2P, as $N$ increases, the total upload capacity ($\sum u_i$) also increases, keeping the distribution time relatively flat and scalable.

> [!question]- Q: With suitable labeled diagram(s), explain in brief the interaction among the different types of DNS servers when a host requests a DNS entry.
> **Answer:**
> DNS utilizes an iterated query model across a distributed hierarchy.
> 1. **Host $\rightarrow$ Local DNS:** A host application wants to resolve `www.example.com`. It queries its ISP's Local DNS server.
> 2. **Local DNS $\rightarrow$ Root DNS:** The Local DNS server queries a Root DNS server, which returns the IP address of a TLD Server responsible for `.com`.
> 3. **Local DNS $\rightarrow$ TLD DNS:** The Local DNS queries the `.com` TLD Server, which returns the IP address of the Authoritative DNS server for `example.com`.
> 4. **Local DNS $\rightarrow$ Authoritative DNS:** The Local DNS queries the Authoritative server, which holds the actual Type A record containing the IP address for `www.example.com`.
> 5. **Local DNS $\rightarrow$ Host:** The Local DNS passes the resolved IP back to the host and caches it for future use.

> [!question]- Q: Cookies are used to save the user's current status. Using a timing diagram, demonstrate a user-server interaction that makes use of the user's state via a cookie.
> **Answer:**
> Since HTTP is stateless, cookies add state tracking.
> **Interaction Flow:**
> 1. **Client requests web page for the first time:** `GET / HTTP/1.1`
> 2. **Server processes request:** Server creates a unique ID for the user and stores it in its backend database.
> 3. **Server responds:** `HTTP/1.1 200 OK` ... `Set-cookie: user_id_12345`
> 4. **Client stores cookie:** Browser saves `user_id_12345` locally.
> 5. **Client makes subsequent request:** `GET /cart HTTP/1.1` ... `Cookie: user_id_12345`
> 6. **Server accesses state:** Server reads the cookie, looks up `user_id_12345` in the database, retrieves the user's shopping cart, and returns the personalized response.

> [!question]- Q: Suggest the suitable type of DNS resolution for a person using a lightweight device.
> **Answer:**
> A **recursive** DNS query is most suitable for a lightweight device. In a recursive query, the heavy lifting of resolving the domain name is offloaded to the ISP's Local DNS server. The lightweight device simply asks the Local DNS, and the Local DNS performs the iterative queries across the Root, TLD, and Authoritative servers, eventually returning the final IP address to the device.

> [!question]- Q: Identify and briefly describe the HTTP methods used when a client sends a request for a document and subsequently sends information to the server.
> **Answer:**
> - **GET Method:** Used by the client to request/retrieve a document or resource from the server.
> - **POST Method:** Used by the client to send/submit information (like form data) to the server. The data is included in the body of the HTTP message.

> [!question]- Q: Define and provide an example of a socket address.
> **Answer:**
> A socket address is the combination of an IP address (identifying the host) and a Port Number (identifying the specific application process on that host).
> **Example:** `192.168.1.5:80` (where `192.168.1.5` is the IP address and `80` is the port number for an HTTP web server).


## Chapter 3: Transport Layer

> [!question]- Q: What are the two most essential transport layer services utilized by various applications? What are the application properties to consider before selecting one of these transport layer services?
> **Answer:**
> The two most essential transport layer services are **TCP (Transmission Control Protocol)** and **UDP (User Datagram Protocol)**.
> 
> **TCP Services:**
> - Reliable, in-order byte stream delivery.
> - Connection-oriented (handshake required).
> - Flow control and congestion control.
> 
> **UDP Services:**
> - Connectionless, best-effort (unreliable) datagram delivery.
> - No flow/congestion control (fast and unrestricted).
> 
> **Application Properties to Consider:**
> 1. *Data Reliability:* Does the application need a 100% guarantee of delivery without errors? (File transfer, web browsing -> TCP).
> 2. *Timing/Latency:* Can the application tolerate delays caused by handshakes and retransmissions? (Real-time gaming, VoIP -> UDP).
> 3. *Throughput:* Does the application require a minimum sustained bandwidth, or is it elastic?
> 4. *Security:* Neither provides native encryption (requires TLS over TCP).

> [!question]- Q: Identify and explain the differences between TCP and UDP headers.
> **Answer:**
> - **UDP Header:** Very small (8 bytes minimum overhead). Contains only 4 fields: Source Port, Destination Port, Length, and Checksum. It prioritizes speed and low overhead.
> - **TCP Header:** Much larger (20 bytes minimum overhead). Contains Source/Dest Ports, Sequence Number (for ordering), Acknowledgment Number (for reliability), Header Length, Flags (SYN, ACK, FIN, etc. for connection state), Receive Window (for flow control), Checksum, and an Urgent Data Pointer.

> [!question]- Q: Justify the necessity of "pushing data" (PSH) and "urgent data" (URG) in TCP with scenarios.
> **Answer:**
> - **PSH (Push):** Instructs the receiver to pass the data to the application layer immediately without waiting for the buffer to fill up. *Scenario:* Interactive terminal sessions (like SSH or Telnet) where every keystroke needs to be processed instantly.
> - **URG (Urgent):** Indicates that the data in the segment is highly prioritized and the application should be notified immediately, using the Urgent Pointer to find the data. *Scenario:* Sending a `Ctrl+C` interrupt command to abort a remote process that is currently receiving a massive file transfer.

> [!question]- Q: Show TCP connection establishment steps, mention disadvantages of a 2-way handshake, and how to overcome them.
> **Answer:**
> TCP uses a **3-Way Handshake** to establish a connection:
> 1. Client sends a **SYN** (Sequence = x) segment.
> 2. Server replies with a **SYNACK** (Seq = y, Ack = x + 1).
> 3. Client replies with an **ACK** (Ack = y + 1).
> 
> **Disadvantages of a 2-Way Handshake:**
> A 2-way handshake is vulnerable to delayed duplicates. A delayed connection request from an old session could arrive at the server, and the server would immediately allocate resources and open a "half-open" connection. Worse, delayed data from that old session could be accepted as valid new data.
> **Solution:** The 3-way handshake forces the client to explicitly acknowledge the server's response. If a delayed SYN reaches the server, the server sends a SYNACK. The client (knowing it didn't request this) will send a RST (Reset) to safely abort the ghost connection before any resources are fully committed.

> [!question]- Q: Describe the TCP connection closing procedure with a labeled timing diagram.
> **Answer:**
> TCP connection termination is a 4-step process:
> 1. **Client sends FIN:** The client application wishes to close, sending a segment with the FIN flag set (enters `FIN_WAIT_1`).
> 2. **Server sends ACK:** Server acknowledges the FIN (enters `CLOSE_WAIT`). The client receives the ACK (enters `FIN_WAIT_2`).
> 3. **Server sends FIN:** Once the server is done sending its remaining data, it sends its own FIN segment (enters `LAST_ACK`).
> 4. **Client sends ACK:** Client acknowledges the server's FIN and enters the `TIME_WAIT` state (waiting ~30-120 seconds to ensure the ACK isn't lost). The server receives the ACK and closes. The client closes after the timer expires.

> [!question]- Q: What is the difference between flow control and congestion control?
> **Answer:**
> - **Flow Control:** A point-to-point mechanism protecting the *receiver*. It ensures the sender does not overwhelm the receiver's application buffer by transmitting data faster than it can be read. TCP handles this using the Receive Window (`rwnd`).
> - **Congestion Control:** A network-wide mechanism protecting the *network core*. It ensures the sender throttles its transmission rate when routers in the network are overwhelmed, preventing packet loss and gridlock.

> [!question]- Q: What is a SYN Flooding Attack and how is it handled?
> **Answer:**
> A SYN Flooding Attack is a Denial of Service (DoS) attack where the attacker sends a massive flurry of TCP SYN requests (often with spoofed source IPs) but never completes the 3-way handshake with the final ACK. The server allocates memory for each half-open connection until its resources are exhausted, preventing legitimate users from connecting.
> **Prevention:** Modern OSs use **SYN Cookies**. Instead of allocating memory immediately upon receiving a SYN, the server mathematically encodes connection details into the Initial Sequence Number (the cookie) of the SYNACK. Only when the client responds with a valid ACK containing that cookie does the server allocate memory.

> [!question]- Q: Explain sender/receiver window sizes in Selective Repeat (SR) vs Go-Back-N (GBN).
> **Answer:**
> - **Go-Back-N (GBN):** Uses a sender window of size $N$ to pipeline packets, but the receiver window size is exactly **1**. The receiver only accepts packets perfectly in order and discards any out-of-order packets.
> - **Selective Repeat (SR):** Uses a sender window of size $N$ AND a receiver window of size $N$. The receiver buffers out-of-order packets, allowing the sender to only retransmit the specific packets that were lost, rather than retransmitting the entire window.

> [!question]- Q: Justify the categorization of port addresses by IANA.
> **Answer:**
> IANA (Internet Assigned Numbers Authority) categorizes port numbers (0 to 65535) into three ranges to organize traffic and enforce security:
> 1. **Well-known Ports (0-1023):** Strictly reserved for standardized, universally recognized system services (e.g., HTTP on 80, HTTPS on 443, DNS on 53). Usually requires admin/root privileges to bind to.
> 2. **Registered Ports (1024-49151):** Assigned to specific applications or vendor services by request to prevent conflicts (e.g., MySQL on 3306).
> 3. **Dynamic/Private Ports (49152-65535):** Ephemeral ports assigned temporarily by the OS to client applications when they initiate an outbound connection.

> [!question]- Q: Justify the application of unreliable UDP in services like RIP.
> **Answer:**
> RIP (Routing Information Protocol) broadcasts routing table updates to neighboring routers periodically (e.g., every 30 seconds). Since the updates are constant and repetitive, losing a single RIP packet is not a critical failure—the neighbor will simply receive the complete table in the next broadcast a few seconds later. Using TCP's connection setups and retransmissions would create unnecessary overhead and delay for a protocol that naturally heals itself via repetition.


## Chapter 4: Network Layer

> [!question]- Q: An ISP is granted a block of addresses starting with 160.25.0.0/16. Design the sub-blocks and show the address allocation for 64 customers needing 256 addresses each, 128 customers needing 128 addresses each, and 128 customers needing 64 addresses each. Find how many addresses remain available.
> **Answer:**
> **Total Addresses Granted:** `/16` means $32 - 16 = 16$ host bits. Total = $2^{16} = 65,536$ addresses.
> 
> **Group 1: 64 customers $\times$ 256 addresses**
> - 256 addresses requires 8 host bits. So the prefix is $32 - 8 = /24$.
> - Total addresses used: $64 \times 256 = 16,384$.
> - *Allocation Block:* Start at `160.25.0.0/16`. Give `160.25.0.0/24` to Customer 1, `160.25.1.0/24` to Customer 2, ..., `160.25.63.0/24` to Customer 64.
> - Next available address: `160.25.64.0`.
> 
> **Group 2: 128 customers $\times$ 128 addresses**
> - 128 addresses requires 7 host bits. So the prefix is $32 - 7 = /25$.
> - Total addresses used: $128 \times 128 = 16,384$.
> - *Allocation Block:* Start at `160.25.64.0`. Two `/25` subnets fit into one `/24`.
> - Give `160.25.64.0/25` to Customer 1, `160.25.64.128/25` to Customer 2, up to `160.25.127.128/25` to Customer 128.
> - Next available address: `160.25.128.0`.
> 
> **Group 3: 128 customers $\times$ 64 addresses**
> - 64 addresses requires 6 host bits. So the prefix is $32 - 6 = /26$.
> - Total addresses used: $128 \times 64 = 8,192$.
> - *Allocation Block:* Start at `160.25.128.0`. Four `/26` subnets fit into one `/24`. We need $128 / 4 = 32$ blocks of `/24`.
> - Give `160.25.128.0/26` to Customer 1, `160.25.128.64/26` to Customer 2, up to `160.25.159.192/26` to Customer 128.
> - Next available address: `160.25.160.0`.
> 
> **Addresses Remaining:**
> Total used = $16,384 + 16,384 + 8,192 = 40,960$ addresses.
> Remaining = $65,536 - 40,960 = 24,576$ addresses.
> *(Which corresponds to the block from `160.25.160.0/20` and `160.25.176.0/20`, etc. overall `160.25.160.0` to `160.25.255.255`)*.

> [!question]- Q: Contrast process-to-process, host-to-host, and source-to-destination delivery.
> **Answer:**
> - **Process-to-Process (Transport Layer):** Ensures data is delivered to the correct application/socket running on a machine, using Port Numbers (e.g., TCP/UDP).
> - **Host-to-Host (Network Layer):** Ensures packets travel globally across the Internet from the source computer to the destination computer, using IP Addresses.
> - **Node-to-Node (Link Layer):** Ensures frames travel locally from one specific device to the physically adjacent device over a single link, using MAC Addresses.

> [!question]- Q: Explain why IPv4 is a "best effort" delivery service.
> **Answer:**
> IPv4 is deemed "best effort" because it makes an earnest attempt to deliver packets to their destination but provides **zero guarantees**. It does not guarantee that the packet will arrive, that packets will arrive in the correct order, that they will arrive within a specific time limit, or that a minimum bandwidth will be maintained. It leaves reliability and error recovery entirely up to the Transport Layer (TCP).

> [!question]- Q: Write the significance of the 8-bit IPv4 service type (TOS) field.
> **Answer:**
> The Type of Service (TOS) field is used to categorize and prioritize different types of IP datagrams (Differentiated Services / DiffServ). It allows routers to prioritize real-time traffic (like VoIP) over background traffic (like file downloads) during periods of network congestion. It also contains bits for Explicit Congestion Notification (ECN).

> [!question]- Q: For 192.168.10.0/26, find the number of subnets, hosts per subnet, first/last valid hosts, and broadcast addresses.
> **Answer:**
> The base address `192.168.10.0` is a Class C address with a default `/24` mask.
> A `/26` mask means $26 - 24 = 2$ bits were borrowed for subnetting.
> - **Number of subnets:** $2^2 = 4$ subnets.
> - **Hosts per subnet:** With $32 - 26 = 6$ host bits, $2^6 - 2 = 64 - 2 = 62$ usable hosts per subnet.
> 
> **Subnet Details:**
> - **Subnet 1:** Network: `192.168.10.0`, Usable: `192.168.10.1` to `192.168.10.62`, Broadcast: `192.168.10.63`
> - **Subnet 2:** Network: `192.168.10.64`, Usable: `192.168.10.65` to `192.168.10.126`, Broadcast: `192.168.10.127`
> - **Subnet 3:** Network: `192.168.10.128`, Usable: `192.168.10.129` to `192.168.10.190`, Broadcast: `192.168.10.191`
> - **Subnet 4:** Network: `192.168.10.192`, Usable: `192.168.10.193` to `192.168.10.254`, Broadcast: `192.168.10.255`
