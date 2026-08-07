# IPv6 Address Types

## Overview

IPv6 addresses fall into three main categories: Unicast, Multicast, and Anycast.

Within Unicast, there are three subtypes: Global Unicast, Link-Local, and Unique Local.

## Unicast

### Global Unicast

The IPv6 equivalent of IPv4 public addresses — routable across the Internet and globally unique.

### Unique Local (ULA)

The IPv6 equivalent of IPv4 private addresses, serving the same function. The difference is that ULA addresses include a Global ID — a random identifier that makes the probability of two identical ULAs colliding very low.

### Link-Local

Conceptually similar to MAC addresses in terms of scope — they only work within the local segment and aren't routable. However, they operate at a different layer and serve a different purpose.

A link-local address works somewhat like APIPA in IPv4, but its role in IPv6 is far more central: rather than being a fallback for misconfigured devices, it's part of how the protocol itself functions. A device connecting to an IPv6 network automatically generates a link-local address, which it uses to communicate within the local segment and to participate in NDP (Neighbor Discovery Protocol).

## Multicast

Multicast is one of the biggest changes IPv6 introduces compared to IPv4 — IPv6 completely eliminates broadcast and replaces it with multicast.

A multicast address identifies a group of devices, not just one. When you need to send traffic to a group, hosts that don't belong to it simply don't have to process that traffic — which is exactly why multicast matters so much in IPv6, especially for NDP.

In IPv4, achieving something similar required broadcast (for example, with ARP): if only 1 out of 500 hosts cared about a message, the other 499 still had to process it, wasting resources and adding noise to the network.

### Multicast address structure

All multicast addresses start with `FF` and belong to the `FF00::/8` prefix.

Examples: `ff02::1`, `ff02::2`, `ff05::1`, `ff02::fb`

An address like `FF02::1` breaks down like this:

```text
FF   →  identifies it as a Multicast address
02   →  scope
::1  →  identifies the multicast group
```

That's why `ff02::1` means: all nodes on the local link.

### The most important ones

| Address | Meaning |
|---|---|
| `FF02::1` | All nodes on the local link. Send a packet here, and every IPv6 device on the same segment receives it. |
| `FF02::2` | All routers on the local link. Only routers listen here — regular hosts ignore it. Useful when a host needs to discover routers. |
| `FF02::1:FFxx:xxxx` | Solicited-Node Multicast — the most important one. Every IPv6 address automatically generates its own. Used for Neighbor Discovery. |

Example: the address `2001:db8::1234:5678` automatically generates the solicited-node multicast address `ff02::1:ff34:5678` — taken from the last 24 bits of the original address.

### Multicast scope

Multicast addresses also carry a scope, indicating how far they should propagate. In `ff02::1`, the `02` means "local link only."

| Scope | Reach |
|---|---|
| 1 | Interface-local |
| 2 | Link-local |
| 5 | Site (organization) |
| 8 | Organization |
| E | Global |

### Automatic group membership

Operating systems automatically subscribe to certain groups. Any IPv6-capable device belongs to `ff02::1`; any router also belongs to `ff02::2`.

## Anycast

Anycast addresses have no distinct format that identifies them as such — their notation looks just like a ULA or Global Unicast address.

Anycast lets the same address identify the same service running in multiple locations. For example, you could configure three servers with the same IPv6 address in Tokyo, Madrid, and Mexico City. Devices send traffic to that one address, but only the closest or most optimal server responds — that decision is made by routers, which calculate the best path toward the available destinations.

A device located in Spain would likely have its traffic routed to the Madrid server, since it's probably the most optimal path. If that Madrid server failed, traffic would automatically be rerouted to one of the other available servers.

---

