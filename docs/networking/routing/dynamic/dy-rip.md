
### RIP

Routing Information Protocol (RIP) is a dynamic routing protocol based on the distance-vector routing approach.

Routers running RIP advertise the networks they know and the distance to those networks. RIP uses **hop count** as its routing metric.

### Hop Count

In RIP, the metric represents the number of routers that must be traversed to reach a destination network.

For example:

```
PC1 — R1 — R2 — R3 — PC2
```

If the network containing `PC2` is behind `R3`, then from `R1` that network is two router hops away:

```
R1 → R2 → R3
```

RIP increases the metric as the route is learned through additional routers.

For example, consider the following network:

```
R1 — R2 — R3
          |
    192.168.3.0/24
```

`R3` has `192.168.3.0/24` directly connected. `R3` advertises the network to `R2`, and `R2` learns the destination at a distance of one hop. When `R2` advertises the route to `R1`, `R1` learns that the same destination is two hops away.

The resulting distance can be represented as:

```
R3 → 192.168.3.0/24 = 0 hops
R2 → 192.168.3.0/24 = 1 hop
R1 → 192.168.3.0/24 = 2 hops
```

This behavior illustrates the fundamental idea behind distance-vector routing: routers learn routes from their neighbors and calculate their own distance to each destination based on the information received.

### Distance-Vector Operation

RIP does not require routers to have complete knowledge of the network topology.

Instead, routers exchange information about:

- Destination networks
- Distance to those networks
- The neighboring router through which the destination can be reached

A router therefore does not need to know the complete topology of the network. It only needs information received from its neighboring routers to determine which destinations are reachable and how far away they are.

For example, if `R2` tells `R1` that it can reach `192.168.3.0/24`, `R1` can use that information to determine a route toward the network through `R2`.

This is the fundamental behavior of a distance-vector routing protocol:

```
Neighbor
   ↓
Destination + Distance
   ↓
Local calculation
   ↓
Routing Table
```

### Hop Count Limitation

RIP is not well suited for large networks because of its maximum hop-count limitation.

RIP considers a metric of 1 through 15 to represent reachable destinations. A metric of 16 represents an unreachable destination.

Therefore, a network that requires more than 15 router hops cannot be reached through RIP, even if a physical path technically exists.

This limitation significantly restricts the size and scalability of networks in which RIP can be used effectively.

### Metric Limitations

Another limitation of RIP is that its routing metric does not consider characteristics such as:

- Bandwidth
- Link speed
- Latency
- Reliability
- Traffic load

RIP makes routing decisions based solely on hop count.

As a result, a path with fewer hops can be preferred even when it has significantly worse link characteristics than a longer path.

For example:

```
Path A:
R1 — R2 — R3
2 hops
Low bandwidth

Path B:
R1 — R4 — R5 — R6
3 hops
High bandwidth
```

RIP prefers Path A because it has fewer hops, regardless of the available bandwidth or other link characteristics.

This illustrates one of the fundamental limitations of RIP: its metric provides a simple measurement of path length but does not represent the actual performance or quality of the links along the path.

More advanced routing protocols use more sophisticated metrics or path-selection mechanisms to make routing decisions based on additional information.