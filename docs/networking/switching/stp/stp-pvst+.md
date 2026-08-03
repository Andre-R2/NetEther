# Per VLAN Spanning Tree (PVST+)

## Overview

Traditional IEEE 802.1D Spanning Tree Protocol has one important limitation: it creates a single spanning tree instance for the entire switched network, regardless of how many VLANs exist.

Imagine a network with multiple VLANs connected through redundant trunk links. Classic STP elects a single Root Bridge, builds a single spanning tree, and blocks one of the redundant links to prevent Layer 2 loops.

As a result, every VLAN follows exactly the same forwarding path while the other redundant link remains unused — wasting available bandwidth.

## What PVST does differently

Per VLAN Spanning Tree (PVST) is Cisco's solution to this limitation. Instead of creating one spanning tree for the entire network, PVST creates one independent STP instance per VLAN.

Each VLAN elects its own Root Bridge, calculates its own spanning tree, and independently decides which ports should forward traffic and which should remain blocked.

For example:

- VLAN 10 may use Trunk A as its active path, while Trunk B remains blocked for that VLAN.
- VLAN 20 may use Trunk B as its active path, while Trunk A remains blocked for that VLAN.

Both trunk links are used simultaneously, but each VLAN still maintains its own loop-free topology.

The main advantage of PVST is that it preserves Layer 2 loop prevention while distributing traffic from different VLANs across different physical links — improving bandwidth utilization and providing basic load balancing.

!!! note "PVST vs PVST+"
    The original PVST ran over Cisco's legacy ISL trunking, which had a field for carrying per-VLAN STP information natively. Since ISL was phased out in favor of the standard 802.1Q, Cisco created PVST+, which tunnels that same per-VLAN BPDU information over a regular 802.1Q trunk — allowing PVST to keep working even when the trunk also connects to non-Cisco equipment that only understands standard (single-instance) STP.

!!! note "The trade-off"
    Running a separate STP instance per VLAN doesn't come for free — every instance means its own BPDUs, its own calculations, and its own CPU/memory overhead on every switch. In a network with hundreds of VLANs, this adds up fast. This scalability concern is exactly what MST (Multiple Spanning Tree) was later designed to solve, by mapping multiple VLANs onto a smaller number of spanning tree instances.

---

**Next:** Rapid PVST+ (RPVST+)