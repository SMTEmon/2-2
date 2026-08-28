---
title: "CSE 4411 Final Exam Master Math & Problem Book"
course: "CSE 4411"
tags:
  - cse4411
  - networking
  - math-notes
  - worked-problems
  - exam-prep
  - formulas
aliases:
  - Final Exam Math Notes
  - CSE 4411 Master Math Book
---

# CSE 4411 Final Exam Master Math & Problem Book

> [!abstract] Executive Summary
> This master document consolidates **every mathematical problem type on the official CSE 4411 Final Exam syllabus**.
> 
> Each problem displays the **Problem Statement** and **Governing Formulas** upfront, with the **Complete Step-by-Step Numerical Derivation hidden inside an interactive foldable dropdown (`> [!example]-`)**. Click any solution block to expand and test yourself!

---

## 📑 Master Math Problem Index & Checklist

### Module 1: Chapter 4 — Data Plane (DHCP to IPv6)
- [ ] [[#Problem 1.1: NAT Translation Table State & Port Capacity|Problem 1.1: NAT Translation Table State & Port Capacity]]
- [ ] [[#Problem 1.2: IPv6 Tunneling Wire Overhead & Payload Length|Problem 1.2: IPv6 Tunneling Wire Overhead & Payload Length]]
- [ ] [[#Problem 1.3: Incremental Checksum Recalculation (RFC 1624)|Problem 1.3: Incremental Checksum Recalculation (RFC 1624)]]

### Module 2: Chapter 5 — Control Plane (Routing without SDN)
- [ ] [[#Problem 2.1: Dijkstra's Link-State Execution Matrix & Shortest Path Tree|Problem 2.1: Dijkstra's Link-State Execution Matrix & Shortest Path Tree]]
- [ ] [[#Problem 2.2: Distance-Vector Convergence via Bellman-Ford Equation|Problem 2.2: Distance-Vector Convergence via Bellman-Ford Equation]]
- [ ] [[#Problem 2.3: Count-to-Infinity Step-by-Step Routing Loop Trace|Problem 2.3: Count-to-Infinity Step-by-Step Routing Loop Trace]]
- [ ] [[#Problem 2.4: OSPF Interface Metric / Cost Calculation|Problem 2.4: OSPF Interface Metric / Cost Calculation]]
- [ ] [[#Problem 2.5: BGP Multi-Attribute Path Selection Elimination|Problem 2.5: BGP Multi-Attribute Path Selection Elimination]]

### Module 3: Chapter 6 — Link Layer and LANs
- [ ] [[#Problem 3.1: CRC-32 Modulo-2 Polynomial Division & Error Detection|Problem 3.1: CRC-32 Modulo-2 Polynomial Division & Error Detection]]
- [ ] [[#Problem 3.2: Slotted ALOHA Maximum Throughput & Limit Derivation|Problem 3.2: Slotted ALOHA Maximum Throughput & Limit Derivation]]
- [ ] [[#Problem 3.3: Pure ALOHA Vulnerable Period & Maximum Throughput|Problem 3.3: Pure ALOHA Vulnerable Period & Maximum Throughput]]
- [ ] [[#Problem 3.4: CSMA/CD Minimum Frame Size & Maximum Link Distance|Problem 3.4: CSMA/CD Minimum Frame Size & Maximum Link Distance]]
- [ ] [[#Problem 3.5: Binary Exponential Backoff Wait Slot Calculation|Problem 3.5: Binary Exponential Backoff Wait Slot Calculation]]
- [ ] [[#Problem 3.6: IEEE 802.1Q VLAN Tag 12-bit VID & PCP Bit-Slicing|Problem 3.6: IEEE 802.1Q VLAN Tag 12-bit VID & PCP Bit-Slicing]]

### Module 4: Physical Layer — Digital Transmission
- [ ] [[#Problem 4.1: Pulse Code Modulation (PCM) Nyquist Rate & SQNR|Problem 4.1: Pulse Code Modulation (PCM) Nyquist Rate & SQNR]]
- [ ] [[#Problem 4.2: B8ZS Scrambling Pulse Sequence Substitution Trace|Problem 4.2: B8ZS Scrambling Pulse Sequence Substitution Trace]]
- [ ] [[#Problem 4.3: HDB3 Scrambling State & Parity Counter Trace|Problem 4.3: HDB3 Scrambling State & Parity Counter Trace]]
- [ ] [[#Problem 4.4: 4B/5B Block Coding Overhead & Run-Length Bounds|Problem 4.4: 4B/5B Block Coding Overhead & Run-Length Bounds]]

### Module 5: Cross-Layer Signature Math — Packet Hex Dump Arithmetic
- [ ] [[#Problem 5.1: Raw IPv6 Datagram Header Hex Decoding|Problem 5.1: Raw IPv6 Datagram Header Hex Decoding]]
- [ ] [[#Problem 5.2: IEEE 802.1Q Tagged Ethernet Frame Hex Dissection|Problem 5.2: IEEE 802.1Q Tagged Ethernet Frame Hex Dissection]]
- [ ] [[#Problem 5.3: Nested ICMP Traceroute Error Hex Decoding|Problem 5.3: Nested ICMP Traceroute Error Hex Decoding]]

---

# 📦 Module 1: Chapter 4 — Data Plane (DHCP to IPv6)

---

### Problem 1.1: NAT Translation Table State & Port Capacity

#### 📝 Problem Statement
A small office network has 150 active workstations behind a single NAT-enabled gateway with public IP `203.0.113.1`. 
1. If each host maintains an average of 40 active TCP web connections, calculate the total number of simultaneous NAT translation table entries required.
2. What is the theoretical maximum number of concurrent outgoing TCP connections the gateway can support, assuming ports `0–1023` are reserved?
3. Host `192.168.1.55` opens an outgoing connection from port `3344` to web server `142.250.190.46:80`. Show the exact headers before and after NAT translation, and specify the new NAT table entry if the assigned public port is `51000`.

#### 🧮 Governing Formulas
$$\text{Total Table Entries} = N_{\text{hosts}} \times N_{\text{conns/host}}$$
$$\text{Max NAT Capacity} = 2^{16} - 1024 = 65,536 - 1024 = \mathbf{64,512 \text{ connections}}$$

> [!example]- Click to Reveal Full Step-by-Step Solution
> **1. Total Table Entries Required:**
> $$\text{Entries} = 150 \times 40 = \mathbf{6,000 \text{ concurrent entries}}$$
> 
> **2. Theoretical Maximum Capacity:**
> The TCP port field is 16 bits wide:
> $$\text{Total Ports} = 2^{16} = 65,536$$
> Subtracting well-known reserved system ports ($0 - 1023 = 1024\text{ ports}$):
> $$\text{Usable Outgoing Ports} = 65,536 - 1024 = \mathbf{64,512 \text{ simultaneous TCP sessions}}$$
> *(Since $6,000 \ll 64,512$, the single public IP has plenty of capacity).*
> 
> **3. Wire Header & Table Mapping:**
> - **On Private LAN (Before NAT):**
>   $$\text{Source: } 192.168.1.55:3344 \quad \longrightarrow \quad \text{Destination: } 142.250.190.46:80$$
> - **On Public WAN (After NAT):**
>   $$\text{Source: } 203.0.113.1:51000 \quad \longrightarrow \quad \text{Destination: } 142.250.190.46:80$$
> - **NAT Table Entry:**
>   $$\text{WAN Side: } (203.0.113.1, 51000) \quad \longleftrightarrow \quad \text{LAN Side: } (192.168.1.55, 3344) \quad [\text{TCP}]$$

---

### Problem 1.2: IPv6 Tunneling Wire Overhead & Payload Length

#### 📝 Problem Statement
An IPv6 host transmits a packet carrying a 1400-byte application data payload over TCP across an IPv4-only transit network using **IPv6-in-IPv4 Tunneling**.
- IPv6 Base Header size $= 40\text{ bytes}$.
- TCP Header size $= 20\text{ bytes}$.
- Outer IPv4 Tunnel Header size $= 20\text{ bytes}$.

**Calculate:**
1. The value written into the IPv6 header's **`Payload Length`** field.
2. The value written into the outer IPv4 header's **`Total Length`** field.
3. The total protocol header overhead percentage across the IPv4 tunnel link.

#### 🧮 Governing Formulas
$$\text{IPv6 Payload Length} = \text{TCP Header (20B)} + \text{Application Data}$$
$$\text{IPv4 Total Length} = \text{IPv4 Header (20B)} + \text{IPv6 Base Header (40B)} + \text{IPv6 Payload}$$
$$\text{Overhead \%} = \frac{\text{Total Header Bytes}}{\text{Total Packet Size on Wire}} \times 100\%$$

> [!example]- Click to Reveal Full Step-by-Step Solution
> **1. IPv6 Header `Payload Length` Field:**
> In IPv6, `Payload Length` counts all bytes *following* the fixed 40-byte base header:
> $$\text{IPv6 Payload Length} = 20\text{ (TCP)} + 1400\text{ (Data)} = \mathbf{1420 \text{ Bytes}}$$
> 
> **2. Outer IPv4 Header `Total Length` Field:**
> The outer IPv4 datagram encapsulates the entire native IPv6 packet:
> $$\text{Total Size of Native IPv6 Packet} = 40\text{ (IPv6 Base)} + 1420\text{ (Payload)} = 1460\text{ Bytes}$$
> Adding the 20-byte outer IPv4 header:
> $$\text{IPv4 Total Length} = 20\text{ (IPv4)} + 1460 = \mathbf{1480 \text{ Bytes}}$$
> *(Note: $1480 \le 1500\text{ MTU}$, so no outer fragmentation occurs).*
> 
> **3. Protocol Header Overhead Percentage:**
> $$\text{Total Header Bytes} = 20\text{ (IPv4)} + 40\text{ (IPv6)} + 20\text{ (TCP)} = 80\text{ Bytes}$$
> $$\text{Overhead \%} = \frac{80}{1480} \times 100\% = \mathbf{5.41\%}$$

---

### Problem 1.3: Incremental Checksum Recalculation (RFC 1624)

#### 📝 Problem Statement
A NAT router rewrites a packet's 16-bit TCP Source Port from $m = \text{0x1000}$ ($4096$) to $m' = \text{0x5000}$ ($20480$). The original 16-bit TCP checksum was $C = \text{0x8550}$. Using RFC 1624 incremental update arithmetic, compute the new checksum $C'$.

#### 🧮 Governing Formulas
$$\sim C' = \sim C + \sim m + m'$$
*(where $\sim$ is 1's complement bitwise NOT, and addition is 1's complement with end-around carry).*

> [!example]- Click to Reveal Full Step-by-Step Solution
> **Step 1: Compute 1's Complements ($\sim$):**
> - $\sim C = \sim\text{0x8550} = \text{0x7AAF}$
> - $\sim m = \sim\text{0x1000} = \text{0xEFFF}$
> - $m' = \text{0x5000}$
> 
> **Step 2: Add in 1's Complement Arithmetic:**
> $$\text{Sum} = \text{0x7AAF} + \text{0xEFFF} + \text{0x5000}$$
> 1. $\text{0x7AAF} + \text{0xEFFF} = \text{0x16AAE} \implies \text{0x6AAE} + 1 = \text{0x6AAF}$
> 2. $\text{0x6AAF} + \text{0x5000} = \text{0xBAAF}$
> 
> **Step 3: Final Complement to get $C'$:**
> $$C' = \sim\text{0xBAAF} = \mathbf{0x4550}$$

---

# 🧠 Module 2: Chapter 5 — Control Plane (Routing without SDN)

---

### Problem 2.1: Dijkstra's Link-State Execution Matrix & Shortest Path Tree

#### 📝 Problem Statement
Given the 6-node network graph below with edge costs:
- $c(u, v) = 2$, $c(u, x) = 1$, $c(v, x) = 2$, $c(v, w) = 3$, $c(x, y) = 1$, $c(w, y) = 1$, $c(w, z) = 5$, $c(y, z) = 2$.

```
          2
     v -------- w
   / | \      / | \
  2  |  3    1  |  5
 /   2   \  /   |   \
u -- x --- y ---+--- z
  1     1     2
```

Compute the complete step-by-step Dijkstra execution table starting from source node **$u$**, and construct the shortest path forwarding table.

#### 🧮 Governing Formulas
$$D(v) = \min( D(v), D(w) + c(w, v) )$$

> [!example]- Click to Reveal Full Step-by-Step Solution
> **Step-by-Step Execution Table:**
> 
> | Step | Confirmed Set $N'$ | $D(v), p(v)$ | $D(w), p(w)$ | $D(x), p(x)$ | $D(y), p(y)$ | $D(z), p(z)$ |
> | :---: | :--- | :---: | :---: | :---: | :---: | :---: |
> | **0** | $\{u\}$ | $2, u$ | $\infty$ | **$1, u$** *(Min)* | $\infty$ | $\infty$ |
> | **1** | $\{u, x\}$ | **$2, u$** *(Min)* | $\infty$ | $1, u$ | $2, x$ | $\infty$ |
> | **2** | $\{u, x, v\}$ | $2, u$ | $5, v$ | $1, u$ | **$2, x$** *(Min)* | $\infty$ |
> | **3** | $\{u, x, v, y\}$ | $2, u$ | **$3, y$** *(Min)* | $1, u$ | $2, x$ | $4, y$ |
> | **4** | $\{u, x, v, y, w\}$ | $2, u$ | $3, y$ | $1, u$ | $2, x$ | **$4, y$** *(Min)* |
> | **5** | $\{u, x, v, y, w, z\}$ | $2, u$ | $3, y$ | $1, u$ | $2, x$ | $4, y$ |
> 
> **Resulting Forwarding Table at Node $u$:**
> 
> | Destination | Shortest Path | Least Cost | Next-Hop Outgoing Interface |
> | :---: | :--- | :---: | :---: |
> | **$v$** | $u \to v$ | **2** | Direct Link $(u, v)$ |
> | **$x$** | $u \to x$ | **1** | Direct Link $(u, x)$ |
> | **$y$** | $u \to x \to y$ | **2** | Interface towards $x$ |
> | **$w$** | $u \to x \to y \to w$ | **3** | Interface towards $x$ |
> | **$z$** | $u \to x \to y \to z$ | **4** | Interface towards $x$ |

---

### Problem 2.2: Distance-Vector Convergence via Bellman-Ford Equation

#### 📝 Problem Statement
A 3-node linear network has direct link costs: $c(x, y) = 4$, $c(y, z) = 1$, and $c(x, z) = \infty$ (no direct link). 
Show the initial distance vectors at each node, the first exchange iteration, and the final converged routing tables using the Bellman-Ford equation.

#### 🧮 Governing Formulas
$$d_x(y) = \min_{v \in \text{Neighbors}(x)} \{ c(x, v) + d_v(y) \}$$

> [!example]- Click to Reveal Full Step-by-Step Solution
> **Iteration 0 (Initial Local State):**
> - **Node $x$ Vector:** $D_x(x) = 0, D_x(y) = 4, D_x(z) = \infty$
> - **Node $y$ Vector:** $D_y(x) = 4, D_y(y) = 0, D_y(z) = 1$
> - **Node $z$ Vector:** $D_z(x) = \infty, D_z(y) = 1, D_z(z) = 0$
> 
> **Iteration 1 (Vectors Exchanged with Immediate Neighbors):**
> - **Node $x$ computes $D_x(z)$:**
>   $$D_x(z) = c(x, y) + D_y(z) = 4 + 1 = \mathbf{5} \quad (\text{via } y)$$
> - **Node $z$ computes $D_z(x)$:**
>   $$D_z(x) = c(z, y) + D_y(x) = 1 + 4 = \mathbf{5} \quad (\text{via } y)$$
> - **Node $y$ computes $D_y(x)$ and $D_y(z)$:** Unchanged ($D_y(x) = 4, D_y(z) = 1$).
> 
> **Iteration 2 (New Vectors Announced):**
> - Nodes $x$ and $z$ announce their new distance estimates ($5$).
> - No node discovers a shorter path $\implies$ **Full Convergence in 2 Iterations!**

---

### Problem 2.3: Count-to-Infinity Step-by-Step Routing Loop Trace

#### 📝 Problem Statement
In the network from Problem 2.2 ($x \overset{4}{\longleftrightarrow} y \overset{1}{\longleftrightarrow} z$), link $(x, y)$ cost suddenly jumps from $4 \to 60$. 
Show the first 3 iterations of the resulting **Count-to-Infinity** loop between nodes $y$ and $z$ when Poisoned Reverse is **disabled**.

#### 🧮 Governing Formulas
$$D_y(x) = \min \{ c(y, x) + D_x(x), c(y, z) + D_z(x) \}$$

> [!example]- Click to Reveal Full Step-by-Step Solution
> **Initial State Before Link Change:** $D_y(x) = 4$, $D_z(x) = 5$ (via $y$).
> 
> **Iteration 1 (Link $(x, y)$ increases to $60$):**
> - Node $y$ checks its two paths to $x$:
>   - Direct to $x$: $c(y, x) + 0 = 60 + 0 = 60$.
>   - Through $z$: $c(y, z) + D_z(x) = 1 + 5 = \mathbf{6}$.
> - Node $y$ sets $D_y(x) = 6$ (via $z$) and advertises $D_y(x) = 6$ to $z$.
> 
> **Iteration 2 ($z$ receives $y$'s update):**
> - Node $z$ recalculates its cost to $x$:
>   $$D_z(x) = c(z, y) + D_y(x) = 1 + 6 = \mathbf{7} \quad (\text{via } y)$$
> - Node $z$ advertises $D_z(x) = 7$ back to $y$.
> 
> **Iteration 3 ($y$ receives $z$'s update):**
> - Node $y$ recalculates:
>   $$D_y(x) = c(y, z) + D_z(x) = 1 + 7 = \mathbf{8} \quad (\text{via } z)$$
> - **Result:** Packets bounce in a loop $y \leftrightarrow z$, incrementing costs slowly step-by-step ($4 \to 6 \to 7 \to 8 \dots$) until reaching $60$ after 56 message iterations!

---

### Problem 2.4: OSPF Interface Metric / Cost Calculation

#### 📝 Problem Statement
An enterprise OSPF network uses the standard Cisco reference bandwidth $BW_{\text{ref}} = 100\text{ Mbps}$ ($10^8\text{ bps}$). 
Calculate the OSPF cost for:
1. FastEthernet link ($100\text{ Mbps}$).
2. GigabitEthernet link ($1\text{ Gbps}$).
3. TenGigabitEthernet link ($10\text{ Gbps}$) with auto-cost reference bandwidth configured to $100\text{ Gbps}$ ($10^{11}\text{ bps}$).

#### 🧮 Governing Formulas
$$\text{OSPF Metric (Cost)} = \max\left( 1, \left\lfloor \frac{\text{Reference Bandwidth}}{\text{Interface Bandwidth}} \right\rfloor \right)$$

> [!example]- Click to Reveal Full Step-by-Step Solution
> **1. FastEthernet ($100\text{ Mbps}$):**
> $$\text{Cost} = \frac{10^8\text{ bps}}{10^8\text{ bps}} = \mathbf{1}$$
> 
> **2. GigabitEthernet ($1\text{ Gbps}$ under default $100\text{M}$ reference):**
> $$\text{Cost} = \max\left(1, \frac{10^8}{10^9}\right) = \max(1, 0.1) = \mathbf{1}$$
> *(Note: Without tuning reference bandwidth, 100M and 1G share cost 1, preventing proper path discrimination).*
> 
> **3. TenGigabitEthernet ($10\text{ Gbps}$ under $100\text{G}$ reference):**
> $$\text{Cost} = \frac{100 \times 10^9\text{ bps}}{10 \times 10^9\text{ bps}} = \mathbf{10}$$

---

### Problem 2.5: BGP Multi-Attribute Path Selection Elimination

#### 📝 Problem Statement
Router $R1$ in AS 100 learns 4 distinct BGP routes to prefix `198.51.100.0/24`:

| Route | `LOCAL_PREF` | `AS-PATH` | Origin | MED | Internal OSPF Cost to Egress | Learned Via |
| :---: | :---: | :--- | :---: | :---: | :---: | :---: |
| **Route 1** | 100 | `AS 200 -> AS 300` (2 hops) | IGP | 50 | 20 | eBGP |
| **Route 2** | 100 | `AS 400 -> AS 500` (2 hops) | IGP | 10 | **8** | eBGP |
| **Route 3** | **150** | `AS 600 -> AS 700 -> AS 800` (3 hops)| IGP | 100 | 45 | iBGP |
| **Route 4** | 100 | `AS 900` (1 hop) | IGP | 0 | 12 | eBGP |

**Determine:**
1. Which route is selected if all 4 are considered?
2. If Route 3 is removed, which route is selected?

#### 🧮 Governing Elimination Hierarchy
$$\text{LOCAL\_PREF (Highest)} \implies \text{AS-PATH (Shortest)} \implies \text{Hot Potato (Lowest OSPF Cost)} \implies \text{MED (Lowest)}$$

> [!example]- Click to Reveal Full Step-by-Step Solution
> **1. Considering All 4 Routes:**
> - Step 1: Compare `LOCAL_PREF`:
>   - Route 1: 100, Route 2: 100, **Route 3: 150**, Route 4: 100.
> - **Winner:** **Route 3 is selected immediately!** (`LOCAL_PREF` strictly overrides shorter `AS-PATH` and lower internal cost).
> 
> **2. If Route 3 is Removed (Evaluating Routes 1, 2, 4):**
> - Step 1: `LOCAL_PREF` is tied ($100 = 100 = 100$).
> - Step 2: Compare `AS-PATH` Length:
>   - Route 1: 2 hops, Route 2: 2 hops, **Route 4: 1 hop**.
> - **Winner:** **Route 4 is selected!** ($1\text{ AS hop} < 2\text{ AS hops}$).

---

# 🔗 Module 3: Chapter 6 — Link Layer and LANs

---

### Problem 3.1: CRC-32 Modulo-2 Polynomial Division & Error Detection

#### 📝 Problem Statement
Given:
- Data bitstream $D = 101001$ ($d = 6\text{ bits}$).
- Generator polynomial $G(x) = x^3 + x + 1 \implies G = 1011$ ($r = 3\text{ bits}$).

**Calculate:**
1. The 3-bit CRC checksum $R$.
2. The transmitted bit sequence $T$.
3. Show how the receiver detects an error if the 2nd bit from the left is inverted during transmission ($101001011 \to 1\mathbf{1}1001011$).

#### 🧮 Governing Formulas
$$R = \text{remainder}\left( \frac{D \cdot 2^r}{G} \right), \quad T = D \cdot 2^r \oplus R$$

> [!example]- Click to Reveal Full Step-by-Step Solution
> **1. Calculate Checksum $R$:**
> - Append $r=3$ zeros to $D$: $D \cdot 2^3 = 101001000$.
> - Perform Modulo-2 long division (XOR subtraction):
> 
> ```
>             100100   (Quotient)
>         ------------
> 1011  ) 101001000
>         1011
>         ----
>          001010
>            1011
>            ----
>            0001000
>               1011
>               ----
>               0011  (Remainder R = 011)
> ```
> **CRC Checksum $R = \mathbf{011}$**.
> 
> **2. Transmitted Sequence ($T$):**
> $$T = D \cdot 2^3 \oplus R = 101001\mathbf{011}$$
> 
> **3. Receiver Error Verification on Bit Inversion ($111001011$):**
> - Divide received $111001011$ by $G = 1011$:
> 
> ```
>             110011
>         ------------
> 1011  ) 111001011
>         1011
>         ----
>          1010
>          1011
>          ----
>          0001011
>             1011
>             ----
>             000011  (Remainder = 011 != 000)
> ```
> - **Remainder $\ne 0 \implies$ Error Detected! Frame is dropped.**

---

### Problem 3.2: Slotted ALOHA Maximum Throughput & Limit Derivation

#### 📝 Problem Statement
A shared broadcast channel has $N$ active stations running Slotted ALOHA. Each station transmits in a slot with probability $p$.
1. Derive the expression for channel efficiency $S(p)$.
2. Find the optimal probability $p^*$ that maximizes throughput.
3. Compute the maximum theoretical channel utilization in the limit $N \to \infty$.

#### 🧮 Governing Formulas
$$S(p) = N \cdot p (1 - p)^{N-1}$$
$$\lim_{N \to \infty} \left(1 - \frac{1}{N}\right)^{N-1} = \frac{1}{e}$$

> [!example]- Click to Reveal Full Step-by-Step Solution
> **1. Probability of Successful Transmission:**
> - Probability that a specific station transmits successfully $= p (1 - p)^{N-1}$.
> - Since any of the $N$ stations can succeed:
>   $$S(p) = N p (1 - p)^{N-1}$$
> 
> **2. Find $p^*$ by Differentiation:**
> $$\frac{d S(p)}{dp} = N (1 - p)^{N-1} - N(N - 1) p (1 - p)^{N-2} = 0$$
> Dividing by $N (1 - p)^{N-2}$:
> $$(1 - p) - (N - 1) p = 0 \implies 1 - p - N p + p = 0 \implies N p = 1 \implies \mathbf{p^* = \frac{1}{N}}$$
> 
> **3. Maximum Throughput Limit ($N \to \infty$):**
> $$S_{max} = N \left(\frac{1}{N}\right) \left(1 - \frac{1}{N}\right)^{N-1} = \left(1 - \frac{1}{N}\right)^{N-1}$$
> As $N \to \infty$:
> $$S_{max} = \lim_{N \to \infty} \left(1 - \frac{1}{N}\right)^{N-1} = \mathbf{\frac{1}{e} \approx 0.368 \ (36.8\%)}$$

---

### Problem 3.3: Pure ALOHA Vulnerable Period & Maximum Throughput

#### 📝 Problem Statement
In unslotted (Pure) ALOHA, frame transmission time is $T$. 
1. What is the length of the **vulnerable period** during which no other station may start transmitting?
2. Derive the maximum channel efficiency $S_{max}$ and compare it with Slotted ALOHA.

#### 🧮 Governing Formulas
$$\text{Vulnerable Period} = 2 \cdot T$$
$$S(G) = G \cdot e^{-2G}$$

> [!example]- Click to Reveal Full Step-by-Step Solution
> **1. Vulnerable Period:**
> If Station A begins transmission at $t_0$, any transmission started in the interval $[t_0 - T, t_0 + T]$ will collide with Station A.
> $$\text{Vulnerable Window} = (t_0 + T) - (t_0 - T) = \mathbf{2T}$$
> 
> **2. Throughput Derivation:**
> Under Poisson arrival rate $G$ frames per frame time:
> $$P(\text{No other frames in } 2T) = e^{-2G}$$
> $$S(G) = G \cdot e^{-2G}$$
> Differentiating with respect to $G$:
> $$\frac{d S}{d G} = e^{-2G} - 2G e^{-2G} = 0 \implies 1 - 2G = 0 \implies \mathbf{G^* = \frac{1}{2} = 0.5}$$
> 
> **Maximum Efficiency:**
> $$S_{max} = 0.5 \cdot e^{-2(0.5)} = \frac{1}{2e} = \mathbf{\frac{1}{2e} \approx 0.184 \ (18.4\%)}$$
> *(Pure ALOHA achieves exactly half the throughput of Slotted ALOHA because its vulnerable window is doubled).*

---

### Problem 3.4: CSMA/CD Minimum Frame Size & Maximum Link Distance

#### 📝 Problem Statement
A $100\text{ Mbps}$ Fast Ethernet LAN (CSMA/CD) has a maximum cable length of $d = 1000\text{ meters}$. The propagation speed of electrical signals in the copper wire is $v = 2 \times 10^8\text{ m/s}$.
1. What is the one-way propagation delay $t_{prop}$?
2. Calculate the minimum frame size $L_{min}$ in bits and bytes to guarantee collision detection.
3. If the standard defines $L_{min} = 64\text{ Bytes}$, what is the maximum allowable distance between two nodes?

#### 🧮 Governing Formulas
$$t_{prop} = \frac{d}{v}, \quad t_{trans} \ge 2 \cdot t_{prop} \implies \frac{L_{min}}{R} \ge \frac{2 \cdot d}{v} \implies L_{min} \ge \frac{2 \cdot d \cdot R}{v}$$

> [!example]- Click to Reveal Full Step-by-Step Solution
> **1. Propagation Delay ($t_{prop}$):**
> $$t_{prop} = \frac{1000\text{ m}}{2 \times 10^8\text{ m/s}} = 5 \times 10^{-6}\text{ s} = \mathbf{5 \ \mu\text{s}}$$
> 
> **2. Minimum Frame Size ($L_{min}$):**
> $$L_{min} \ge 2 \cdot t_{prop} \times R = 2 \times (5 \times 10^{-6}\text{ s}) \times (100 \times 10^6\text{ bps}) = 1000\text{ bits} = \mathbf{125 \text{ Bytes}}$$
> 
> **3. Maximum Allowable Distance for $L_{min} = 64\text{ Bytes}$ ($512\text{ bits}$):**
> $$512 \ge \frac{2 \cdot d_{max} \cdot (100 \times 10^6)}{2 \times 10^8} \implies 512 \ge d_{max} \implies \mathbf{d_{max} = 512 \text{ meters}}$$

---

### Problem 3.5: Binary Exponential Backoff Wait Slot Calculation

#### 📝 Problem Statement
Two nodes on a $10\text{ Mbps}$ Ethernet LAN collide while transmitting. 
- Slot time $= 512\text{ bit times} = 51.2\ \mu\text{s}$.
1. If both nodes experience their **3rd collision** ($m=3$), specify the range of $K$ and list all possible delay times each node can choose.
2. What is the probability that they collide again on their next transmission attempt?

#### 🧮 Governing Formulas
$$K \in \{0, 1, 2, \dots, 2^{\min(m, 10)} - 1\}$$
$$\text{Backoff Delay} = K \times 51.2\ \mu\text{s}$$
$$P(\text{Recollision}) = \frac{1}{\text{Number of Choices in Set}}$$

> [!example]- Click to Reveal Full Step-by-Step Solution
> **1. Set of $K$ Values for $m=3$:**
> $$K \in \{0, 1, 2, \dots, 2^3 - 1\} = \{0, 1, 2, 3, 4, 5, 6, 7\} \quad (8\text{ choices})$$
> 
> **Possible Delay Times ($K \times 51.2\ \mu\text{s}$):**
> - $K=0 \implies \mathbf{0.0\ \mu\text{s}}$
> - $K=1 \implies \mathbf{51.2\ \mu\text{s}}$
> - $K=2 \implies \mathbf{102.4\ \mu\text{s}}$
> - $K=3 \implies \mathbf{153.6\ \mu\text{s}}$
> - $K=4 \implies \mathbf{204.8\ \mu\text{s}}$
> - $K=5 \implies \mathbf{256.0\ \mu\text{s}}$
> - $K=6 \implies \mathbf{307.2\ \mu\text{s}}$
> - $K=7 \implies \mathbf{358.4\ \mu\text{s}}$
> 
> **2. Probability of Recollision:**
> Since both nodes independently pick from 8 equally likely choices:
> $$P(\text{Both pick same } K) = \frac{8}{8 \times 8} = \frac{1}{8} = \mathbf{12.5\%}$$

---

### Problem 3.6: IEEE 802.1Q VLAN Tag 12-bit VID & PCP Bit-Slicing

#### 📝 Problem Statement
A switch receives an 802.1Q tagged frame whose 16-bit TCI field has hex value `0x6064`. 
Extract:
1. The 3-bit Priority (PCP) in decimal.
2. The 1-bit Drop Eligible Indicator (DEI).
3. The 12-bit VLAN Identifier (VID) in decimal.

#### 🧮 Governing Bit-Slicing Mask
```
Bit 15 - 13: PCP (3 bits) | Bit 12: DEI (1 bit) | Bit 11 - 0: VID (12 bits)
```

> [!example]- Click to Reveal Full Step-by-Step Solution
> **Step 1: Convert `0x6064` to 16-bit Binary:**
> $$\text{0x6064} = 0110\ 0000\ 0110\ 0100_2$$
> 
> **Step 2: Extract Sub-Fields:**
> 1. **PCP (Bits 15–13):** $011_2 = \mathbf{3}$ (Priority level 3).
> 2. **DEI (Bit 12):** $0_2 = \mathbf{0}$ (Not drop eligible).
> 3. **VID (Bits 11–0):** $0000\ 0110\ 0100_2 = \text{0x064} = 6 \times 16 + 4 = \mathbf{100} \implies \mathbf{VLAN\ 100}$.

---

# ⚡ Module 4: Physical Layer — Digital Transmission

---

### Problem 4.1: Pulse Code Modulation (PCM) Nyquist Rate & SQNR

#### 📝 Problem Statement
A human telephone speech signal with bandwidth up to $f_{max} = 3.4\text{ kHz}$ is sampled at $f_s = 8000\text{ samples/sec}$ and digitized using $n_b = 8\text{ bits/sample}$.
1. Is the sampling rate sufficient according to the Nyquist theorem?
2. Calculate the resulting digital bit rate $N$ (in kbps).
3. What is the total number of quantization levels $L$?
4. Calculate the Signal-to-Quantization-Noise Ratio ($\text{SNR}_{dB}$).
5. If the voltage range is $[-5\text{V}, +5\text{V}]$, calculate the quantization step size $\Delta$ and the maximum quantization error $|e_q|$.

#### 🧮 Governing Formulas
$$f_{s,\text{min}} = 2 f_{max}, \quad N = f_s \times n_b, \quad L = 2^{n_b}$$
$$\text{SNR}_{dB} = 6.02 n_b + 1.76\text{ dB}, \quad \Delta = \frac{V_{max} - V_{min}}{L}, \quad |e_q| \le \frac{\Delta}{2}$$

> [!example]- Click to Reveal Full Step-by-Step Solution
> **1. Nyquist Rate Check:**
> $$f_{s,\text{min}} = 2 \times 3.4\text{ kHz} = 6.8\text{ kHz} = 6800\text{ samples/s}$$
> Since $f_s = 8000\text{ samples/s} > 6800$, **Yes, Nyquist criterion is satisfied with zero aliasing.**
> 
> **2. Bit Rate ($N$):**
> $$N = 8000\text{ samples/s} \times 8\text{ bits/sample} = 64,000\text{ bps} = \mathbf{64 \text{ kbps}}$$
> 
> **3. Quantization Levels ($L$):**
> $$L = 2^8 = \mathbf{256 \text{ levels}}$$
> 
> **4. Signal-to-Quantization-Noise Ratio ($\text{SNR}_{dB}$):**
> $$\text{SNR}_{dB} = 6.02(8) + 1.76 = 48.16 + 1.76 = \mathbf{49.92 \text{ dB}}$$
> 
> **5. Step Size ($\Delta$) and Maximum Error ($|e_q|$):**
> $$\Delta = \frac{5 - (-5)}{256} = \frac{10\text{V}}{256} \approx \mathbf{0.03906\text{ V} \ (39.06\text{ mV})}$$
> $$|e_q| \le \frac{\Delta}{2} = \frac{39.06\text{ mV}}{2} = \mathbf{19.53\text{ mV}}$$

---

### Problem 4.2: B8ZS Scrambling Pulse Sequence Substitution Trace

#### 📝 Problem Statement
Given the data bitstream:
$$\mathbf{1 \quad 0 \quad 0 \quad 0 \quad 0 \quad 0 \quad 0 \quad 0 \quad 0 \quad 1 \quad 0 \quad 0 \quad 0 \quad 0 \quad 0 \quad 0 \quad 0 \quad 0 \quad 1}$$
Assume that the pulse immediately preceding this sequence was **Negative ($-V$)**.
Determine the complete transmitted voltage pulse sequence using **B8ZS Scrambling**.

#### 🧮 Governing B8ZS Rules
$$\text{8 Consecutive Zeros} \implies \mathbf{000VB0VB}$$
- If preceding pulse was **$+V$** $\implies$ `0 0 0 + - 0 - +`
- If preceding pulse was **$-V$** $\implies$ `0 0 0 - + 0 + -`

> [!example]- Click to Reveal Full Step-by-Step Solution
> 1. Initial bit `1`: Preceding pulse was $-V \implies$ AMI alternates to $\mathbf{+V}$.
> 2. First block of **8 zeros** (`00000000`):
>    - Preceding pulse is $+V$.
>    - Rule for $+V \implies \mathbf{0 \ 0 \ 0 \ + \ - \ 0 \ - \ +}$.
> 3. Middle bit `1`:
>    - Last non-zero pulse was $+V \implies$ AMI alternates to $\mathbf{-V}$.
> 4. Second block of **8 zeros** (`00000000`):
>    - Preceding pulse is $-V$.
>    - Rule for $-V \implies \mathbf{0 \ 0 \ 0 \ - \ + \ 0 \ + \ -}$.
> 5. Final bit `1`:
>    - Last non-zero pulse was $-V \implies$ AMI alternates to $\mathbf{+V}$.
> 
> **Final Transmitted Pulse Sequence:**
> $$\mathbf{+ \quad 0 \ 0 \ 0 \ + \ - \ 0 \ - \ + \quad - \quad 0 \ 0 \ 0 \ - \ + \ 0 \ + \ - \quad +}$$

---

### Problem 4.3: HDB3 Scrambling State & Parity Counter Trace

#### 📝 Problem Statement
Using the same data bitstream from Problem 4.2 (`1 0 0 0 0 0 0 0 0 1 0 0 0 0 1`), assume the preceding pulse was **Positive ($+V$)** and the number of pulses since the last substitution was **0 (Even)**. 
Determine the transmitted sequence using **HDB3 Scrambling**.

#### 🧮 Governing HDB3 Rules
$$\text{4 Consecutive Zeros:}$$
- **Odd Pulse Count Since Last Sub:** $\mathbf{000V}$ (Violate sign of last pulse).
- **Even Pulse Count Since Last Sub:** $\mathbf{B00V}$ ($B$ alternates sign, $V$ copies $B$).

> [!example]- Click to Reveal Full Step-by-Step Solution
> 1. Initial bit `1`: Preceding was $+V \implies \mathbf{-V}$ (Pulse count since start $= 1$, **Odd**).
> 2. First block of 4 zeros (`0000`):
>    - Count $= 1$ (**Odd**), last pulse was $-V$.
>    - Rule: `0 0 0 V` $\implies \mathbf{0 \ 0 \ 0 \ -}$ (Reset count to 0).
> 3. Second block of 4 zeros (`0000`):
>    - Count $= 0$ (**Even**), last pulse was $-V$.
>    - Rule: `B 0 0 V` $\implies \mathbf{+ \ 0 \ 0 \ +}$ (Reset count to 0).
> 4. Middle bit `1`:
>    - Last pulse was $+V \implies$ AMI alternates to $\mathbf{-V}$ (Count $= 1$, **Odd**).
> 5. Third block of 4 zeros (`0000`):
>    - Count $= 1$ (**Odd**), last pulse was $-V$.
>    - Rule: `0 0 0 V` $\implies \mathbf{0 \ 0 \ 0 \ -}$ (Reset count to 0).
> 6. Final bit `1`:
>    - Last pulse was $-V \implies$ AMI alternates to $\mathbf{+V}$.
> 
> **Final Transmitted HDB3 Sequence:**
> $$\mathbf{- \quad 0 \ 0 \ 0 \ - \quad + \ 0 \ 0 \ + \quad - \quad 0 \ 0 \ 0 \ - \quad +}$$

---

### Problem 4.4: 4B/5B Block Coding Overhead & Run-Length Bounds

#### 📝 Problem Statement
A Fast Ethernet 100Base-FX transmitter uses 4B/5B block coding combined with NRZ-I line coding to send data at an effective rate of $R_{\text{data}} = 100\text{ Mbps}$.
1. Calculate the required baud rate (signal rate on the wire).
2. Calculate the exact percentage overhead introduced by 4B/5B.
3. What is the maximum possible number of consecutive zeros that can appear on the wire?

#### 🧮 Governing Formulas
$$\text{Baud Rate} = R_{\text{data}} \times \frac{5}{4}, \quad \text{Overhead \%} = \frac{5 - 4}{4} \times 100\%$$

> [!example]- Click to Reveal Full Step-by-Step Solution
> **1. Signal Rate on Wire (Baud Rate):**
> $$\text{Baud Rate} = 100\text{ Mbps} \times \frac{5}{4} = \mathbf{125 \text{ Mbaud}}$$
> 
> **2. Percentage Overhead:**
> $$\text{Overhead} = \frac{5 - 4}{4} \times 100\% = \frac{1}{4} \times 100\% = \mathbf{25\%}$$
> 
> **3. Maximum Consecutive Zero Bound:**
> By 4B/5B design:
> - No 5-bit code has more than **1 leading zero**.
> - No 5-bit code has more than **2 trailing zeros**.
> - In the worst case (a code ending with 2 zeros followed by a code starting with 1 zero):
>   $$\text{Max Consecutive Zeros} = 2\text{ (trailing)} + 1\text{ (leading)} = \mathbf{3 \text{ zeros}}$$
> *(This guarantees that NRZ-I will never go more than 3 bit intervals without a clock-synchronizing transition).*

---

# 🔍 Module 5: Cross-Layer Signature Math — Packet Hex Dump Arithmetic

---

### Problem 5.1: Raw IPv6 Datagram Header Hex Decoding

#### 📝 Problem Statement
Dissect the following raw hexadecimal byte dump of an IPv6 packet header:
```
60 00 00 00 05 DC 06 40 20 01 0D B8 00 00 00 00 00 00 00 00 00 00 00 01 20 01 0D B8 00 00 00 00 00 00 00 00 00 00 00 02
```
**Questions:**
1. What is the IP Version?
2. What is the Payload Length in decimal bytes?
3. What is the Next Header protocol?
4. What is the Hop Limit?
5. Write the Source and Destination IP addresses in compressed notation.

> [!example]- Click to Reveal Full Step-by-Step Solution
> **1. IP Version:**
> - High nibble of Byte 0 $= \text{`6`} \implies \mathbf{IPv6}$.
> 
> **2. Payload Length (Bytes 4–5):**
> - Bytes 4–5 $= \text{`05 DC`}$.
> - Decimal: $(5 \times 256) + (13 \times 16 + 12) = 1280 + 220 = \mathbf{1500 \text{ Bytes}}$.
> 
> **3. Next Header (Byte 6):**
> - Byte 6 $= \text{`06`} \implies \mathbf{TCP \ (Protocol\ 6)}$.
> 
> **4. Hop Limit (Byte 7):**
> - Byte 7 $= \text{`40`} \implies (4 \times 16 + 0) = \mathbf{64 \text{ Hops}}$.
> 
> **5. Compressed IPv6 Addresses:**
> - Source (Bytes 8–23): `2001:0db8:0000:0000:0000:0000:0000:0001` $\implies \mathbf{2001:db8::1}$
> - Destination (Bytes 24–39): `2001:0db8:0000:0000:0000:0000:0000:0002` $\implies \mathbf{2001:db8::2}$

---

### Problem 5.2: IEEE 802.1Q Tagged Ethernet Frame Hex Dissection

#### 📝 Problem Statement
Dissect the first 20 bytes of an Ethernet frame hex dump:
```
00 1A 2B 3C 4D 5E 00 22 6B 45 1F B1 81 00 E0 14 08 00 45 00
```
**Questions:**
1. What is the Destination MAC address?
2. What is the Source MAC address?
3. What indicates that this frame is VLAN-tagged?
4. What is the Priority (PCP) level (in decimal)?
5. What is the VLAN ID (VID) in decimal?
6. What is the encapsulated Layer 3 protocol?

> [!example]- Click to Reveal Full Step-by-Step Solution
> **1. Destination MAC (Bytes 0–5):** $\mathbf{00:1A:2B:3C:4D:5E}$
> 
> **2. Source MAC (Bytes 6–11):** $\mathbf{00:22:6B:45:1F:B1}$
> 
> **3. 802.1Q Tag Identification:**
> - Bytes 12–13 $= \text{`81 00`} \implies$ **TPID for IEEE 802.1Q**.
> 
> **4. Priority (PCP) & 5. VLAN ID (VID) from TCI (Bytes 14–15):**
> - Bytes 14–15 $= \text{`E0 14`}$.
> - Convert to binary: $1110\ 0000\ 0001\ 0100_2$.
> - **PCP (Bits 15–13):** $111_2 = \mathbf{7 \text{ (Highest Network Control Priority)}}$.
> - **DEI (Bit 12):** $0_2 = \mathbf{0}$.
> - **VID (Bits 11–0):** $0000\ 0001\ 0100_2 = 1 \times 16 + 4 = \mathbf{20} \implies \mathbf{VLAN\ 20}$.
> 
> **6. Encapsulated Layer 3 Protocol (Bytes 16–17):**
> - Bytes 16–17 $= \text{`08 00`} \implies \mathbf{IPv4}$.

---

### Problem 5.3: Nested ICMP Traceroute Error Hex Decoding

#### 📝 Problem Statement
Dissect the following ICMP error packet hex dump returned during a network trace:
```
45 00 00 38 00 00 00 00 40 01 C3 D4 C0 A8 01 01 C0 A8 01 64 03 03 E4 F0 00 00 00 00 45 00 00 28 9F 10 00 00 40 11 ...
```
**Questions:**
1. What is the outer Layer 3 protocol?
2. What are the ICMP Type and Code (Bytes 20–21)? What does this signal to the sender?
3. In the nested inner payload (starting at Byte 28), what was the transport protocol of the original offending packet (Byte 37)?

> [!example]- Click to Reveal Full Step-by-Step Solution
> **1. Outer Protocol (Byte 9):**
> - Byte 9 $= \text{`01`} \implies \mathbf{ICMP \ (Protocol\ 1)}$.
> 
> **2. ICMP Type & Code (Bytes 20–21):**
> - Byte 20 $= \text{`03`}$, Byte 21 $= \text{`03`}$.
> - **Type 3, Code 3:** $\mathbf{Destination\ Port\ Unreachable}$.
> - **Signal:** In Traceroute, this code proves that the packet reached the **final destination host** (because the high-number UDP port was closed), signaling **Trace Complete!**
> 
> **3. Original Offending Packet Protocol (Byte 37):**
> - Inner IP header starts at Byte 28.
> - Inner Protocol field is at Byte $28 + 9 = \text{Byte } 37 = \text{`11`} = 17 \implies \mathbf{UDP}$.

---
#### Navigation
← Return to: [[00 - CSE 4411 Final Exam Master Blueprint & Formula Sheet]]
