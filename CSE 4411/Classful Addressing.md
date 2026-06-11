---
tags:
  - networking
  - ipv4
  - fundamentals
aliases:
  - IPv4 Octets
---

| Class       | Leading Bits | First Octet Range | Default Subnet Mask | Purpose                                  |
| ----------- | ------------ | ----------------- | ------------------- | ---------------------------------------- |
| **Class A** | 0            | 1 to 126          | \(255.0.0.0\)       | Massive networks (over 16 million hosts) |
| **Class B** | 10           | 128 to 191        | \(255.255.0.0\)     | Medium networks (up to 65,534 hosts)     |
| **Class C** | 110          | 192 to 223        | \(255.255.255.0\)   | Small networks (up to 254 hosts)         |
| **Class D** | 1110         | 224 to 239        | _N/A_               | Reserved for **multicast** addressing    |
| **Class E** | 1111         | 240 to 255        | _N/A_               | Reserved for **experimental** research   |

# IPv4 Octets Explained

An **octet** is a technical term for a block of **8 binary bits** (0s and 1s), which equals exactly **1 byte** of data. 

In an IPv4 address, there are 32 bits in total, divided into **four octets** separated by periods (dots). The **"first octet"** is simply the very first 8-bit block on the far left of the IP address.

## Visual Breakdown

Here is how a standard IP address like `192.168.1.1` looks in decimal format versus its underlying binary format:

```text
Decimal Format:    192    .    168    .     1     .     1
                  └───┘       └───┘       └───┘       └───┘
                    │           │           │           │
Binary Format:   11000000 .  10101000 .  00000001 .  00000001
                 └──────┘    └──────┘    └──────┘    └──────┘
                    ▲           ▲           ▲           ▲
                 1st Octet   2nd Octet   3rd Octet   4th Octet
```

> [!INFO] Why the First Octet Matters
> In the old classful system, routers only looked at the **first octet** to instantly know how to route the data:
> - **Class A:** First octet is between decimal `1` and `126`.
> - **Class B:** First octet is between decimal `128` and `191`.
> - **Class C:** First octet is between decimal `192` and `223`.

***

## Related Notes
- [[Classful Addressing]]
- [[CIDR]]
