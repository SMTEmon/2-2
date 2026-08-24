---
title: "05 - Virtual LANs (VLANs & IEEE 802.1Q)"
course: "CSE 4411"
chapter: 6
section: 6.4
tags:
  - cse4411
  - networking
  - vlans
  - 802-1q
  - trunking
  - final-exam
aliases:
  - Virtual LANs
  - 802.1Q Tagging
---

# 05 - Virtual LANs (VLANs & IEEE 802.1Q)

> [!abstract] Key Takeaway
> **VLANs (Virtual Local Area Networks)** partition a single physical switch infrastructure into multiple isolated **broadcast domains**. 
> Spanning VLANs across multiple switches uses **Trunk Ports** with **IEEE 802.1Q 4-byte header tags**.

---

## 1. Motivation: Why Partition into VLANs?

```
Physical Switch:
[ Port 1 - 8: CS Dept (VLAN 10) ]   <=== ISOLATED ===>   [ Port 9 - 16: EE Dept (VLAN 20) ]
```

1. **Broadcast Containment:** Broadcasts (such as ARP or DHCP) from CS students do not flood EE computers.
2. **Security & Privacy:** Traffic between VLAN 10 and VLAN 20 is isolated at Layer 2. Moving between VLANs requires routing through a **Layer 3 Router (Router-on-a-Stick)** where firewall access control lists (ACLs) can be applied.
3. **Dynamic User Management:** A user moving offices can keep their original subnet by reassigning their switch port VLAN in software.

---

## 2. VLAN Trunking & IEEE 802.1Q Frame Format

When traffic between VLAN-capable switches travels over a single physical link (**Trunk Link**), the sending switch inserts a **4-byte 802.1Q Tag** directly between the Source MAC and EtherType fields:

```
Standard Ethernet Frame:
+--------------+--------------+-------------+------------------+---------+
| Dest MAC (6) |  Src MAC (6) |  Type (2 B) |   Payload Data   | CRC (4) |
+--------------+--------------+-------------+------------------+---------+

802.1Q Tagged Ethernet Frame:
+--------------+--------------+-------------------+-------------+--------------+---------+
| Dest MAC (6) |  Src MAC (6) |  802.1Q Tag (4 B) |  Type (2 B) | Payload Data | CRC (4) |
+--------------+--------------+-------------------+-------------+--------------+---------+
                              |
     +------------------------+-----------------------+
     | TPID: 0x8100 (2 bytes) | Priority (3b) | DEI(1)| VLAN ID (12 bits: 0-4095) |
     +------------------------+-----------------------+--------------------------+
```

### The 802.1Q Header Breakdown

| Sub-Field | Width | Function |
| :--- | :---: | :--- |
| **Tag Protocol Identifier (TPID)** | 16 bits | Fixed value `0x8100` identifying that the frame is 802.1Q tagged. |
| **Priority Code Point (PCP)** | 3 bits | 8 levels of Class of Service (QoS / Voice priority). |
| **Drop Eligible Indicator (DEI)** | 1 bit | If 1, frame can be dropped during network congestion. |
| **VLAN Identifier (VID)** | 12 bits | Identifies the VLAN ($2^{12} = 4096$ possible IDs, $1 - 4094$ usable). |

---

## 3. "Why" Questions & Exam Traps

> [!question] How do hosts in VLAN 10 communicate with hosts in VLAN 20?
> **Answer:**
> Because VLANs create separate Layer 2 broadcast domains, direct Layer 2 switching is impossible. Traffic must be forwarded to a **Layer 3 Router** (via separate physical interfaces or a single 802.1Q trunk port — "Router-on-a-Stick"), which routes the packet between subnets at Layer 3.

---
#### Navigation
← Previous: [[04 - Ethernet Frame & Switched LANs]] | Next: [[06 - Synthesis: A Day in the Life of a Web Request]] →
