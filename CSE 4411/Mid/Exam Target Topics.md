
---

## 📈 Chapter 1: Network Edge & Core
- **Bandwidth-Delay Product (BDP):** The notes mention delay components, but the exam frequently asks you to calculate BDP ($R \times d_{prop}$). You need to know that this represents the "number of bits that fit on the link."
- **Comparing Switching Models:** Be ready to write out the pros/cons of Packet-Switched vs. Circuit-Switched networks. 
- **Security Scenarios:** Don't just know what a DoS attack is; know the difference between *Bandwidth Flooding* vs. *Connection Flooding*, and how a *Man-in-the-Middle* attack works conceptually.
- **Constant vs Variable Delays:** You must know that Transmission and Propagation are largely constant, while Queueing is variable.

## 🌐 Chapter 2: Application Layer
- **P2P vs Client-Server Math:** It's not enough to know the formulas; the exam asks you to *deduce* why P2P is more scalable by pointing out that the denominator ($u_s + \sum u_i$) grows in P2P, keeping the time flat.
- **Timing Diagrams:** 
  - *Cookies:* You must be able to draw the interaction flow (`Set-cookie` from server, `Cookie` attached to next client request).
  - *DNS:* Be able to trace an Iterated DNS query specifically.
- **Device-Specific DNS:** Knowing that a "lightweight device" should use *Recursive* resolution to offload the heavy lifting to the Local DNS.

## 🚚 Chapter 3: Transport Layer (Highly Tested)
- **TCP Flags in Scenarios (Often missing in raw notes):** 
  - *PSH (Push):* Used when data must be processed immediately without waiting for buffers (e.g., interactive Telnet/SSH).
  - *URG (Urgent):* Used to interrupt processes (e.g., hitting `Ctrl+C`).
- **SYN Flooding & SYN Cookies:** The notes mention the 3-way handshake prevents ghost connections, but the exam specifically asks about *SYN Flooding* attacks. You need to mention **SYN Cookies** as the prevention mechanism.
- **UDP Justifications:** The exam loves asking *why* we use UDP for specific things. A favorite question is why **RIP** (Routing Information Protocol) uses UDP. *(Answer: It broadcasts updates every 30 seconds. If a packet drops, the next broadcast fixes it. TCP overhead is useless here).*
- **Window Sizes (GBN vs SR):** This is a guaranteed question. Know that GBN Receiver Window = 1, while SR Receiver Window = N. 
- **Port Categorizations:** Be able to list the three IANA categories: Well-known (0-1023), Registered (1024-49151), and Dynamic/Private (49152-65535).

## 🗺️ Chapter 4: Network Layer
- **Hardcore Subnetting (VLSM):** The exam will give you a block (e.g., `/16`) and ask you to carve it up for different sized customers (e.g., 64 customers needing 256 addresses each). You must know how to calculate the required host bits and assign contiguous blocks.
- **IPv4 Header Deep-Dives:** 
  - *TOS Field:* Know that the 8-bit Type of Service field is used for QoS/DiffServ (prioritizing VoIP over file downloads).
  - *Best Effort:* Be ready to explain exactly what "Best Effort" means (zero guarantees on delivery, order, timing, or bandwidth).
- **Delivery Scopes:** Understand the distinction between Node-to-Node (MAC/Link), Host-to-Host (IP/Network), and Process-to-Process (Port/Transport).

---