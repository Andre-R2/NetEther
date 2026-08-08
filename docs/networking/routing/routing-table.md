# Routing Table

## Overview

The routing table is a collection of routes used by a router to determine how packets should be forwarded toward their destinations.

Each route can contain several pieces of information:

- Destination Network
- Prefix Length
- Next-Hop
- Outgoing Interface
- Route Source
- Administrative Distance
- Metric

Example:

```
10.10.10.0/24 via 192.168.2.1 GigabitEthernet0/1
```

This means that traffic destined for `10.10.10.0/24` should be forwarded to the next-hop `192.168.2.1` through `GigabitEthernet0/1`.

Cisco IOS displays additional information for routes in the routing table. For example:

```
O    10.10.10.0/24 [110/20] via 192.168.2.1, GigabitEthernet0/1
```

The entry can be interpreted as:

```
O                   → Route source
10.10.10.0/24       → Destination network and prefix length
[110/20]            → [Administrative Distance / Metric]
192.168.2.1         → Next-hop
GigabitEthernet0/1  → Outgoing interface
```

In this example, `O` identifies OSPF as the route source, `110` is the Administrative Distance, and `20` is the OSPF metric.

## How Routes Get into the Table

Routes can enter the routing table through different sources, including directly connected networks, static configuration, and dynamic routing protocols.

## Directly Connected

When you configure an IP address on an interface, the router automatically creates a route for the directly connected network.

```
interface Gi0/0
 ip address 192.168.1.1 255.255.255.0
```

The router automatically knows that `192.168.1.0/24` is directly connected through `Gi0/0`.

```
192.168.1.0/24    Connected
```

Cisco IOS also creates a local route for the interface's own IP address:

```
192.168.1.0/24    Connected
192.168.1.1/32    Local
```

The connected route represents the directly attached network, while the local route represents the router's own interface address.

## Static

Routes can also be configured manually:

```
ip route 10.10.10.0 255.255.255.0 192.168.2.1
```

The router then has a route toward `10.10.10.0/24` using `192.168.2.1` as the next-hop:

```
10.10.10.0/24
    via 192.168.2.1
```

## Dynamic Routing Protocols

Routes can also be learned through dynamic routing protocols such as **OSPF**, **EIGRP**, **RIP**, **IS-IS**, and **BGP**.

These protocols allow routers to exchange routing information and dynamically determine paths to remote networks.

When multiple routing sources provide a route to the same destination:

```
10.10.10.0/24 → OSPF
10.10.10.0/24 → EIGRP
10.10.10.0/24 → Static
```

The router must determine which route should be preferred.

## Choosing the Best Route

Route selection is based on the destination prefix and, when multiple routes exist for the same destination prefix, the preference of the routing source and the protocol-specific metric.

## Longest Prefix Match

Longest Prefix Match (LPM) determines which route most specifically matches the destination address.

Suppose the routing table has:

```
10.0.0.0/8        Gi0/0
10.10.0.0/16      Gi0/1
10.10.10.0/24     Gi0/2
0.0.0.0/0         Gi0/3
```

A packet destined for `10.10.10.50` matches all four prefixes:

```
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
0.0.0.0/0
```

The router selects `10.10.10.0/24` because it has the longest and therefore most specific matching prefix.

Longest Prefix Match determines which destination prefix applies to the packet. Administrative Distance and metric are relevant when multiple routes exist for that same destination prefix.

## Administrative Distance

Administrative Distance (AD) determines which routing source is preferred when multiple sources provide routes to the same destination prefix.

A lower Administrative Distance is preferred.

| Source    | AD |
| --------- | --: |
| Connected | 0 |
| Static    | 1 |
| eBGP      | 20 |
| EIGRP     | 90 |
| OSPF      | 110 |
| IS-IS     | 115 |
| RIP       | 120 |
| iBGP      | 200 |

For example:

| Source | Route         | AD | Metric |
| ------ | ------------- | --: | -----: |
| Static | 10.10.10.0/24 | **1** | — |
| EIGRP  | 10.10.10.0/24 | **90** | **25600** |
| OSPF   | 10.10.10.0/24 | **110** | **20** |
| RIP    | 10.10.10.0/24 | **120** | **2** |

Static routing is preferred because it has the lowest Administrative Distance, even though RIP has a lower metric.

Administrative Distance is used to compare routes from different routing sources. When multiple routes are learned from the same routing protocol, the protocol's metric is used to determine the preferred path.

## Metric

A metric is a value used by a routing protocol to evaluate available paths to a destination.

The meaning and calculation of the metric depend on the routing protocol. For example:

- **OSPF** uses cost.
- **EIGRP** uses a composite metric.
- **RIP** uses hop count.

When multiple routes to the same destination are learned through the same routing protocol, the protocol uses its metric to select the preferred path.

## Default Route

If no more specific route matches the destination, the default route is used.

For IPv4, the default route is:

```
0.0.0.0/0
```

Example:

```
0.0.0.0/0    192.168.1.254
```

This means that traffic for which no more specific route exists is forwarded toward `192.168.1.254`.

Because `0.0.0.0/0` has a prefix length of `/0`, it is less specific than every other IPv4 network prefix and therefore acts as the route of last resort.