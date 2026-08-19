# IEEE 802.1Q Tagging

## Overview

IEEE 802.1Q is the standard that defines how Ethernet frames are tagged to identify the VLAN to which they belong.

802.1Q is used on trunk links, allowing multiple VLANs to share a single physical connection while keeping their traffic logically separated.

## Why is VLAN Tagging Necessary?

Access ports can belong to only one VLAN.

Since they connect end devices such as PCs, printers, or servers, every frame entering or leaving an access port is transmitted without a VLAN tag.

This works perfectly because only one VLAN uses that link.

However, problems arise when a single physical link must carry traffic from multiple VLANs.

Without additional information, the receiving switch would have no way of determining which VLAN each frame belongs to.

IEEE 802.1Q solves this problem by inserting a VLAN tag into Ethernet frames before they are transmitted over a trunk link.

## Ethernet Frame Tagging

A standard Ethernet frame does not contain VLAN information.

When a frame leaves a trunk port, the switch inserts a 4-byte 802.1Q tag between the Source MAC Address and the EtherType/Length field.

The tagged frame looks like this:

```text
+---------+---------+------------+----------+---------+
| Dest MAC| Src MAC | 802.1Q Tag | EtherType| Payload |
+---------+---------+------------+----------+---------+
```

The 802.1Q tag contains several fields, including:

- VLAN ID (VID) — Identifies the VLAN to which the frame belongs.
- Priority Code Point (PCP) — Used for Layer 2 Quality of Service (QoS).
- Drop Eligible Indicator (DEI) — Indicates whether the frame may be dropped during network congestion.

For example, if the VLAN ID is 10, the receiving switch immediately knows that the frame belongs to VLAN 10.

## Trunk Communication

A trunk link can simultaneously transport traffic from many VLANs.

Each frame carries its own VLAN ID, allowing the receiving switch to separate the traffic correctly and forward it within the appropriate VLAN.

```text
VLAN 10 Frame ── Tag: VLAN 10 ──┐
                                │
VLAN 20 Frame ── Tag: VLAN 20 ──┼── Trunk Link
                                │
VLAN 30 Frame ── Tag: VLAN 30 ──┘
```

## Leaving the Trunk

When a tagged frame is forwarded out of an access port, the switch removes the 802.1Q tag before transmitting it.

As a result, end devices never see VLAN tags.
