# IPv4 Address Classes

## Overview

IPv4 classful addressing was the original method used to divide the IPv4 address space into five predefined address classes: A, B, C, D, and E.

Each class allocated a fixed number of bits to the network and host portions of an IPv4 address. The size of each network was predetermined, making address allocation simple but inflexible.

| Class | Purpose | Network Portion | Host Portion |
|---|---|---|---|
| A | Very large networks | 1 octet | 3 octets |
| B | Medium-sized networks | 2 octets | 2 octets |
| C | Small networks | 3 octets | 1 octet |
| D | Multicast | N/A | N/A |
| E | Experimental and research | N/A | N/A |

## Class identification

In the classful addressing system, the first bits of the first octet were reserved to identify the address class. The remaining bits formed part of the Network ID.

| Class | Reserved Bits | Binary Pattern | First Octet Range |
|---|---|---|---|
| A | 1 bit | `0xxxxxxx` | 1–126* |
| B | 2 bits | `10xxxxxx` | 128–191 |
| C | 3 bits | `110xxxxx` | 192–223 |
| D | 4 bits | `1110xxxx` | 224–239 |
| E | 4 bits | `1111xxxx` | 240–255 |

!!! note
    Although the binary pattern for Class A allows values from **0–127**, the ranges **0.0.0.0/8** and **127.0.0.0/8** are reserved. Therefore, usable Class A networks begin at **1.0.0.0** and end at **126.0.0.0**.

## Limitations of classful addressing

The classful addressing system was simple, but it wasted a significant number of IPv4 addresses.

For example, suppose an organization required approximately **300 host addresses**:

- A **Class C** network supports only **254 usable hosts**, which is insufficient.
- The next available option was a **Class B** network, capable of supporting **65,534 hosts** — far more than required.

As a result, organizations often received address blocks much larger than necessary, leaving thousands of addresses permanently unused.

This lack of flexibility became a major problem as the Internet expanded rapidly. Eventually, classful addressing could no longer scale efficiently, leading to the adoption of **CIDR (Classless Inter-Domain Routing)**, which replaced fixed address classes with variable-length network prefixes.

---

**Next:** CIDR