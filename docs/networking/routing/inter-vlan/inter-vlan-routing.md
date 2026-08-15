# Inter-VLAN Routing

## VLANs vs. subnets

Subnets and VLANs are usually thought of together, but they're really two different concepts.

**Subnets** are a Layer 3 concept — they divide an address space into smaller IP networks. Each subnet represents a distinct IP network, and communication between subnets requires routing.

**VLANs**, on the other hand, are a Layer 2 concept — they separate broadcast domains. A switch with no VLAN segmentation keeps all of its ports within the same broadcast domain.

If several IP subnets are connected to the same physical switch but belong to the same VLAN, those subnets remain independent at Layer 3, but the devices still share the same Layer 2 broadcast domain. A Layer 2 broadcast can reach devices belonging to different IP subnets, as long as they're all inside the same VLAN.

VLANs let you split that broadcast domain into multiple independent ones. That's why, even though VLAN and subnet are different concepts, they're normally used together: a VLAN represents the Layer 2 broadcast domain, and a subnet represents the IP network associated with it.

## Methods of Inter-VLAN routing

There are two main methods for routing between VLANs.

### 1. Router-on-a-Stick

This method uses a router connected to a switch through a single trunk link, capable of carrying multiple VLANs using IEEE 802.1Q tagging. On the router, subinterfaces are created — each one associated with a VLAN and an IP address that acts as the gateway for that network.

```text
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
```

`encapsulation dot1Q 10` is what actually ties that subinterface to VLAN 10 — without it, the subinterface exists but never processes any tagged traffic.

Multiple VLANs travel over that same physical link:

```text
                 TRUNK
SWITCH ===================== ROUTER
          VLAN 10, 20, 30
```

When the router receives traffic belonging to VLAN 10, the frame arrives tagged with 802.1Q and gets processed by the subinterface associated with VLAN 10. The router routes it toward the destination network, then sends it back out through the subinterface matching the outgoing VLAN:

```text
VLAN 10 (192.168.10.0/24)
      ↓
   ROUTER (routing)
      ↓
VLAN 20 (192.168.20.0/24)
```

The traffic returns to the switch over that same physical link — just tagged as VLAN 20 this time.

### 2. Layer 3 switch (SVIs)

A Layer 3 switch can perform Inter-VLAN routing using **Switch Virtual Interfaces (SVIs)**. Each VLAN that needs routing gets its own SVI:

```text
interface vlan 10
 ip address 192.168.10.1 255.255.255.0

interface vlan 20
 ip address 192.168.20.1 255.255.255.0
```

Then Layer 3 routing is enabled globally:

```text
ip routing
```

Without this command, the SVIs can still hold IP addresses, but the switch won't actually route traffic between them.

Once enabled, the L3 switch uses the SVIs as its Layer 3 interfaces and routes directly between the networks tied to each VLAN:

```text
VLAN 10 (192.168.10.0/24)
      ↓
   SVI VLAN 10
      ↓
   L3 SWITCH (routing)
      ↓
   SVI VLAN 20
      ↓
VLAN 20 (192.168.20.0/24)
```

Unlike Router-on-a-Stick, traffic never has to leave toward an external router to be routed between VLANs — the Layer 3 switch does the routing itself.

## Which one to use

For networks with multiple VLANs and higher performance requirements, an L3 switch with SVIs is usually the better choice, since Router-on-a-Stick funnels all inter-VLAN traffic through a single physical link to the router — a bottleneck that an L3 switch, routing internally at hardware speed, simply doesn't have.
