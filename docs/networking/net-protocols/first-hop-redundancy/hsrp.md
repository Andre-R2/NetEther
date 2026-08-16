# HSRP

HSRP (Hot Standby Router Protocol) is a Cisco proprietary First-Hop Redundancy Protocol (FHRP) that provides gateway redundancy for IPv4 networks.

Its main objective is to eliminate the default gateway as a single point of failure.

Hosts typically use a single default gateway to reach destinations outside their local subnet. If that gateway becomes unavailable, the hosts may lose connectivity to other networks even if the rest of the network is still operational.

HSRP solves this problem by providing a virtual gateway shared by multiple Layer 3 devices.

## Basic Operation

HSRP creates a group of routers or Layer 3 devices that cooperate to provide a single logical default gateway.

Each device participating in the HSRP group has its own physical IP address. The group also has a separate virtual IP address that represents the gateway.

For example:

```text
Router 1
Physical IP: 172.16.20.2

Router 2
Physical IP: 172.16.20.3

HSRP Virtual IP:
172.16.20.1
```

From the endpoint's perspective, the default gateway is always:

```text
172.16.20.1
```

The endpoint does not need to know which physical router is currently forwarding the traffic.

One device assumes the Active role, while another assumes the Standby role.

The Active device is responsible for forwarding traffic destined for the virtual gateway. The Standby device monitors the Active device and is prepared to take over if the Active device becomes unavailable.

If the Active device fails, the Standby device can transition to the Active role.

## Physical IP vs. Virtual IP

Each HSRP device has its own physical IP address.

The HSRP group also has a virtual IP address.

These addresses serve different purposes:

```text
Router 1
Physical IP → 172.16.20.2

Router 2
Physical IP → 172.16.20.3

HSRP Group
Virtual IP → 172.16.20.1
```

The physical IP identifies the individual Layer 3 device.

The virtual IP represents the logical default gateway shared by the HSRP group.

The hosts use the virtual IP as their default gateway rather than the physical IP of either router.

## Virtual MAC Address

HSRP also provides a virtual MAC address associated with the virtual gateway.

This is important because Ethernet communication requires both an IP destination and a destination MAC address.

When an endpoint needs to send traffic to another network, it determines that the destination is remote and forwards the packet to its default gateway.

The endpoint uses ARP to resolve the gateway's IP address to a MAC address.

With HSRP, the gateway IP is the HSRP virtual IP, and the corresponding destination MAC is the HSRP virtual MAC.

Therefore, the endpoint effectively communicates with a logical gateway rather than with a specific physical router.

The important concept is that the endpoint remains unaware of which physical HSRP device is currently Active.

If the Active device fails, the Standby device assumes the Active role while continuing to represent the same logical gateway.

> HSRP does not provide two default gateways to the host. It provides one logical default gateway backed by multiple physical Layer 3 devices.

The endpoint does not think in terms of Router 1 and Router 2 as separate gateways. It simply uses the HSRP virtual IP as its default gateway.

```text
Default Gateway → 172.16.20.1
```

Behind that virtual gateway, multiple Layer 3 devices coordinate to provide gateway redundancy.
