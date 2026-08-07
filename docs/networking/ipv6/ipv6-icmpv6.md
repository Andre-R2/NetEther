# ICMPv6

## Overview

ICMPv6 (Internet Control Message Protocol version 6) is a fundamental control protocol in IPv6. Devices use it to report errors, exchange network information, discover neighbors, discover routers, and participate in the autoconfiguration process.

Unlike protocols such as TCP or UDP, ICMPv6 doesn't carry user data — its job is to carry the information needed for the IPv6 network to function correctly.

In IPv4, many organizations blocked ICMP with little consequence. In IPv6, this is no longer advisable, since ICMPv6 is part of how the protocol itself operates. Blocking it can break mechanisms such as:

- SLAAC
- Neighbor Discovery Protocol (NDP)
- Router Discovery
- Duplicate Address Detection (DAD)
- Path MTU Discovery

## Message types

ICMPv6 handles two broad groups of messages.

### Error messages

Report when a problem occurs during communication.

Examples:

- Destination Unreachable
- Packet Too Big
- Time Exceeded
- Parameter Problem

### Informational messages

Enable IPv6's normal operation. The most important ones are:

- Echo Request
- Echo Reply
- Router Solicitation (RS)
- Router Advertisement (RA)
- Neighbor Solicitation (NS)
- Neighbor Advertisement (NA)
- Redirect

## Why it matters

Much of how IPv6 operates depends on ICMPv6. Protocols such as Neighbor Discovery Protocol (NDP) use ICMPv6 messages to discover neighbors, locate routers, detect duplicate addresses, and enable device autoconfiguration.

For this reason, ICMPv6 is considered one of the most important protocols in how IPv6 works internally.

## The most important ICMPv6 messages

Although ICMPv6 defines many message types, these are the ones that matter most for how an IPv6 network functions:

| Message | Function |
|---|---|
| Echo Request | Checks connectivity between two devices (ping). |
| Echo Reply | Responds to an Echo Request, confirming the destination is reachable. |
| Router Solicitation (RS) | A host requests configuration information from the routers on the network. |
| Router Advertisement (RA) | The router announces the network prefix and tells the host how to configure itself (SLAAC, DHCPv6, or both). |
| Neighbor Solicitation (NS) | Discovers neighbors and verifies that an IPv6 address isn't already in use by another device (DAD). |
| Neighbor Advertisement (NA) | Responds to a Neighbor Solicitation, providing the requested information. |
| Redirect | A router informs a host that a more efficient route exists toward a destination. |
| Destination Unreachable | Indicates the destination can't be reached. |
| Packet Too Big | Reports that a packet exceeds the MTU allowed on the link. |
| Time Exceeded | Indicates the packet ran out of Hop Limit before reaching its destination. |

!!! note
    Router Solicitation (RS), Router Advertisement (RA), Neighbor Solicitation (NS), Neighbor Advertisement (NA), and Redirect are all part of the Neighbor Discovery Protocol (NDP) message set — the protocol that replaces functions like ARP and handles router discovery, neighbor discovery, duplicate address detection, and IPv6 device autoconfiguration. Redirect is part of that same NDP message family too, even though its specific job (pointing a host to a better next-hop) is different from the discovery/autoconfiguration role the other four play.

---

