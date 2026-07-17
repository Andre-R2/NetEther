# Address Resolution Protocol (ARP)

## Overview

The Address Resolution Protocol (ARP) is used to resolve an IPv4 address into a MAC address within a local network.

ARP operates between the Internet Layer and the Data Link Layer, allowing devices to discover the Layer 2 address required to deliver Ethernet frames on a LAN.

Whenever a device needs to send data to another device, it not only needs the destination IP address, but also the destination MAC address. If that MAC address is not already stored in the local ARP cache, the device must discover it before communication can begin.

## How ARP works

When the sender knows the destination IP address but not its MAC address, it generates an ARP Request.

An ARP Request is a broadcast message that asks:

> "Who has this IP address?"

The request is sent to the local switch, which floods it through all ports belonging to the same broadcast domain.

The request also includes the sender's IP address and MAC address so the destination device knows where to send the response.

If a device owns the requested IP address, it replies with an ARP Reply containing its MAC address.

The sender then stores the IP-to-MAC mapping in its ARP cache, allowing future communications to occur without sending another request until the cache entry expires.

## Communication outside the local network

If the destination device belongs to another network, the process changes slightly.

Instead of resolving the destination device's MAC address, the sender performs ARP for the default gateway's MAC address.

Once the frame reaches the router, the router removes the Layer 2 header, consults its routing table, and forwards the packet toward the next network. If necessary, it performs another ARP process on the outgoing network to discover the next device's MAC address.

For this reason, ARP only resolves MAC addresses within the local network.

## ARP Cache

The ARP cache is a temporary table stored in the device's volatile memory that maintains mappings between IPv4 addresses and MAC addresses.

Caching these mappings improves network performance by avoiding unnecessary ARP Requests every time a device communicates with a known destination.

Since the cache is stored in RAM, its entries eventually expire and can also be cleared manually.

## Limitations

- Only supports IPv4.
- Only resolves MAC addresses within the local network.
- Routers do not forward ARP Requests between networks.

## Why it matters

Without ARP, a device could know the destination IP address but would have no way to discover the destination MAC address needed to build an Ethernet frame. As a result, communication over Ethernet networks would not be possible.

---

**Next:** [IPv4 Addressing](ipv4-addressing.md)
