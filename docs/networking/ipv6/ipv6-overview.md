# IPv6 Overview

IPv6 (Internet Protocol Version 6) is the protocol responsible for assigning logical addresses to devices so they can communicate over a network and the Internet.

It is the successor to IPv4, which has exhausted its available public address space due to the rapid growth of the Internet.

IPv4 uses 32-bit addresses, providing approximately 4.3 billion unique addresses. When IPv4 was designed, this number seemed more than enough. However, with the expansion of the Internet, smartphones, cloud computing, and the Internet of Things (IoT), the demand for IP addresses eventually exceeded the available supply.

IPv6 solves this problem by using 128-bit addresses, providing approximately:

```text
2^128 ≈ 340 undecillion addresses
```

This number is so large that, for practical purposes, it can be considered virtually unlimited.

## Advantages of IPv6

Compared to IPv4, IPv6 provides several improvements:

- A vastly larger address space.
- Eliminates the need for techniques such as NAT to conserve public IPv4 addresses.
- Makes network addressing and hierarchical routing more scalable.
- Includes native support for IPsec, providing authentication and encryption capabilities.
- Supports automatic address configuration (SLAAC), allowing hosts to configure themselves without manual intervention.
- Simplifies the packet header, improving forwarding efficiency.
- Removes the need for broadcast traffic by relying on multicast and other mechanisms.

Although subnetting still exists in IPv6, it is no longer performed to conserve address space as it was in IPv4. Instead, it is mainly used for network organization, hierarchy, and routing efficiency.