# Neighbor Discovery Protocol (NDP)

## Overview

NDP is one of the most important protocols in IPv6. It has 5 main functions:

- Discovering neighbors
- Discovering routers
- Resolving IPv6 addresses to MAC addresses
- Detecting duplicate addresses
- Verifying that a neighbor is still reachable

## Solicited-Node Multicast

A special multicast address used for address resolution.

Every IPv6 unicast or anycast address automatically generates a Solicited-Node Multicast address:

```text
FF02::1:FFXX:XXXX
```

Where the last 24 bits (`XX:XXXX`) correspond to the last 24 bits of the original IPv6 address.

Example:

```text
IPv6:                    2001:db8:1::1234:5678
Last 24 bits:             34:5678
Solicited-Node Multicast: FF02::1:FF34:5678
```

## How address resolution works

When a device wants to send a packet to another device but only has its IPv6 address, it still needs the destination's MAC address.

1. It checks its Neighbor Cache (the IPv6 equivalent of the ARP table). If it doesn't already know the MAC for the destination IPv6 address, it can't build the Ethernet frame yet.
2. It calculates the destination's Solicited-Node Multicast address — the multicast group associated with that IPv6 address, derived from its last 24 bits.
3. It builds a Neighbor Solicitation (NS) message, containing the target IPv6 address.
4. That NS message is encapsulated into an Ethernet frame, addressed to the multicast MAC corresponding to that IPv6 multicast group.
5. Every device checks the Neighbor Solicitation; whoever isn't the target discards it.
6. The target device replies with a Neighbor Advertisement (NA), sent back to the originating device.

Step 4 relies on switches being able to forward that multicast frame only to the ports where interested devices are — which requires MLD snooping (the IPv6 equivalent of IGMP snooping) to be enabled. Without it, the switch simply floods the multicast frame to every port in the VLAN, much like a broadcast — the frame still only gets processed by the intended device at Layer 3, but every other device on the segment still receives it at Layer 2.

This same NS/NA exchange is reused for Duplicate Address Detection (DAD) — the difference is that during DAD, a device sends the Neighbor Solicitation for its *own* tentative address, from the unspecified address (`::`), to check whether anyone else on the segment already has it.

---
