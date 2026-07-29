# Trunk Ports

## Overview

A trunk port is a switch interface designed to carry traffic from multiple VLANs simultaneously over a single physical link.

Trunk links originated from the concept of trunking used in analog telephone systems, where multiple conversations could share the same physical medium. This idea was later adapted to modern data networks, allowing multiple logical networks (VLANs) to share one physical connection without mixing their traffic.

Trunk ports operate at Layer 2 and transport VLAN traffic using the IEEE 802.1Q tagging standard.

## The problem trunk ports solve

Before trunk links existed, carrying multiple VLANs between two switches required dedicating one physical link per VLAN.

For example, if two switches needed to transport traffic for 20 VLANs, they would require:

- 20 physical cables
- 20 switch ports on each switch
- More rack space
- Higher costs
- Poor scalability
- More complex administration

Trunk ports solve this problem by allowing multiple VLANs to share a single physical link while keeping their traffic logically separated.

## Where trunk links are used

Although trunk links are most commonly used between switches, they're also used to connect:

- Switch ↔ Switch
- Switch ↔ Router (Router-on-a-Stick)
- Switch ↔ Layer 3 Switch
- Switch ↔ Hypervisor
- Switch ↔ Firewall

The primary purpose of trunk ports is scalability and efficient bandwidth utilization.

## Trunk ports vs. access ports

| | Access port | Trunk port |
|---|---|---|
| VLANs carried | A single VLAN | Multiple VLANs simultaneously |
| Frame tagging | Untagged | Tagged (802.1Q), except the Native VLAN |
| Typical use | Connecting end devices | Connecting switches, routers, firewalls, hypervisors |

## How trunk ports work

When an Ethernet frame enters a switch through an access port, it arrives untagged, since end devices are generally unaware of VLAN tagging.

The switch internally associates the frame with the VLAN configured on the ingress access port — at this stage, the frame itself remains untagged. For example, if a frame enters through an access port assigned to VLAN 10 and leaves through another access port that also belongs to VLAN 10, the switch simply forwards it without adding any VLAN tag.

The situation changes when that frame needs to leave through a trunk port. Since trunk links transport traffic for multiple VLANs, the receiving device must know which VLAN each frame belongs to. Immediately before transmitting the frame through the trunk link, the switch inserts an 802.1Q tag into the Ethernet frame. This tag contains several fields, including the VLAN ID, which identifies the VLAN associated with that frame.

When the receiving switch (or any other network device connected through a trunk) receives the frame, it inspects the 802.1Q header, reads the VLAN ID, and immediately knows which VLAN the frame belongs to.

## Native VLAN

Although trunk links normally transport tagged traffic, there's one important exception: frames belonging to the Native VLAN are transmitted without an 802.1Q tag (unless the switch is explicitly configured to tag the native VLAN too).

The Native VLAN exists because the IEEE 802.1Q standard was originally designed to maintain compatibility with legacy devices and protocols that only understood traditional Ethernet frames without VLAN tags.

When an untagged frame arrives on a trunk port, the receiving switch automatically associates it with the configured Native VLAN.

For proper operation, the Native VLAN must match on both ends of the trunk link. If each side is configured with a different Native VLAN, a Native VLAN mismatch occurs, which can lead to connectivity problems and, in some cases, security vulnerabilities.

!!! note "Security consideration"
    A Native VLAN mismatch isn't just a connectivity annoyance — it can enable a VLAN hopping attack (via double-tagging), where an attacker crafts a frame with two 802.1Q tags to jump into a VLAN it shouldn't have access to. Common best practice: avoid using the default VLAN 1 as the Native VLAN, and set it to an unused VLAN ID instead.

---

## Dynamic Trunk Protocol (DTP)

Dynamic Trunk Protocol (DTP) is a Cisco proprietary Layer 2 protocol that automatically negotiates whether a switch port should operate as an access port or as a trunk port.

Instead of manually configuring both ends of a link, Cisco introduced DTP to automate the trunk negotiation process between compatible switches.

Several operating modes are available:

### Access

```cisco
switchport mode access
```

The interface always operates as an access port and never attempts to negotiate a trunk.

---

### Trunk

```cisco
switchport mode trunk
```

The interface always operates as a trunk port regardless of any negotiation.

---

### Dynamic Auto

Passive mode.

The switch waits for the neighboring device to initiate DTP negotiation. It never starts the negotiation itself.

If the opposite side actively negotiates a trunk, the port accepts it.

---

### Dynamic Desirable

Active mode.

The switch actively sends DTP messages attempting to negotiate a trunk link with the neighboring switch.

---

### DTP Negotiation Results

| Switch A | Switch B | Result |
|----------|----------|--------|
| Access | Access | Access |
| Access | Trunk | Configuration mismatch |
| Trunk | Trunk | Trunk |
| Dynamic Auto | Dynamic Auto | Access |
| Dynamic Desirable | Dynamic Auto | Trunk |
| Dynamic Desirable | Dynamic Desirable | Trunk |
| Trunk | Dynamic Auto | Trunk |

---

## Best Practice

Today, DTP is rarely used in production networks.

Modern networks typically configure trunk and access ports manually.

```cisco
switchport mode access
```

or

```cisco
switchport mode trunk
```

This approach provides greater predictability and improves security.

Leaving a port in Dynamic Desirable allows another Cisco switch to negotiate a trunk automatically.

An attacker could exploit this behavior by connecting a rogue switch and successfully negotiating a trunk, potentially gaining access to traffic from multiple VLANs. This attack is commonly known as VLAN Hopping.

For this reason, it is considered best practice to disable DTP negotiation whenever trunk negotiation is not required.

```cisco
switchport mode access
switchport nonegotiate
```

or

```cisco
switchport mode trunk
switchport nonegotiate
```