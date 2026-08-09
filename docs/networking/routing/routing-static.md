# Static Routing

## Overview

Static routing is a routing method in which routes are manually configured by an administrator rather than being learned through a dynamic routing protocol.

A static route defines a specific path that a router can use to reach a destination network. The route can specify a next-hop address, an outgoing interface, or both.

Static routing is commonly used in small networks, stub networks, default paths, and situations where precise control over routing behavior is required.

Unlike dynamic routing, static routes do not automatically adapt to topology changes. If the specified path becomes unavailable, the route generally remains configured until the administrator changes or removes it, unless additional mechanisms are used to provide an alternative path.

## Static Route Configuration

On Cisco IOS, a static IPv4 route is configured with the `ip route` command:

```
ip route <destination-network> <subnet-mask> <next-hop>
```

For example:

```
ip route 10.10.10.0 255.255.255.0 192.168.2.2
```

This creates a route toward `10.10.10.0/24` using `192.168.2.2` as the next-hop.

The resulting route can appear in the routing table as:

```
S    10.10.10.0/24 [1/0] via 192.168.2.2
```

The `S` identifies the route as static. The value `1` represents the default Administrative Distance of a static route, while `0` represents its metric in this example.

The destination network and subnet mask identify the network being reached, while the next-hop identifies the neighboring router to which the packet should be forwarded.

## Next-Hop Static Routes

A next-hop static route specifies the IP address of the neighboring router that should receive the packet.

Example:

```
ip route 10.10.10.0 255.255.255.0 192.168.2.2
```

The router determines that traffic destined for `10.10.10.0/24` should be forwarded toward `192.168.2.2`.

The next-hop address must be reachable through one of the router's connected networks. The router may therefore need to perform a recursive lookup to determine the outgoing interface toward the next-hop.

## Exit-Interface Static Routes

A static route can specify the outgoing interface instead of a next-hop address.

Example:

```
ip route 10.10.10.0 255.255.255.0 GigabitEthernet0/1
```

This tells the router to forward traffic for `10.10.10.0/24` through `GigabitEthernet0/1`.

Exit-interface static routes are particularly straightforward on point-to-point interfaces, where there is only one possible neighboring device on the link.

On multi-access networks such as Ethernet, specifying only an exit interface can require additional Layer 2 resolution to determine the destination MAC address.

## Fully Specified Static Routes

A fully specified static route identifies both the outgoing interface and the next-hop address.

Example:

```
ip route 10.10.10.0 255.255.255.0 GigabitEthernet0/1 192.168.2.2
```

This provides the router with both pieces of forwarding information:

- The outgoing interface is `GigabitEthernet0/1`.
- The next-hop is `192.168.2.2`.

Fully specified routes can be useful when the forwarding path needs to explicitly identify both the Layer 3 next-hop and the Layer 3 interface through which that next-hop is reached.

## Recursive Lookup

When a static route specifies a next-hop address, the router must determine how to reach that next-hop.

For example:

```
ip route 10.10.10.0 255.255.255.0 192.168.2.2
```

The router first identifies `192.168.2.2` as the next-hop for the destination `10.10.10.0/24`.

It then examines its routing information to determine which interface can reach `192.168.2.2`.

For example:

```
192.168.2.0/24 → GigabitEthernet0/1
```

The router can therefore forward traffic toward `10.10.10.0/24` through `GigabitEthernet0/1` and the next-hop `192.168.2.2`.

This process is known as a **recursive lookup** because the router must resolve the next-hop using another route before it can determine the complete forwarding path.

## Default Static Routes

A default static route is a static route that matches destinations for which no more specific route exists.

The IPv4 default route is:

```
0.0.0.0/0
```

It can be configured as:

```
ip route 0.0.0.0 0.0.0.0 192.168.1.254
```

This creates a static default route toward `192.168.1.254`.

A default static route is commonly used when a router has a single path toward external networks, such as an Internet connection or an upstream router.

Because `0.0.0.0/0` is less specific than all other IPv4 prefixes, it is used only when no more specific matching route exists.

## Floating Static Routes

A floating static route is a static route configured with a higher Administrative Distance than another preferred route.

Its purpose is to provide a backup path that remains inactive while a preferred route is available.

For example, suppose a router already learns a destination through OSPF:

```
O    10.10.10.0/24 [110/20] via 192.168.2.2
```

A static route can be configured with an Administrative Distance greater than OSPF:

```
ip route 10.10.10.0 255.255.255.0 192.168.3.2 200
```

The final value, `200`, sets the Administrative Distance of the static route.

Because the static route has an AD of `200`, OSPF with an AD of `110` is preferred while the OSPF route is available.

If the OSPF route is removed from the routing table, the floating static route can become the preferred available route.

This makes floating static routes useful for providing manually configured backup paths.

## Static Route Administrative Distance

Static routes have a default Administrative Distance of `1`.

Because lower Administrative Distance values are preferred, a default static route normally has a higher preference than routes learned through most dynamic routing protocols.

For example:

```
Static    → AD 1
EIGRP     → AD 90
OSPF      → AD 110
RIP       → AD 120
```

Administrative Distance can be manually modified when a different route preference is required.

For example:

```
ip route 10.10.10.0 255.255.255.0 192.168.3.2 200
```

This creates a static route with an Administrative Distance of `200`, allowing it to function as a backup route rather than the primary route.

## Verification

Static routes can be verified using the routing table:

```
show ip route
```

A specific destination can also be examined:

```
show ip route 10.10.10.0
```

The configured static routes can be viewed in the running configuration:

```
show running-config | include ip route
```

The routing table should be used to verify whether the route is actually installed and available for forwarding, while the running configuration confirms how the static route was configured.

A static route may exist in the configuration without being installed in the routing table if its next-hop or outgoing interface cannot be resolved.

