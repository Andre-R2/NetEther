# Access Ports

## Overview

An access port is a switch interface designed to connect end devices, such as PCs, printers, IP phones, or servers, to a single VLAN.

Unlike trunk ports, an access port belongs to only one VLAN at a time and always sends and receives untagged Ethernet frames.

## How Access Ports Work

When an end device sends an Ethernet frame, the frame reaches the switch without any VLAN tag.

The switch already knows which VLAN that access port belongs to. For example, if the port is configured as VLAN 10, the switch internally associates every incoming frame on that port with VLAN 10.

Although the frame itself remains untagged, the switch keeps track of its VLAN membership throughout the entire switching process.

From that point on, every Layer 2 operation performed by the switch occurs within VLAN 10, including:

- Source MAC learning
- CAM Table lookup
- Frame forwarding
- Flooding decisions

From the switch's perspective, every VLAN behaves as an independent Layer 2 network.

## Forwarding Frames

If the destination device is connected to another access port in the same VLAN, the switch forwards the frame through that port.

Before leaving the switch, the frame is transmitted without any VLAN tag, exactly as it arrived.

## Communication Between VLANs

An Ethernet frame cannot be forwarded directly between different VLANs.

If a device in VLAN 10 needs to communicate with a device in VLAN 20, the frame must be processed by a Layer 3 device, such as a router or a Layer 3 switch.

The Layer 3 device receives the packet, performs routing, and builds a new Ethernet frame for the destination VLAN.

This process is known as Inter-VLAN Routing.

## Important

End devices are completely unaware of VLANs.

They always send and receive standard Ethernet frames without 802.1Q tags.

VLAN membership exists only inside the switch. The switch is responsible for associating incoming frames with the correct VLAN and handling all Layer 2 operations accordingly.