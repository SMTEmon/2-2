---
tags: [networking/chapter4, practice-problems, quiz3]
source: "Kurose & Ross, 8th ed. — Chapter 4 Review Questions R17-R31, Problems P8-P20"
---

# Practice Problems: IP Datagram → IPv6 (Quiz 3 scope)

Up → [[00 - Index]] · Related → [[12 - Book Extras Beyond the Slides (IP to IPv6)]]

> [!abstract] Scope
> Every book Review Question / Problem below is tagged to the section it tests
> (§4.3.1 datagram format, §4.3.2 addressing/DHCP, §4.3.3 NAT, §4.3.4 IPv6) —
> the exact quiz range. Questions from §4.1/§4.2 (R1–R16) and §4.4 SDN (R32–R35,
> P21–P25) are **excluded** on purpose.
>
> Problems are grouped by **type**, not by number, since the point is to recognize
> the *pattern* each question is testing, not memorize a specific number.

## 🗂️ Problem-type map

| Type | What it tests | Book Qs |
|---|---|---|
| A. Terminology / short-answer | Definitions, "what is X" | R17–R21, R27–R31 |
| B. Header-overhead arithmetic | % overhead, datagram counts | R25, P17 |
| C. Binary ⇄ dotted-decimal | Address notation | R22 |
| D. Longest-prefix-matching tables | Build/read a forwarding table | P8, P9, P10 |
| E. Subnet design / CIDR carving | Allocate address blocks to meet size constraints | P11, P12, P13, P14, P15 |
| F. NAT translation table | Fill in a NAT table from a scenario | P18 |
| G. DHCP scenarios | Apply DHCP concepts to a setup | R23, R26 |
| H. NAT/IPv6 discussion & security | Open-ended reasoning | R24, R29, R30, R31, P19, P20 |
| I. Live-lookup exercise | Requires an actual internet query | P16 |

---

## A. Terminology / short-answer (R17–R21, R27–R31)

> [!example] R17 — How does Host B know to pass the payload to TCP vs. UDP?
> The **protocol field** in the IP header (value 6 = TCP, 17 = UDP) tells the receiving
> host's network layer which upper-layer protocol should get the datagram's payload.

> [!example] R18 — Which field bounds a datagram to ≤ N router hops?
> **TTL (Time To Live)** — decremented by each router; datagram dropped at TTL = 0.

> [!example] R19 — Do the IP and TCP/UDP checksums cover common bytes?
> **No.** The IP header checksum covers only the **IP header**. The TCP/UDP checksum
> covers the **TCP/UDP header + data** (the IP payload). They protect disjoint byte
> ranges — see [[12 - Book Extras Beyond the Slides (IP to IPv6)]] for why both exist.

> [!example] R20 — Where are fragments reassembled?
> **Only at the final destination host** — never at an intermediate router (IPv4). IPv6
> doesn't fragment in-network at all; see [[09 - IPv6 & Tunneling]].

> [!example] R21 — Do routers have IP addresses? How many?
> Yes — a router has **one IP address per interface** (one per attached link), not a
> single address for the whole device.

> [!example] R27 — What is route aggregation, and why is it useful?
> Advertising **one short prefix** to summarize many smaller, contiguous address blocks
> underneath it (e.g. an ISP advertising `/20` for eight `/23` customer blocks). Useful
> because it keeps global routing tables from growing with every individual
> organization's address block — see [[06 - IP Addressing, Subnets & CIDR]].

> [!example] R28 — What's a "plug-and-play" / "zeroconf" protocol?
> A protocol that configures a device's network settings **automatically**, with no
> manual admin intervention — DHCP is the canonical example.

> [!example] R29 — What is a private network address? Can it appear on the public Internet?
> An address from the **RFC 1918** reserved ranges (`10.0.0.0/8`, `172.16.0.0/12`,
> `192.168.0.0/16`) — meaningful only *inside* a local network. It should **never**
> appear as a source/destination address on the public Internet; a NAT device
> translates it to a public address before packets leave the private network.

> [!example] R30 — Compare IPv4 and IPv6 header fields
> Common to both: version, a "type of service"/traffic-class field, a length field, a
> hop-limiting field (TTL / hop limit), a next-protocol field, source & destination
> address. **IPv4-only**: header length, identification/flags/fragmentation offset,
> header checksum, options. **IPv6-only**: flow label. See the side-by-side table in
> [[09 - IPv6 & Tunneling]].

> [!example] R31 — Does IPv6 treat the IPv4 tunnel as a link-layer protocol?
> **Agree.** From the IPv6 datagram's perspective, the IPv4 network it tunnels through
> is just "the next hop's link layer" — IPv6 has no idea it's riding inside an IPv4
> datagram; it only sees that it handed its datagram off to whatever "link" connects it
> to the next IPv6-aware node. That's precisely the encapsulation abstraction.

---

## B. Header-overhead arithmetic

> [!example] R25 — 40-byte app chunks every 20 ms, encapsulated TCP-over-IP: overhead %?
> Header overhead = 20 bytes (IP) + 20 bytes (TCP) = 40 bytes.
> Total datagram size = 40 (headers) + 40 (app data) = 80 bytes.
> $$
> \%\,\text{overhead} = \frac{40}{80} = 50\% \qquad \%\,\text{data} = \frac{40}{80} = 50\%
> $$

> [!example] P17 — 1500-byte datagrams (20-byte IP header), sending a 5,000,000-byte MP3: how many datagrams?
> Usable payload per datagram: $1500 - 20 = 1480$ bytes.
> $$
> \#\text{datagrams} = \left\lceil \frac{5{,}000{,}000}{1480} \right\rceil = \lceil 3378.38 \rceil = \boxed{3379}
> $$
> (The last datagram is only partially full: $5{,}000{,}000 - 3378 \times 1480 = 560$ bytes of data.)

---

## C. Binary ⇄ dotted-decimal

> [!example] R22 — 32-bit binary form of 223.1.3.27?
> $$
> 223.1.3.27 = 11011111\ 00000001\ 00000011\ 00011011
> $$
> Method: convert each octet independently to 8-bit binary. $223 = 11011111$,
> $1 = 00000001$, $3 = 00000011$, $27 = 00011011$.

---

## D. Longest-prefix-matching tables (fully worked)

> [!info] General method
> 1. For each interface's given address **range**, find the length of the address's
>    common leading bits (or decompose the range into the minimal set of CIDR blocks
>    if it isn't a clean power-of-two-aligned block).
> 2. Build a table of `(prefix, interface)` pairs plus a default/"otherwise" row.
> 3. To classify a new address: check it against **every** prefix, and pick the
>    interface belonging to the **longest matching prefix**. If nothing matches, use
>    the default.

### P9 — 8-bit addresses, table already given

| Prefix | Interface | Address range | Count |
|---|---|---|---|
| `00` | 0 | 0 – 63 | 64 |
| `010` | 1 | 64 – 95 | 32 |
| `011` | 2 | 96 – 127 | 32 |
| `10` | 2 | 128 – 191 | 64 |
| `11` | 3 | 192 – 255 | 64 |

(Interface 2 owns **two** disjoint prefixes — `011` and `10` — which happen to be
contiguous as address *ranges* (96–191), covering 96 addresses in total.)

### P10 — 8-bit addresses, table already given

| Prefix | Interface | Address range | Count |
|---|---|---|---|
| `110` | 0 | 192 – 223 | 32 |
| `10` | 1 | 128 – 191 | 64 |
| `111` | 2 | 224 – 255 | 32 |
| otherwise | 3 | 0 – 127 | 128 |

> [!tip] Why interface 0 needs `110`, not just `1`
> The table you're given lists interface 0 as matching prefix `1` — but that overlaps
> with `10` (interface 1) and `111` (interface 2), both *longer* prefixes. By longest
> prefix matching, interface 0 only actually "wins" for addresses starting `110` — i.e.
> the leftover slice of the `1`-space not already claimed by a longer, more specific
> prefix. This is the single most important intuition for this problem type: **a
> shorter prefix only governs the addresses that no longer prefix already claims.**

### P8 — 32-bit addresses, four interfaces (build the table yourself)

Given ranges:

| Interface | Range (binary) |
|---|---|
| 0 | `11100000 00000000...` – `11100000 00111111...` |
| 1 | `11100000 01000000...` – `11100000 01000000 11111111 11111111` |
| 2 | `11100000 01000001...` – `11100001 01111111...` |
| 3 | otherwise |

**a. Longest-prefix table** (find common-prefix length for each range):

| Interface | Prefix | CIDR form |
|---|---|---|
| 0 | `11100000 00` (10 bits) | `224.0.0.0/10` |
| 1 | `11100000 01000000` (16 bits) | `224.64.0.0/16` |
| 2 | `1110000` (7 bits) | `224.0.0.0/7` |
| 3 | otherwise | default |

Interface 2's block (`/7`) is *broader* than interface 0/1's blocks, but that's fine —
longest-prefix matching always prefers the more specific (`/10` or `/16`) match over
the looser `/7` whenever an address falls inside both.

**b. Classify the three test addresses:**

| Address | Binary check | Longest match | → Interface |
|---|---|---|---|
| `11001000 10010001 01010001 01010101` = 200.145.81.85 | doesn't start `1110000` | none | **3** (otherwise) |
| `11100001 01000000 11000011 00111100` = 225.64.195.60 | starts `1110000`, not `11100000 00`/`11100000 01000000` | `/7` only | **2** |
| `11100001 10000000 00010001 01110111` = 225.128.17.119 | starts `1110000` | `/7` only | **2** |

---

## E. Subnet design / CIDR carving

> [!example] P11 — Fit three subnets (≥60, ≥90, ≥12 interfaces) inside `223.1.17.0/24`
> Method: figure out the smallest power-of-two block that fits each requirement
> (remember: 2 addresses per subnet are unusable — network ID and broadcast).
>
> | Need | Smallest block | Usable addresses |
> |---|---|---|
> | ≥ 90 | `/25` (128 addrs) | 126 |
> | ≥ 60 | `/26` (64 addrs) | 62 |
> | ≥ 12 | `/28` (16 addrs) | 14 |
>
> A valid, non-overlapping carve-up of `223.1.17.0/24` (256 addresses total):
>
> | Subnet | Block |
> |---|---|
> | Needs ≥ 90 | `223.1.17.0/25` (.0 – .127) |
> | Needs ≥ 60 | `223.1.17.128/26` (.128 – .191) |
> | Needs ≥ 12 | `223.1.17.192/28` (.192 – .207) |
> | *(spare, unused)* | `223.1.17.208/28` – `.255` |

> [!example] P14 — Example host address in `128.119.40.128/26`; then split `128.119.40.64/26` into 4 equal subnets
> - `/26` spans `128.119.40.128` – `128.119.40.191` (network ID `.128`, broadcast `.191`).
>   Any address strictly between works, e.g. **`128.119.40.130`**.
> - Splitting a `/26` (64 addresses) into **4 equal** pieces → each piece is `/28` (16 addresses each):
>
> | Subnet | Block |
> |---|---|
> | 1 | `128.119.40.64/28` |
> | 2 | `128.119.40.80/28` |
> | 3 | `128.119.40.96/28` |
> | 4 | `128.119.40.112/28` |

> [!tip] P12 & P13 — "Rewrite in a.b.c.d/x notation"
> These aren't new problems — they just ask you to re-express the binary-prefix tables
> from [[02 - Router Architecture & Switching Fabrics]] (the original longest-prefix
> example) and from P8 above, using dotted-decimal CIDR notation instead of raw bit
> strings. Practice: convert `11001000 00010111 00010***` → `200.23.16.0/21`, etc.
> (Take each prefix, pad the remaining bits with 0, convert to dotted decimal, append `/`+bit-count.)

> [!question] P15 — Multi-router topology carve-up (self-practice)
> Given a `/23` block and a topology with 3 host-bearing subnets (needing 250, 120, 120
> interfaces) plus 3 router-to-router link subnets (2 interfaces each): apply the same
> method as P11 — largest requirement gets the largest block first, then work down —
> and then build a longest-prefix forwarding table for each router. This is genuinely a
> "do it yourself" design exercise; the technique is identical to P11, just with six
> subnets instead of three and an extra forwarding-table step.

---

## F. NAT translation table

> [!example] P18 — NAT scenario: router WAN address `24.34.112.235`, home net `192.168.1.0/24`
> **a. Assign home-network addresses**: give the router's LAN-side interface one
> address (e.g. `192.168.1.1`) and each host a distinct address from the same block
> (e.g. `192.168.1.2`, `192.168.1.3`, `192.168.1.4`, …) — any valid host addresses in
> `192.168.1.0/24` work, as long as they're unique.
>
> **b. NAT table, two connections per host, all to `128.119.40.86:80`**: each
> connection needs a *distinct* NAT-assigned WAN-side port, since the WAN-side IP
> (`24.34.112.235`) is shared. Template (fill in your own chosen private ports/hosts):
>
> | WAN side | LAN side |
> |---|---|
> | `24.34.112.235, 5001` | `192.168.1.2, 3345` |
> | `24.34.112.235, 5002` | `192.168.1.2, 3346` |
> | `24.34.112.235, 5003` | `192.168.1.3, 3345` |
> | `24.34.112.235, 5004` | `192.168.1.3, 3346` |
> | `24.34.112.235, 5005` | `192.168.1.4, 3345` |
> | `24.34.112.235, 5006` | `192.168.1.4, 3346` |
>
> Key idea being tested: the NAT **WAN-side port number**, not the IP, is what
> disambiguates which internal host/connection a reply belongs to.

---

## G. DHCP scenarios

> [!example] R23 — Find your own DHCP-assigned settings
> This is a hands-on exercise: on your own machine run `ipconfig /all` (Windows) or
> `ifconfig`/`ip addr` + check your router's DHCP leases (Linux/Mac), and record: your
> IP address, subnet mask, default gateway (first-hop router), and DNS server address.

> [!example] R26 — Home wireless router + 5 PCs: how are addresses assigned? Is NAT used?
> The ISP gives the wireless router **one** public IP via DHCP. The router then runs
> **its own internal DHCP server**, handing out private addresses (e.g. `192.168.1.x`)
> to the 5 PCs over 802.11. **Yes, NAT is used** — otherwise the 5 PCs, with only
> private addresses, couldn't reach the public Internet at all (their addresses aren't
> globally routable), and the ISP only gave out one public address to share.

---

## H. Discussion / open-ended reasoning

> [!example] R24 — 3 routers between source and destination: interfaces crossed / tables indexed?
> A datagram crosses **4 links total** → 4 link interfaces (source host → R1 → R2 → R3
> → destination host means it traverses the outgoing interface of the source, then in
> and out of each of the 3 routers... concretely: **each of the 3 routers indexes its
> forwarding table exactly once**, so **3 forwarding-table lookups** occur en route.

> [!example] P19 — Detecting host count behind a NAT via the IP identification field
> **a.** Since each host stamps its own **sequentially increasing** ID field
> independently, an observer sniffing all NAT-outbound traffic can look for **distinct
> increasing subsequences** interleaved in the observed ID stream — each such
> subsequence corresponds to one internal host. Counting the number of concurrent
> increasing runs estimates the host count.
> **b.** If IDs were assigned **randomly** instead, this technique **fails** — there's no
> per-host structure left to detect; random IDs from different hosts are
> indistinguishable from random IDs from the same host.

> [!example] P20 — P2P connection between two NAT'd peers (Arnold & Bernard)
> Direct connection is impossible (neither NAT will admit an unsolicited inbound SYN).
> The standard technique is **NAT hole punching via a rendezvous server**: both Arnold
> and Bernard make *outbound* connections to a well-known public server first (which
> NATs always allow). That server learns each peer's translated public `IP:port` and
> shares it with the other peer. Both peers then simultaneously send packets to each
> other's translated address — each outbound packet "punches a hole" in its own NAT
> that the other peer's incoming packet can then pass through. (This is the idea behind
> **STUN/TURN/ICE**, referenced in [[12 - Book Extras Beyond the Slides (IP to IPv6)]].)

---

## I. Live-lookup exercise

> [!question] P16 — ARIN whois + MaxMind geolocation
> Can't be pre-answered generically — it requires querying `https://www.arin.net/whois`
> for three real universities' address blocks, then cross-checking geolocation via
> `https://www.maxmind.com`. **Note for your own understanding**: whois reliably tells
> you *who owns* a block and often the registrant's official address, but IP
> geolocation (MaxMind-style) is a **best-effort estimate**, not a guarantee — a block
> registered to a university's central admin office doesn't guarantee every address in
> it is physically used on that campus.

---
#### Navigation
◀ [[12 - Book Extras Beyond the Slides (IP to IPv6)]] · ▲ [[00 - Index]]
