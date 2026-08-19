# MAC Address Table

## Overview

The MAC table is a database that switches maintain, storing all MAC-to-port relationships. This is what allows a switch to know which device is connected to which port, and to forward Ethernet frames out the correct port.

## How entries get into the table

Entries can be registered in two ways:

| Type | How it's added | Behavior |
|---|---|---|
| Dynamic | Automatically, from the source MAC of every frame received | Temporary — stays as long as traffic keeps arriving from that MAC. Removed after a period of inactivity (MAC agin). |
| Static | Manually configured | Permanent, used for extra security or stability. Takes priority over dynamic entries if there's a conflict. |

If the switch receives traffic from the same MAC but on a different port, it deletes the old entry and registers the new one — keeping the table accurate as devices move around the network.

MAC address table looks like this:

| VLAN | MAC Address | Type | Port |
|------|-------------|-------|------|
| 1 | 001A.2B3C.4D5E | Dynamic | Fa0/1 |
| 1 | 0050.56AA.BBCC | Dynamic | Fa0/2 |
| 1 | 00E0.FC11.2233 | Dynamic | Fa0/5 |
| 1 | 08D4.2B99.7F10 | Dynamic | Fa0/7 |
| 10 | 00AA.11BB.22CC | Dynamic | Fa0/10 |
| 10 | 00CC.33DD.44EE | Static | Fa0/12 |
| 20 | 001C.58A2.9F01 | Dynamic | Gi0/1 |
| 20 | 00D0.97F8.7A55 | Dynamic | Gi0/2 |

| Column | Description |
|--------|-------------|
| VLAN | VLAN where the MAC address was learned. |
| MAC Address | Layer 2 hardware address of the device. |
| Type | Indicates whether the entry is Dynamic or Static. |
| Port | Switch interface associated with that MAC address. |

## Direct forwarding vs. flooding

When the switch has the destination MAC-to-port mapping already in its table, it can forward the frame as unicast, directly to that one port — avoiding flooding entirely.

But not every flooding event happens for the same reason — there are three distinct scenarios.

## Broadcast flooding

Happens whenever the switch receives an Ethernet frame whose destination MAC is `FF:FF:FF:FF:FF:FF` — a common example is an ARP Request, used when a device needs to resolve the MAC address associated with an IPv4 address.

The switch checks the Ethernet header: `Dest MAC: FF:FF:FF:FF:FF:FF` means Ethernet broadcast, and the switch forwards it out every port except the one it arrived on.

## Unknown unicast flooding

Sometimes a device already has both the IP and the MAC address of another device — so it skips the broadcast ARP Request entirely and starts sending unicast traffic directly.

When those frames reach the switch, it checks its MAC table. If the destination MAC isn't registered yet, the switch performs unknown unicast flooding — sending the frame out every port except the one it arrived on.

When the destination device receives the frame, it replies to the originating device. As that reply passes through the switch, it automatically learns the destination device's MAC address and associates it with the port it arrived on. From that point on, the switch can forward future unicast frames directly to that port, without flooding again.

## Multicast flooding

Happens when the frame is multicast and the switch doesn't have IGMP Snooping (IPv4) or MLD Snooping (IPv6) enabled. Since the switch has no way of knowing which ports have members of that multicast group, it sends the frame to every port in the VLAN.

---
