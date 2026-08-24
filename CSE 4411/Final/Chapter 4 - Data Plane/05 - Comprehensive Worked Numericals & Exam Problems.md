---
title: "05 - Comprehensive Worked Numericals & Exam Problems"
course: "CSE 4411"
chapter: 4
tags:
  - cse4411
  - networking
  - practice-problems
  - worked-numericals
  - exam-prep
  - dhcp
  - nat
  - ipv6
aliases:
  - Chapter 4 Practice Problems
  - DHCP NAT IPv6 Numericals
---

# 05 - Comprehensive Worked Numericals & Exam Problems (DHCP to IPv6)

> [!abstract] Exam Objective
> This problem set provides step-by-step mathematical solutions for **NAT Translation Table state mapping**, **DHCP DORA parameter extraction**, and **IPv6-in-IPv4 Tunneling byte overhead**.

---

## 🧮 Problem Set 1: NAT Translation Table Mapping

### Problem Statement
A home router with public IP `198.51.100.25` connects a local network (`192.168.1.0/24`) to the Internet. 
1. Host `192.168.1.10` opens a web session (`Src Port: 4001`) to `www.google.com` (`142.250.190.46:80`).
2. Host `192.168.1.20` opens an HTTPS session (`Src Port: 4001`) to `www.wikipedia.org` (`198.35.26.96:443`).
3. Host `192.168.1.10` opens a DNS query (`Src Port: 5353`) to `8.8.8.8:53`.

**Question:** Construct the NAT Translation Table and specify the exact packet headers (Source IP, Source Port, Destination IP, Destination Port) as they travel:
- Across the Home LAN
- Across the Public WAN

---

### Step-by-Step Solution

#### The NAT Translation Table

| WAN (Public) Side Entry | LAN (Private) Side Entry | Transport Protocol |
| :---: | :---: | :---: |
| `198.51.100.25 : 50001` | `192.168.1.10 : 4001` | TCP |
| `198.51.100.25 : 50002` | `192.168.1.20 : 4001` | TCP |
| `198.51.100.25 : 50003` | `192.168.1.10 : 5353` | UDP |

#### Outbound Wire Headers

| Session | On Local LAN (Before NAT) | On Public WAN (After NAT) |
| :---: | :--- | :--- |
| **Google Web** | `Src: 192.168.1.10:4001 -> Dst: 142.250.190.46:80` | `Src: 198.51.100.25:50001 -> Dst: 142.250.190.46:80` |
| **Wikipedia** | `Src: 192.168.1.20:4001 -> Dst: 198.35.26.96:443` | `Src: 198.51.100.25:50002 -> Dst: 198.35.26.96:443` |
| **Google DNS** | `Src: 192.168.1.10:5353 -> Dst: 8.8.8.8:53` | `Src: 198.51.100.25:50003 -> Dst: 8.8.8.8:53` |

---

## 🧮 Problem Set 2: IPv6 Tunneling Byte Overhead Calculation

### Problem Statement
An IPv6 host transmits a 1200-byte TCP data payload. 
- IPv6 Base Header size $= 40\text{ bytes}$.
- TCP Header size $= 20\text{ bytes}$.
- Outer IPv4 Tunnel Header size $= 20\text{ bytes}$.

**Calculate:**
1. Total Length of the native IPv6 packet.
2. Value of the `Payload Length` field in the IPv6 header.
3. Total Length of the encapsulated IPv4 tunnel packet on the wire.
4. Total protocol header overhead percentage across the tunnel link.

---

### Step-by-Step Solution

1. **Native IPv6 Packet Total Size:**
   $$\text{IPv6 Packet Size} = 40\text{ (IPv6)} + 20\text{ (TCP)} + 1200\text{ (Data)} = \mathbf{1260\text{ Bytes}}$$

2. **IPv6 Header `Payload Length` Field:**
   $$\text{Payload Length} = 20\text{ (TCP)} + 1200\text{ (Data)} = \mathbf{1220\text{ Bytes}}$$
   *(IPv6 Payload Length excludes the 40-byte base header!)*

3. **Encapsulated IPv4 Tunnel Packet Total Size:**
   $$\text{IPv4 Outer Packet Size} = 20\text{ (IPv4)} + 1260\text{ (IPv6 Packet)} = \mathbf{1280\text{ Bytes}}$$

4. **Protocol Header Overhead Percentage:**
   $$\text{Total Header Bytes} = 20\text{ (IPv4)} + 40\text{ (IPv6)} + 20\text{ (TCP)} = 80\text{ Bytes}$$
   $$\text{Overhead \%} = \frac{80}{1280} \times 100\% = \mathbf{6.25\%}$$

---
#### Navigation
← Previous: [[04 - Book Extras & Professor Traps]] | Next: [[00 - Index]] (Chapter 5) →
