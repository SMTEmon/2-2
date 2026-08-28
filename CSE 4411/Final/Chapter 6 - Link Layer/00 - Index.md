---
title: "Chapter 6: Link Layer and LANs"
course: "CSE 4411"
chapter: 6
tags:
  - cse4411
  - networking
  - link-layer
  - lan
  - mac
  - ethernet
  - final-exam
aliases:
  - Link Layer and LANs
  - Chapter 6 Final Scope
---

# Chapter 6: Link Layer and LANs

> [!abstract] Final Exam Roadmap (Official Scope)
> The **Link Layer** manages the physical transfer of frames across a single communication link between adjacent network nodes. 
> 
> This vault covers error detection (**CRC-32 Modulo-2 polynomial division**), Multiple Access Control (**ALOHA efficiency derivations, CSMA/CD, Binary Exponential Backoff**), **MAC addressing and ARP**, switched Ethernet networks with **self-learning filtering**, **VLANs (802.1Q)**, and the complete end-to-end synthesis: **"A Day in the Life of a Web Request."** *(Note: MPLS and Datacenter Networking are excluded from the Final Exam syllabus).*

---

## ✅ Chapter 6 Study Progress Checklist
- [ ] [[01 - Link Layer Fundamentals & Error Detection]] — 2D Parity, CRC Modulo-2 Arithmetic ($D \cdot 2^r / G$)
- [ ] [[02 - Multiple Access Protocols (Partitioning, ALOHA, CSMA-CD)]] — Slotted ALOHA ($36.8\%$), Pure ALOHA ($18.4\%$), CSMA/CD $L_{min}$
- [ ] [[03 - Link Layer Addressing & ARP]] — 48-bit MACs, ARP 4-Way Flow, Cross-Subnet Forwarding
- [ ] [[04 - Ethernet Frame & Switched LANs]] — 802.3 Header, Self-Learning Switches, Filtering/Forwarding
- [ ] [[05 - Virtual LANs (VLANs & IEEE 802.1Q)]] — Port Isolation, 802.1Q 4-Byte Tag, Trunking
- [ ] [[06 - Synthesis - A Day in the Life of a Web Request]] — 5-Layer End-to-End Flow (DHCP $\to$ ARP $\to$ DNS $\to$ TCP $\to$ HTTP)
- [ ] [[07 - Book Extras & Professor Traps]] — Cut-Through vs Store-and-Forward, CRC Burst Bounds
- [ ] [[08 - Comprehensive Worked Numericals & Exam Problems]] — CRC Long Division, CSMA/CD Backoff Slots

---

## 🗺️ Master Visual Navigation Map

```mermaid
flowchart TD
    Ch6["Chapter 6: Final Exam Scope<br>(Link Layer and LANs)"]
    
    Ch6 --> Sec1["[[01 - Link Layer Fundamentals & Error Detection]]<br>Framing, Parity, Checksum & CRC Modulo-2 Math"]
    Ch6 --> Sec2["[[02 - Multiple Access Protocols (Partitioning, ALOHA, CSMA-CD)]]<br>TDMA/FDMA, Slotted/Pure ALOHA Derivations & CSMA/CD Math"]
    Ch6 --> Sec3["[[03 - Link Layer Addressing & ARP]]<br>MAC vs IP, ARP 4-Way Flow & Cross-Subnet Forwarding"]
    Ch6 --> Sec4["[[04 - Ethernet Frame & Switched LANs]]<br>802.3 Frame Layout, Star Topology & Self-Learning Switches"]
    Ch6 --> Sec5["[[05 - Virtual LANs (VLANs & IEEE 802.1Q)]]<br>Port Isolation, 802.1Q 4-Byte Tagging & Trunking"]
    Ch6 --> Sec6["[[06 - Synthesis - A Day in the Life of a Web Request]]<br>Full 5-Layer End-to-End Protocol Flow"]
    Ch6 --> Sec7["[[07 - Book Extras & Professor Traps]]<br>Cut-Through Switching & CRC Error Bounds"]
    Ch6 --> Sec8["[[08 - Comprehensive Worked Numericals & Exam Problems]]<br>CRC Long Division, Efficiency Math & Switch Tables"]
```

---

## 📑 Detailed Note Registry

| # | Note Document | Core Question Answered | High-Yield Topics |
| :---: | :--- | :--- | :--- |
| **01** | [[01 - Link Layer Fundamentals & Error Detection]] | *How do nodes mathematically verify that frames arrived uncorrupted?* | Link Layer Services, 2D Parity, CRC Polynomial Division ($D \cdot 2^r / G$) |
| **02** | [[02 - Multiple Access Protocols (Partitioning, ALOHA, CSMA-CD)]] | *How do multiple nodes share a broadcast wire without collision chaos?* | TDMA/FDMA, Slotted ALOHA ($36.8\%$), Pure ALOHA ($18.4\%$), CSMA/CD $L_{min}$ derivation |
| **03** | [[03 - Link Layer Addressing & ARP]] | *How is a layer 3 IP address mapped to physical layer 2 hardware?* | 48-bit MAC Addresses, ARP Broadcast Query/Unicast Reply, Cross-Subnet Rewriting |
| **04** | [[04 - Ethernet Frame & Switched LANs]] | *How do Ethernet switches learn network topology without configuration?* | 802.3 Ethernet Header (Preamble, SFD, FCS), Self-Learning Algorithm, Filtering/Forwarding |
| **05** | [[05 - Virtual LANs (VLANs & IEEE 802.1Q)]] | *How do network engineers isolate broadcast domains across switches?* | Port-based VLANs, 802.1Q 4-byte Tag Format, VLAN Trunking |
| **06** | [[06 - Synthesis - A Day in the Life of a Web Request]] | *How do all 5 layers work together when you open a browser?* | DHCP $\to$ ARP $\to$ DNS $\to$ TCP Handshake $\to$ HTTP GET |
| **07** | [[07 - Book Extras & Professor Traps]] | *What subtle edge cases appear on Link Layer exam questions?* | Store-and-Forward vs Cut-Through switching, CRC burst detection bounds |
| **08** | [[08 - Comprehensive Worked Numericals & Exam Problems]] | *How do you solve CRC and MAC efficiency problems step-by-step?* | Full CRC polynomial divisions, CSMA/CD backoff slot wait calculations |

---
#### Navigation
Next → [[01 - Link Layer Fundamentals & Error Detection]]
