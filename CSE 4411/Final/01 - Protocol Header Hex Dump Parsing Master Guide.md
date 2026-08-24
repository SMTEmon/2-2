---
title: "Protocol Header Hex Dump Parsing Master Guide"
course: "CSE 4411"
tags:
  - cse4411
  - networking
  - hex-dump
  - protocol-headers
  - ipv6
  - ethernet
  - icmp
  - exam-prep
aliases:
  - Hex Dump Parsing Guide
  - Packet Header Dissection
---

# Protocol Header Hex Dump Parsing Master Guide

> [!abstract] Executive Summary
> In CSE 4411 examinations, the instructor frequently provides **raw hexadecimal byte dumps** of network packets and asks students to extract fields, verify flags, calculate lengths, and identify protocol anomalies.
> 
> This guide is a complete field manual for manually dissecting **IPv4, IPv6, Ethernet II, IEEE 802.1Q, ICMP, and TCP/UDP** headers under exam conditions with zero errors.

---

## 1. Fast Manual Hex Arithmetic Toolkit

```
Hexadecimal Nibble:   0   1   2   3   4   5   6   7   8   9   A   B   C   D   E   F
Decimal Value:        0   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15
Binary (4 bits):    0000 ...                                                  1111
```

### The Three Golden Rules of Hex Dump Parsing
1. **$1\text{ Byte} = 2\text{ Hex Digits}$ (2 Nibbles):** 
   - A string `45 00 02 40` represents 4 distinct bytes ($32\text{ bits}$).
   - Byte $0 = \text{`45`}$, Byte $1 = \text{`00`}$, Byte $2 = \text{`02`}$, Byte $3 = \text{`40`}$.
2. **Network Byte Order is Strictly Big-Endian:**
   - The Most Significant Byte (MSB) comes first on the wire.
   - For a 16-bit field spanning bytes `0x02 0x40`, the decimal value is:
     $$\text{Decimal} = (2 \times 256) + (4 \times 16 + 0) = 512 + 64 = \mathbf{576}$$
3. **Scaling Multipliers:**
   - **IPv4 Header Length (IHL):** Value $\times \mathbf{4}$ bytes.
   - **IPv4 Fragment Offset:** Value $\times \mathbf{8}$ bytes.
   - **TCP Data Offset:** Value $\times \mathbf{4}$ bytes.

---

## 2. Master Protocol Header Dissection Templates

### Template A: IPv4 Header (20 – 60 Bytes)

```
Byte 0: [ Version (4b) | IHL (4b) ]          Byte 1: [ DSCP / TOS (6b) | ECN (2b) ]
Byte 2-3: [ Total Length (16 bits) ]
Byte 4-5: [ Identification (16 bits) ]
Byte 6-7: [ Flags (3b: Res, DF, MF) | Fragment Offset (13 bits) ]
Byte 8: [ Time to Live - TTL (8 bits) ]      Byte 9: [ Protocol (8 bits) ]
Byte 10-11: [ Header Checksum (16 bits) ]
Byte 12-15: [ Source IP Address (4 bytes) ]
Byte 16-19: [ Destination IP Address (4 bytes) ]
Byte 20+: [ Options & Padding (Optional) ]
```

- **Protocol Numbers:** `0x01` (ICMP - 1), `0x06` (TCP - 6), `0x11` (UDP - 17), `0x29` (IPv6 Tunneling - 41), `0x59` (OSPF - 89).
- **IP Address Classes (First Octet):**
  - **Class A:** `0` – `127` (`0x00` – `0x7F`)
  - **Class B:** `128` – `191` (`0x80` – `0xBF`)
  - **Class C:** `192` – `223` (`0xC0` – `0xDF`)
  - **Class D (Multicast):** `224` – `239` (`0xE0` – `0xEF`)

---

### Template B: IPv6 Header (Fixed 40 Bytes)

```
Byte 0-3: [ Version (4b) | Traffic Class (8b) | Flow Label (20b) ]
Byte 4-5: [ Payload Length (16 bits) - Bytes after 40B base header ]
Byte 6:   [ Next Header (8 bits) - 0x06 (TCP), 0x11 (UDP), 0x3A (ICMPv6), 0x2C (Fragment) ]
Byte 7:   [ Hop Limit (8 bits) ]
Byte 8-23:  [ Source IPv6 Address (16 bytes / 128 bits) ]
Byte 24-39: [ Destination IPv6 Address (16 bytes / 128 bits) ]
```

---

### Template C: Ethernet II & IEEE 802.1Q VLAN Frame

```
Byte 0-5:   [ Destination MAC Address (6 bytes: XX:XX:XX:XX:XX:XX) ]
Byte 6-11:  [ Source MAC Address (6 bytes: XX:XX:XX:XX:XX:XX) ]
Byte 12-13: [ EtherType / Length (2 bytes) ]
              0x0800 -> IPv4
              0x0806 -> ARP
              0x86DD -> IPv6
              0x8100 -> IEEE 802.1Q Tagged Frame
```

#### If Byte 12–13 is `0x8100` (802.1Q Tag):
```
Byte 14-15: [ Priority PCP (3 bits) | DEI (1 bit) | VLAN ID (12 bits: 0-4095) ]
Byte 16-17: [ Encapsulated EtherType (e.g., 0x0800 for IPv4) ]
```

---

### Template D: ICMP Message Header (8 Bytes Base)

```
Byte 0: [ Type (8 bits) ]                     Byte 1: [ Code (8 bits) ]
Byte 2-3: [ Checksum (16 bits) ]
Byte 4-7: [ Header Data (Identifier & Seq # for Ping; Unused/MTU for Error) ]
Byte 8+:  [ Payload: Original IP Header + First 8 Bytes of Offending Datagram ]
```

- **Type/Code Combinations:**
  - `0x08 0x00` $\implies$ Echo Request (Ping)
  - `0x00 0x00` $\implies$ Echo Reply (Ping)
  - `0x0B 0x00` $\implies$ TTL Expired in Transit (Traceroute hop)
  - `0x03 0x03` $\implies$ Destination Port Unreachable (Traceroute destination)
  - `0x03 0x04` $\implies$ Fragmentation Needed & DF Set (PMTUD)

---

### Template E: TCP Segment Header (20 – 60 Bytes)

```
Byte 0-1: [ Source Port (16 bits) ]          Byte 2-3: [ Destination Port (16 bits) ]
Byte 4-7: [ Sequence Number (32 bits) ]
Byte 8-11: [ Acknowledgment Number (32 bits) ]
Byte 12:  [ Data Offset / Header Length (4 bits) | Reserved (4 bits) ]
Byte 13:  [ Flags: CWR | ECE | URG | ACK | PSH | RST | SYN | FIN ]
Byte 14-15: [ Receive Window - rwnd (16 bits) ]
Byte 16-17: [ Checksum (16 bits) ]           Byte 18-19: [ Urgent Pointer (16 bits) ]
```

---

## 3. High-Difficulty Edge Cases & Professor Traps

### 🚨 Edge Case 1: Variable IHL with Options (`IHL > 5`)
- When Byte 0 is `0x46` (IHL = 6) or `0x48` (IHL = 8), the IPv4 header length is:
  $$\text{Header Length} = 6 \times 4 = \mathbf{24\text{ Bytes}} \quad \text{or} \quad 8 \times 4 = \mathbf{32\text{ Bytes}}$$
- **The Trap:** The Layer 4 TCP/UDP payload does **NOT** begin at byte 20; it begins at byte 24 or 32!

---

### 🚨 Edge Case 2: Fragmented Hex Dumps (Flags & Offset Decoding)
Look at **Bytes 6 and 7**:
1. Convert the first hex nibble of Byte 6 to binary (3 bits = Flags, 1 bit = MSB of Offset):
   - `0x40 0x00` $\implies 0100\ 0000 \implies \mathbf{DF = 1, MF = 0, \text{Offset} = 0}$ (Unfragmented datagram).
   - `0x20 0x00` $\implies 0010\ 0000 \implies \mathbf{DF = 0, MF = 1, \text{Offset} = 0}$ (**First Fragment**).
   - `0x20 0xB9` $\implies 0010\ 0000\ 1011\ 1001 \implies \mathbf{DF = 0, MF = 1}$, $\text{Offset} = \text{0x00B9} = 185 \implies 185 \times 8 = \mathbf{1480\text{ Bytes}}$ (**Middle Fragment**).
   - `0x00 0xB9` $\implies 0000\ 0000\ 1011\ 1001 \implies \mathbf{DF = 0, MF = 0}$, $\text{Offset} = 1480\text{ Bytes}$ (**Last Fragment**).

---

### 🚨 Edge Case 3: Ethernet Minimum Frame Size & Padding Discrepancy
- Minimum Ethernet payload is **46 bytes** (Total frame = **64 bytes**).
- **The Scenario:** You receive an Ethernet frame measuring 64 bytes on the wire. Byte 2–3 of the IPv4 header shows `Total Length = 0x0028` ($40\text{ bytes}$).
- **The Rationale:** 
  $$\text{Ethernet Frame} = 14\text{ (Header)} + 40\text{ (IPv4)} + \mathbf{6\text{ (Dummy Padding)}} + 4\text{ (FCS)} = \mathbf{64\text{ Bytes}}$$
  *(The extra 6 trailing zero bytes are Ethernet padding, NOT data).*

---

### 🚨 Edge Case 4: Nested ICMP Payload Dissection (Traceroute Analysis)
When an ICMP error message is returned, bytes 8+ contain the **original IP header + first 8 bytes of the offending transport datagram**:

```
ICMP Frame:
[ Byte 0-7: ICMP Type & Code ] 
  └─► [ Byte 8-27: Inner IPv4 Header of original packet ]
        └─► [ Byte 28-35: First 8 bytes of original UDP/TCP segment ]
              • Bytes 28-29 = Source Port
              • Bytes 30-31 = Destination Port (e.g., 33434)
```

---

### 🚨 Edge Case 5: IPv6 Extension Header Daisy-Chaining
- If Byte 6 (`Next Header`) of the IPv6 base header is **`0x2C` (44)**, the datagram is **Fragmented**!
- The 8-byte Fragment Extension Header immediately follows the 40-byte base header:
  - Byte 40: Next Header pointing to TCP (`0x06`).
  - Byte 42–43: Fragment Offset (13 bits) and M-flag (More fragments).
  - Byte 44–47: 32-bit Identification.

---

### 🚨 Edge Case 6: TCP Flag Bitmasking
Byte 13 of the TCP header contains the flag bits:

| Hex Value | Binary Representation | Active Flags | Semantic Meaning |
| :---: | :---: | :--- | :--- |
| **`0x02`** | `00000010` | **SYN** | Connection Initiation (Handshake Step 1) |
| **`0x12`** | `00010010` | **SYN + ACK** | Connection Acknowledgment (Handshake Step 2) |
| **`0x10`** | `00010000` | **ACK** | Standard Data Packet / Handshake Step 3 |
| **`0x11`** | `00010001` | **FIN + ACK** | Graceful Connection Teardown |
| **`0x14`** | `00010100` | **RST + ACK** | Immediate Connection Reset / Port Closed |

---

## 4. Final Exam Simulation Set (5 Full Problems with Solutions)

### 🧮 Problem 1: IPv6 Datagram Raw Hex Dump
**Hex Dump:**
```
60 00 00 00 04 D2 06 40 20 01 0D B8 00 00 00 00 00 00 00 00 00 00 00 01 20 01 0D B8 00 00 00 00 00 00 00 00 00 00 00 02
```

**Questions:**
1. What is the IP Version?
2. What is the Payload Length (in decimal bytes)?
3. What is the Next Header protocol?
4. What is the Hop Limit?
5. Write the Source and Destination IPv6 addresses in compressed notation.

**Solution:**
1. **Version:** High nibble of Byte 0 $\implies \text{`6`} \implies \mathbf{IPv6}$.
2. **Payload Length:** Bytes 4–5 $= \text{`04 D2`} \implies (4 \times 256) + (13 \times 16 + 2) = 1024 + 210 = \mathbf{1234\text{ Bytes}}$.
3. **Next Header:** Byte 6 $= \text{`06`} \implies \mathbf{TCP\ (Protocol\ 6)}$.
4. **Hop Limit:** Byte 7 $= \text{`40`} \implies (4 \times 16 + 0) = \mathbf{64\text{ Hops}}$.
5. **Addresses:**
   - Source IP (Bytes 8–23): `2001:0db8:0000:0000:0000:0000:0000:0001` $\implies \mathbf{2001:db8::1}$
   - Destination IP (Bytes 24–39): `2001:0db8:0000:0000:0000:0000:0000:0002` $\implies \mathbf{2001:db8::2}$

---

### 🧮 Problem 2: IEEE 802.1Q Tagged Ethernet Frame
**Hex Dump:**
```
00 1A 2B 3C 4D 5E 00 11 22 33 44 55 81 00 A0 64 08 00 45 00 00 3C ...
```

**Questions:**
1. What is the Destination MAC address?
2. Is this frame VLAN-tagged? If so, what is the VLAN ID (VID in decimal)?
3. What is the Priority (PCP) level of this frame?
4. What is the encapsulated Layer 3 protocol?

**Solution:**
1. **Destination MAC:** Bytes 0–5 $= \mathbf{00:1A:2B:3C:4D:5E}$.
2. **VLAN Tag:** Bytes 12–13 $= \text{`81 00`} \implies \mathbf{Yes,\ 802.1Q\ Tagged}$.
   - Tag bytes 14–15 $= \text{`A0 64`} = 1010\ 0000\ 0110\ 0100_2$.
   - Lower 12 bits $(\text{VID}) = 0000\ 0110\ 0100_2 = (1 \times 64) + (2 \times 16) + 4 = 64 + 32 + 4 = \mathbf{100} \implies \mathbf{VLAN\ 100}$.
3. **Priority (PCP):** Upper 3 bits $= 101_2 = \mathbf{5}$ (High Priority Video/Voice).
4. **Encapsulated Protocol:** Bytes 16–17 $= \text{`08 00`} \implies \mathbf{IPv4}$.

---

### 🧮 Problem 3: ICMP Traceroute Reply Analysis
**Hex Dump:**
```
45 00 00 38 1A 2B 00 00 40 01 C3 D4 C0 A8 01 01 C0 A8 01 64 0B 00 E4 F0 00 00 00 00 45 00 00 28 9F 10 00 00 01 11 ...
```

**Questions:**
1. What is the Outer IP Protocol (Byte 9)?
2. What is the ICMP Type and Code? What does this mean?
3. What is the original packet's TTL when it expired?

**Solution:**
1. **Outer Protocol:** Byte 9 $= \text{`01`} \implies \mathbf{ICMP}$.
2. **ICMP Type & Code (Bytes 20–21):**
   - Type $= \text{`0B`} = 11$, Code $= \text{`00`} = 0$.
   - Meaning: **Time-To-Live (TTL) Expired in Transit** (Generated by an intermediate router during Traceroute).
3. **Original Packet TTL:**
   - Nested original IP header begins at Byte 28.
   - Original TTL is at Byte $28 + 8 = \text{Byte } 36 = \text{`01`} \implies \mathbf{TTL = 1}$. *(Router decremented $1 \to 0$ and dropped the packet).*

---

### 🧮 Problem 4: Fragmented IPv4 Datagram Analysis
**Hex Dump:**
```
45 00 02 00 4A 12 20 B9 40 06 8B 1C 0A 00 00 01 0A 00 00 02
```

**Questions:**
1. What is the Total Length of this fragment (in bytes)?
2. Is this the first, middle, or last fragment?
3. What is the starting Byte Offset of this fragment's data in the original unfragmented datagram?

**Solution:**
1. **Total Length:** Bytes 2–3 $= \text{`02 00`} = (2 \times 256) = \mathbf{512\text{ Bytes}}$.
2. **Fragment Position:**
   - Bytes 6–7 $= \text{`20 B9`} = 0010\ 0000\ 1011\ 1001_2$.
   - $\text{MF Flag (3rd bit)} = \mathbf{1}$ (More fragments follow).
   - $\text{Offset (lower 13 bits)} = \text{`0x00B9`} = 185 \ne 0$.
   - Since $\text{MF} = 1$ and $\text{Offset} > 0$, this is a **MIDDLE Fragment**.
3. **Byte Offset:**
   $$\text{Byte Offset} = \text{Offset Field} \times 8 = 185 \times 8 = \mathbf{1480\text{ Bytes}}$$

---

### 🧮 Problem 5: DHCP Offer Packet Encapsulation
**Hex Dump:**
```
FF FF FF FF FF FF 00 22 6B 45 1F B1 08 00 45 00 01 48 00 00 40 00 40 11 3A 20 DF 01 02 05 FF FF FF FF 00 43 00 44 01 34 00 00 02 ...
```

**Questions:**
1. What is the Destination MAC address?
2. What is the Destination IP address?
3. What are the Source and Destination UDP Ports? What protocol does this represent?

**Solution:**
1. **Destination MAC:** Bytes 0–5 $= \text{`FF FF FF FF FF FF`} \implies \mathbf{Ethernet\ Broadcast}$.
2. **Destination IP:** Bytes 26–29 (Bytes 12–15 of IPv4 header) $= \text{`FF FF FF FF`} \implies \mathbf{255.255.255.255\ (IP\ Broadcast)}$.
3. **UDP Ports (Bytes 30–33):**
   - Source Port: Bytes 30–31 $= \text{`00 43`} = 4 \times 16 + 3 = \mathbf{67\ (DHCP\ Server)}$.
   - Destination Port: Bytes 32–33 $= \text{`00 44`} = 4 \times 16 + 4 = \mathbf{68\ (DHCP\ Client)}$.
   - Protocol: **DHCP (Dynamic Host Configuration Protocol)**.

---
#### Navigation
← Return to: [[00 - CSE 4411 Final Exam Master Blueprint & Formula Sheet]]
