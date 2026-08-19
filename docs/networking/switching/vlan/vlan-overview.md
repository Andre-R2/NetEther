# VLANs

## Overview

Before VLANs existed, networks were commonly designed as flat networks, where many devices shared the same Layer 2 broadcast domain.

If an organization needed to separate different areas, such as Sales, Administration, or Servers, the traditional approach was to use separate physical switches and independent cabling for each group.

This made the infrastructure expensive and difficult to scale, modify, or relocate. Moving a device from one area to another could require physical changes to the network cabling.

In a flat network, all devices within the same local segment shared the same broadcast domain.

For a small home LAN, this was usually not a problem. However, as enterprise networks grew to hundreds or thousands of devices, this design became inefficient. Large broadcast domains increased unnecessary traffic, consumed network resources, and made network management more difficult.

When a device needed to send a broadcast message, such as an ARP request, every device inside the same broadcast domain received and processed that traffic, even if the information was irrelevant to them.

This created the need for logical network segmentation.

## VLANs

A VLAN (Virtual Local Area Network) allows a single physical switch to behave as multiple logical switches.

Instead of requiring separate physical infrastructure for each department or network segment, VLANs divide one physical switching device into multiple isolated Layer 2 networks.

For example, a single switch can contain:

- VLAN 10 for Users
- VLAN 20 for Servers
- VLAN 30 for Voice
- VLAN 40 for Guest Devices

Although these devices share the same physical hardware, they belong to different logical networks.

## Broadcast Domains

One of the main advantages of VLANs is the creation of independent broadcast domains.

Without VLANs, a switch represents one large broadcast domain.

![Without VLANs](../../../assets/net-assets/sw-single-broadcast-domain-light.svg#only-light)
![Without VLANs](../../../assets/net-assets/sw-single-broadcast-domain-dark.svg#only-dark)


With VLANs, the same physical switch can be divided into multiple independent broadcast domains.

![With VLANs](../../../assets/net-assets/sw-broadcast-domains-light.svg#only-light)
![With VLANs](../../../assets/net-assets/sw-broadcast-domains-dark.svg#only-dark)


When a broadcast frame arrives from VLAN 10, the switch forwards that traffic only through ports that belong to VLAN 10.

Devices assigned to other VLANs will not receive that broadcast because they belong to a different Layer 2 broadcast domain.

Internally, the switch maintains information about VLAN membership and the ports assigned to each VLAN. When forwarding frames, the switch considers both the destination MAC address and the VLAN to which the frame belongs.

## VLANs and Network Layers

When an organization needs to connect multiple subnets to the same switch, subnetting alone is not enough to provide Layer 2 segmentation.

Subnetting is a Layer 3 technique that divides an IP address space into multiple subnets. 
Communication between different subnets requires a Layer 3 device, such as a router or a Layer 3 switch.

VLANs, on the other hand, are a Layer 2 technology used to segment a switch into multiple independent broadcast domains. Without VLANs, the switch operates as a single broadcast domain, broadcast frames are flooded across all ports within that domain.

Therefore, subnetting provides a Layer 3 segmentation, while VLANs provide a Layer 2 segmentation. Each VLAN is commonly associated with its own IP subnet, allowing the Layer 2 and Layer 3 segmentation to align.

A common enterprise design looks like this:

```text
VLAN 10
192.168.10.0/24

VLAN 20
192.168.20.0/24
```

This relationship is considered a best practice because it creates a clear mapping between Layer 2 segmentation and Layer 3 addressing.

However, the VLAN itself is not the subnet. They are different technologies operating at different layers.

## VLANs in Network Security

VLANs are not only used for organization. They are also an important part of network security, isolation, and administration.

Consider an enterprise network where every device exists in the same network segment:

- User computers
- Servers
- Databases
- CCTV cameras
- Guest devices
- IoT devices

Many IoT devices, such as IP cameras, provide limited security features and may receive few or no firmware updates.

If one of these devices is compromised, an attacker could use it as an entry point to scan the internal network, discover servers, and attempt lateral movement toward more critical systems.

By placing CCTV systems in their own VLAN and subnet, administrators isolate them from sensitive parts of the network.

If a device inside that VLAN is compromised, the attacker does not automatically gain access to every other device in the organization.

A VLAN alone is not a complete security solution, but it makes it possible to implement stronger security policies using:

- Firewalls
- Access Control Lists (ACLs)
- Layer 3 routing policies
- Network monitoring
- Access control between VLANs

## Benefits of VLANs

### Reduced Attack Surface

Segmenting the network limits the impact of a security breach. Compromising one VLAN is far less damaging than compromising an entire flat network.

### Reduced Lateral Movement

When proper security policies are applied between VLANs, malware and attackers have a much harder time moving from one network segment to another.

### Reduced Broadcast Traffic

Each VLAN creates its own broadcast domain, preventing unnecessary broadcasts from reaching every device on the network.

This reduces network noise and improves overall efficiency.

### Better Organization and Management

VLANs make enterprise networks easier to design, troubleshoot, expand, and maintain by grouping devices according to their function, department, location, or security requirements.

## VLANs in Enterprise Network Design

Modern enterprise networks are usually designed around logical functions rather than physical locations.

A common design might look like this:

```text
VLAN 10   Users
VLAN 20   Servers
VLAN 30   Voice
VLAN 40   Guest Network
VLAN 50   CCTV
```

This approach simplifies administration, improves security, and allows different communication policies to be applied to each network segment.
