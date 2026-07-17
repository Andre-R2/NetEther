# Collision & Broadcast Domains

## Overview

Collision and Broadcast Domains are two fundamental concepts in Ethernet networking. They define how traffic propagates across a network and how devices share the communication medium.

Understanding these concepts is essential for designing scalable networks, reducing unnecessary traffic, and improving overall network performance.

## Broadcast Domain

A **Broadcast Domain** is the set of devices that can receive the same Ethernet broadcast frame. In other words, it defines the area of the network over which a broadcast message can propagate.

Within a Broadcast Domain, any device can send a broadcast frame that is received by every other device in that same domain.

Routers represent the natural boundary of a Broadcast Domain. When a router receives an Ethernet frame, it removes the Layer 2 header, extracts the IP packet, consults its routing table, and builds a new Ethernet frame for the next network. Since broadcasts exist only at Layer 2, they do not cross the router.

By default, switches and hubs forward broadcast frames through all ports except the one where the frame was received, meaning they do not divide Broadcast Domains.

However, modern networks do not always require routers to segment broadcast traffic. Managed switches can create **VLANs**, where each VLAN forms an independent Broadcast Domain. This allows multiple logical networks to coexist on the same physical infrastructure.

Limiting the size of Broadcast Domains is an important network design practice because excessive broadcast traffic consumes bandwidth and forces every device in the LAN to process packets that are often irrelevant to them.

## Collision Domain

A **Collision Domain** is a network segment where two or more devices could transmit simultaneously over the same physical medium, causing their signals to collide.

Collisions were common in early Ethernet networks that used **hubs**, since every connected device shared the same transmission medium. A hub simply repeats incoming signals out of every port without examining MAC addresses or making forwarding decisions, meaning the entire hub operates as a single Collision Domain.

Modern Ethernet switches solve this problem by creating a separate Collision Domain for each switch port. In addition, **Full-Duplex** communication allows devices to transmit and receive simultaneously using separate channels, effectively eliminating collisions in today's switched Ethernet networks.

As a result, collisions are extremely rare in modern networks, and **CSMA/CD (Carrier Sense Multiple Access with Collision Detection)** has become largely obsolete.

## Broadcast vs Collision Domains

| Feature | Broadcast Domain | Collision Domain |
|----------|------------------|------------------|
| Defines | The scope of broadcast traffic | The area where signal collisions may occur |
| Layer | Data Link (Layer 2) | Physical (Layer 1) |
| Separated by | Routers and VLANs | Switch ports |
| Shared by a Hub | Yes | Yes |
| Shared by a Switch | Yes (unless VLANs are configured) | No (one per port) |
| Modern relevance | Still fundamental | Mostly historical due to switched Full-Duplex Ethernet |

## Duplex Communication

Ethernet links can operate in three different communication modes, depending on how data flows between two devices.

### Simplex

In **Simplex** communication, data flows in only one direction. One device always transmits, while the other only receives.

Examples include traditional television and radio broadcasting.

### Half Duplex

In **Half Duplex**, both devices can transmit and receive data, but **not at the same time**. Since both devices share the same communication medium, simultaneous transmissions may result in collisions.

Half Duplex was commonly used in early Ethernet networks connected through **hubs**, where every device shared the same Collision Domain.

### Full Duplex

In **Full Duplex**, both devices can transmit and receive data simultaneously using separate communication channels. Because the transmission and reception paths are independent, collisions cannot occur.

Modern switched Ethernet networks operate almost exclusively in **Full Duplex**, providing higher throughput and eliminating the need for collision detection mechanisms such as **CSMA/CD**.

## Comparison

| Mode | Transmission | Collisions | Common Use |
|------|--------------|------------|------------|
| **Simplex** | One-way only | No | Television, Radio |
| **Half Duplex** | Both directions, one at a time | Yes | Legacy hub-based Ethernet |
| **Full Duplex** | Simultaneous in both directions | No | Modern switched Ethernet |

**Next:** Switching