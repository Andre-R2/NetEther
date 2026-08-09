# EIGRP

## Overview

EIGRP (Enhanced Interior Gateway Routing Protocol) is an advanced distance-vector routing protocol developed by Cisco. It is an Interior Gateway Protocol (IGP) designed to allow routers within an autonomous system to dynamically exchange routing information, select optimal paths, and quickly adapt to topology changes.

Unlike traditional distance-vector protocols such as RIP, EIGRP does not select paths based solely on hop count. Instead, it uses a composite metric based primarily on bandwidth and delay. Reliability and load can also be included in the metric calculation through EIGRP's K-values, although they are disabled by default in typical Cisco configurations.

EIGRP combines characteristics traditionally associated with distance-vector and link-state protocols. It maintains information about neighboring routers and learned routes, but it does not build a complete link-state database of the network. Instead, it relies on the Diffusing Update Algorithm (DUAL) to calculate loop-free paths and maintain efficient routing.

## EIGRP Operation

EIGRP-enabled routers first discover neighboring routers by exchanging Hello packets. When two routers establish the required parameters for EIGRP communication, they form a neighbor relationship and begin exchanging routing information.

EIGRP maintains several tables to organize the information it learns from the network:

- **Neighbor Table** — maintains information about directly connected EIGRP neighbors.
- **Topology Table** — maintains the routes learned through EIGRP and the information required to evaluate available paths.
- **Routing Table** — contains the best routes selected for packet forwarding.

Not every route stored in the EIGRP topology table is installed in the routing table. EIGRP evaluates the available paths and selects the best routes according to its metric and the DUAL algorithm.

This separation allows EIGRP to maintain alternative paths without necessarily installing all of them in the routing table.

## Neighbor Table

The Neighbor Table contains information about EIGRP neighbors with which the router has established an adjacency.

Neighbor relationships are established through EIGRP Hello packets. These packets are also used to maintain the relationship and verify that neighboring routers remain reachable.

A router does not consider every EIGRP-enabled router on the network to be a neighbor. The routers must meet the necessary EIGRP requirements and successfully establish an adjacency.

The Neighbor Table therefore represents the routers from which the local router can directly exchange EIGRP routing information.

## Topology Table

The Topology Table contains routes learned through EIGRP from neighboring routers.

For each destination, EIGRP can maintain information about multiple possible paths. This allows the router to evaluate alternatives rather than relying exclusively on the path currently installed in the routing table.

The topology table therefore contains more routing information than the routing table.

## EIGRP Metric

EIGRP uses a composite metric to evaluate routing paths.

The default metric calculation is primarily based on:

- **Bandwidth**
- **Delay**

EIGRP can also incorporate:

- **Reliability**
- **Load**

These additional components are controlled through EIGRP K-values and are not normally enabled in standard Cisco configurations.

Because EIGRP evaluates more than the number of routers between the source and destination, it can make more informed path-selection decisions than protocols such as RIP.

For example, two paths may contain the same number of hops while having significantly different bandwidth or delay characteristics. EIGRP can distinguish between those paths using its composite metric.

## DUAL

EIGRP uses the Diffusing Update Algorithm (DUAL) to calculate routes and maintain loop-free paths.

DUAL is responsible for selecting the best available path and determining whether an alternate path can be used safely if the current path becomes unavailable.

One of the important characteristics of DUAL is that it can maintain loop-free backup paths. When a valid backup path is already available, EIGRP can switch to that path without having to perform a complete route discovery process.

This contributes to EIGRP's fast convergence.

## Successor

The successor is the neighbor that provides the best path to a particular destination according to EIGRP's route-selection process.

The successor's route is normally installed in the IP routing table and used to forward traffic toward that destination.

A destination can have a successor even when other valid paths are available in the topology table.


## Feasible Successor

A feasible successor is an alternative path to a destination that satisfies EIGRP's feasibility condition.

The feasibility condition allows DUAL to determine whether an alternate route is guaranteed to be loop-free.

A feasible successor is particularly valuable when the current successor becomes unavailable. Because the alternate path has already been determined to be loop-free, EIGRP can use it immediately without first having to discover a new route.

This is one of the mechanisms that contributes to EIGRP's fast convergence.

```
Destination
     │
     ├── Successor
     │      └── Best available path
     │
     └── Feasible Successor
            └── Valid loop-free backup path
```

A route does not automatically become a feasible successor simply because it is the second-best path. It must satisfy EIGRP's feasibility condition.

## Convergence

EIGRP is designed to converge quickly because it maintains information about alternative paths and uses DUAL to determine whether those paths can be used safely.

If a successor fails and a valid feasible successor already exists, EIGRP can immediately use the alternate path.

If no feasible successor exists, EIGRP must perform additional route discovery before selecting a new path.

Therefore, the presence of feasible successors can significantly reduce the time required for EIGRP to recover from certain topology changes.

## EIGRP vs. RIP

EIGRP and RIP are both classified as distance-vector routing protocols, but they differ significantly in how they evaluate and maintain routes.

| Characteristic | RIP | EIGRP |
|---|---|---|
| Routing approach | Distance vector | Advanced distance vector |
| Primary metric | Hop count | Composite metric |
| Maximum hop count | 15 | Not based on hop count |
| Neighbor discovery | Periodic routing updates | Hello packets |
| Routing information | Routing updates | Neighbor and topology information |
| Backup paths | No equivalent to EIGRP feasible successors | Feasible successors |
| Route calculation | Bellman-Ford based | DUAL |
| Convergence | Relatively slow | Generally fast |
| Developed by | IETF standard | Cisco |

The most important difference is not simply that EIGRP has a more complex metric. EIGRP maintains additional path information and uses DUAL to determine whether alternate paths can be used without introducing routing loops.

EIGRP's behavior can therefore be understood as a combination of neighbor discovery, route information maintenance, metric-based path selection, and DUAL-based loop-free route calculation.

