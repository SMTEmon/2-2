---
tags: [networking/chapter4, book-vs-slides, quiz3]
source: "Kurose & Ross, Computer Networking: A Top-Down Approach, 8th ed. — §4.3 (pp. 330-353)"
---

# Book Extras: What's in Chapter 4 §4.3 but NOT in the Slides

Up → [[00 - Index]] · Scope note → [[13 - Practice Problems, IP Datagram to IPv6]]

> [!abstract] Quiz 3 scope reminder
> Quiz 3 syllabus = "IP datagram to IPv6, before SDN" = book **§4.3** in full
> (§4.3.1 IPv4 Datagram Format → §4.3.2 IPv4 Addressing (incl. DHCP) → §4.3.3 NAT →
> §4.3.4 IPv6), stopping before §4.4 (Generalized Forwarding and SDN). This note
> covers everything in that range that the slide deck (`Chapter_4_v8.2.pptx`)
> **compresses, skips, or never mentions at all.**

## 🔍 Brief report: is there anything genuinely new?

> [!success] Yes — five things stand out
> 1. **DHCP's mobility shortcoming** — a host loses its ongoing TCP connections every
>    time it gets a new DHCP address on a new subnet. Not mentioned on the slides at all,
>    and a very natural short-answer quiz question.
> 2. **Classful addressing history** (Class A/B/C, why it was replaced by CIDR) — the
>    slides jump straight to CIDR; the book explains *why* CIDR was needed with a
>    concrete "2,000 hosts wasted a /16" example.
> 3. **A full "Focus on Security" sidebar on Firewalls and Intrusion Detection/Prevention
>    Systems** — this topic does not appear on the slides in this chapter at all (it's
>    only foreshadowed, and properly covered in Ch. 8).
> 4. **NAT's own numeric/architectural detail** — the 60,000-simultaneous-connection
>    capacity argument, NAT traversal (STUN, RFC 5389), and the "router itself gets its
>    WAN address via DHCP, and runs its own DHCP server on the LAN side" double-duty
>    point are all book-only.
> 5. **IPv6's historical/trivia layer** — the 2008-vs-2018 exhaustion-date debate, what
>    happened to "IPv5," the **anycast** address type, and the "flag day" framing for why
>    a synchronized IPv4→IPv6 switchover was never realistic.
>
> Everything else in §4.3 is the *same material* as the slides, just written out in full
> prose with more justification ("why," not just "what").

---

## §4.3.1 — IPv4 Datagram Format: book-only detail

> [!info] Why checksum at *both* transport and network layers?
> The book explicitly asks and answers this — a natural exam question:
> 1. The **IP header checksum** covers only the IP header; the **TCP/UDP checksum**
>    covers the *entire* TCP/UDP segment (header + data). They protect different scopes.
> 2. TCP/UDP and IP aren't necessarily glued together — TCP could in principle run over
>    a non-IP network layer (e.g. ATM), and IP can carry payloads that never reach
>    TCP/UDP at all. Each layer must be able to verify its own integrity independently.

> [!info] Why does the header checksum need recomputing at every router?
> Because **TTL** (and possibly **options**) changes at every hop — any field change
> invalidates the old checksum, so it must be recomputed hop-by-hop. (IPv6 sidesteps
> this entirely by dropping the header checksum — see §4.3.4 below.)

> [!note] The "protocol" field is the network↔transport glue
> The book draws an explicit analogy: the **protocol field** (e.g. 6 = TCP, 17 = UDP) is
> to the network/transport boundary what the **port number** is to the transport/application
> boundary, and what a link-layer frame's type field is to the link/network boundary.
> Three layers, three demultiplexing keys.

> [!warning] The book does NOT actually teach fragmentation math in the main text!
> Direct quote: *"We'll not cover fragmentation here; but readers can find a detailed
> discussion online, among the 'retired' material from earlier versions of this book."*
> The 8th-edition authors consider IP fragmentation a legacy topic (since IPv6 dropped
> it entirely). **Your slides still cover it in detail** (the 4000-byte / MTU 1500 worked
> example) — see [[05 - IP Datagram Format & Fragmentation]] — so for the quiz, treat
> the **slide version** as the authoritative source for any fragmentation-offset math,
> since the book has deliberately moved on from it.

> [!tip] Fun framing the book uses
> The book calls the payload field the datagram's "raison d'être" — everything else in
> the header exists to get that field delivered correctly.

## §4.3.2 — IPv4 Addressing: book-only detail

> [!info] Interface, precisely
> "An IP address is technically associated with an **interface**, rather than with the
> host or router containing that interface." A host usually has 1–2 interfaces
> (wired + wireless); a router always has ≥ 2 (that's what makes it a router).

### Classful addressing (pre-CIDR history)

> [!example] Why CIDR replaced classful addressing
> Before CIDR, the network portion of an address was constrained to exactly **8, 16, or
> 24 bits** — Class A, B, and C respectively:
>
> | Class | Prefix length | Max hosts |
> |---|---|---|
> | A | /8 | ~16.7 million |
> | B | /16 | 65,534 |
> | C | /24 | 254 |
>
> The gap between B and C was enormous. **Worked example from the book**: an
> organization with 2,000 hosts didn't fit in a Class C (/24, max 254), so it was forced
> to take a whole Class B (/16, 65,534 addresses) — wasting **more than 63,000
> addresses** it would never use. This rapid, wasteful depletion of the Class B space is
> exactly the problem CIDR (arbitrary-length prefixes) was invented to solve.

> [!info] Who actually allocates address blocks — the full chain
> **ICANN** → 5 **Regional Internet Registries (RIRs)**: **ARIN** (North America),
> **RIPE** (Europe), **APNIC** (Asia-Pacific), **LACNIC** (Latin America), **AFRINIC**
> (Africa) → ISPs → organizations. ICANN also manages the DNS root zone and (per the
> book) has "the very contentious job of assigning domain names and resolving domain
> name disputes."

> [!note] The IP broadcast address
> `255.255.255.255` — a datagram sent to this address is delivered to **all hosts on the
> same subnet**. Routers *can* forward it to neighboring subnets but normally don't.
> Not mentioned anywhere in the slides.

### DHCP — book-only detail

> [!danger] DHCP's mobility shortcoming (likely quiz material)
> Because a host gets a **new** IP address every time it joins a new subnet, **any
> ongoing TCP connection breaks** when a mobile node moves from one subnet to
> another mid-connection. DHCP alone cannot preserve an active connection across a
> subnet change — this is exactly the problem that mobile-IP / cellular network mobility
> mechanisms (Chapter 7) exist to solve.

> [!info] DHCP relay agent
> If a subnet has **no local DHCP server**, a **DHCP relay agent** (usually the
> first-hop router) forwards discover/request broadcasts to a known DHCP server
> elsewhere, and relays the replies back. This is how one central DHCP server can
> serve multiple subnets.

> [!info] Official DHCP message names
> The book (unlike the slides) names the actual message types you'd see in a packet
> capture: **DHCPDISCOVER → DHCPOFFER → DHCPREQUEST → DHCPACK**. Also:
> - **Lease time**: how long an address stays valid (commonly hours to days) — the
>   client can **renew** before it expires.
> - Because DHCP is automatic, plug-and-play, it's also called a **zeroconf**
>   (zero-configuration) protocol.
> - A reference open-source implementation exists from the **Internet Systems
>   Consortium (ISC)**.

## §4.3.3 — NAT: book-only detail

> [!info] The motivating scenario the book uses
> Small office/home office (SOHO) networks with many IP-capable devices (phones,
> tablets, game consoles, smart TVs, printers…) would otherwise need a whole block of
> public addresses from the ISP — wasteful and administratively painful for a home user.
> NAT lets the ISP hand out just **one** public address per household.

> [!success] NAT's port-space capacity
> Because the TCP/UDP port field is 16 bits, a single NAT public IP can in principle
> support **over 60,000 simultaneous connections** ($2^{16} - \text{well-known ports} \approx 60{,}000+$).

> [!tip] The router's own address is *also* usually DHCP
> A neat detail: the home NAT router typically gets its **own** WAN-side address via
> DHCP from the ISP, **and** simultaneously runs **its own DHCP server** on the LAN
> side to hand out private addresses to home devices. Double DHCP duty.

> [!warning] Two distinct objections to NAT (the book separates these — good exam distinction)
> 1. **Practical**: port numbers are meant to address *processes*, not hosts. This breaks
>    incoming connections to servers/peers behind a NAT (e.g. P2P). Mitigations: **NAT
>    traversal tools**, e.g. **STUN (RFC 5389)**.
> 2. **Architectural/philosophical**: routers are "supposed to" be layer-3-only devices;
>    NAT rewriting transport-layer port numbers violates that separation of concerns and
>    the end-to-end principle (see [[11 - Middleboxes & Internet Architecture]]).

### 🆕 New topic entirely: Firewalls & Intrusion Detection/Prevention Systems

> [!question] Not on the slides at all
> This "Focus on Security" sidebar sits right after the NAT section in the book, and
> nothing like it appears in the slide deck for this chapter.

| Device | What it does | Depth of inspection |
|---|---|---|
| **Firewall** | Denies suspicious datagrams entry based on header fields (src/dst IP, port); can track TCP connection state and only admit datagrams belonging to approved connections | Header fields only |
| **Intrusion Detection System (IDS)** | Performs **deep packet inspection** — examines payload too, not just headers; matches against a database of known attack **signatures**; raises an alert | Headers + payload |
| **Intrusion Prevention System (IPS)** | Same as IDS, but actively **blocks** the offending packets instead of just alerting | Headers + payload |

> [!warning] Limitation acknowledged by the book
> Signature-based IDS/firewalls **cannot** catch novel attacks for which no signature
> yet exists — they only protect against *known* attack patterns.

## §4.3.4 — IPv6: book-only detail

> [!info] The exhaustion-date debate (historical color)
> Two IETF working-group leaders estimated IPv4 exhaustion would hit in **2008** and
> **2018** respectively — both wrong in different directions. The real milestone: **IANA
> allocated the last unassigned IPv4 block to the RIRs in February 2011.**

> [!tip] Trivia: what happened to IPv5?
> It was going to be the **ST-2** protocol, but ST-2 was dropped — so the next version
> after IPv4 was numbered **IPv6**, not IPv5.

> [!info] IPv6 address types — new content vs. slides
> The slides never mention IPv6 address *types*. The book does:
> - **Unicast** — one sender, one specific receiver (as in IPv4).
> - **Multicast** — one sender, a group of receivers (also exists in IPv4).
> - **Anycast** — *new in IPv6* — delivers a datagram to **any one** of a group of hosts,
>   e.g. sending an HTTP GET to the *nearest* of several mirror servers.
> - There is **no broadcast address** in IPv6 (multicast/anycast supersede it).

> [!info] Why IPv6 dropped the header checksum, precisely
> The book's reasoning: transport-layer (TCP/UDP) and link-layer (Ethernet) protocols
> already checksum their own data, so a network-layer checksum was judged
> "sufficiently redundant" — and, since it would otherwise need recomputing at every
> hop (just like IPv4's), dropping it was a straightforward speed win.

> [!info] "Packet Too Big" — IPv6's answer to fragmentation
> If an IPv6 router receives a datagram too large for the outgoing link, it doesn't
> fragment it — it **drops the datagram** and sends a **"Packet Too Big" ICMPv6 error**
> back to the sender, who must resend at a smaller size. This is the mechanism behind
> what's generally called **Path MTU Discovery**.

> [!info] Why a "flag day" was never realistic
> A flag day = a single moment when *everyone* switches protocols simultaneously. The
> book points to the **NCP → TCP transition ~40 years ago** — even then, with a tiny
> Internet run by a handful of "wizards," a flag day wasn't feasible. With billions of
> devices today, it's "unthinkable." This is *why* tunneling (see
> [[09 - IPv6 & Tunneling]]) became the practical transition strategy instead.

> [!note] How a tunnel-exit router recognizes an IPv6-in-IPv4 tunnel
> The outer IPv4 datagram's **protocol field = 41** signals "my payload is an IPv6
> datagram" (per RFC 4213).

> [!quote] The book's closing lesson (nice essay-style exam material)
> "Introducing new protocols into the network layer is like replacing the foundation of
> a house... Introducing new application-layer protocols is like adding a new layer of
> paint to a house." Network-layer change (IPv6, multicast, resource reservation) has
> historically been slow and hard; application-layer change (Web, IM, streaming,
> social media) has been fast and easy. This explains *why* IPv6 took so long — see the
> live 2026 adoption numbers in [[09 - IPv6 & Tunneling]].

> [!note] Adoption stats in the book are stale — already covered
> The book (2020 printing) cites ~25% Google IPv6 traffic and ~1/3 of US government
> domains. [[09 - IPv6 & Tunneling]] already updates this with the **2026 milestone**
> (native IPv6 crossed 50% of Google traffic in April 2026) — no action needed here.

---
#### Navigation
◀ [[11 - Middleboxes & Internet Architecture]] · ▶ [[13 - Practice Problems, IP Datagram to IPv6]]
