# OSPF SPF

**Shortest Path First (SPF)** is the process OSPF uses to calculate the shortest paths through the topology represented in its Link-State Database (LSDB), using the **Dijkstra shortest-path algorithm** as its foundation.

The purpose of SPF is to determine the best paths from the local router to the destinations represented in the OSPF topology:

```text
LSAs → LSDB → Topology → SPF Calculation → Shortest Path Tree → Best Paths → Routing Table
```

## LSDB as the input to SPF

OSPF doesn't run SPF directly from the routing table — SPF uses the information contained in the LSDB instead. The LSDB is a collection of LSAs describing the OSPF topology within the router's area and, depending on the route type, information about networks reachable through other areas or external routing domains.

That information includes routers, links, neighbor relationships, link costs, connected networks, external routes, and Router IDs — everything a router needs to construct its own representation of the network topology.

Routers don't receive a ready-made topology map. Instead, they receive LSAs and use the information inside those LSAs to build their own view of the topology.

## Dijkstra's algorithm

Dijkstra's algorithm is a general-purpose shortest-path algorithm, used to calculate minimum-cost paths from a starting node to other nodes in a weighted graph — it isn't an OSPF-specific concept. OSPF simply uses it as the engine behind its own SPF calculation:

```text
Dijkstra → general shortest-path algorithm
SPF      → OSPF's specific shortest-path calculation
```

## SPF root

Each OSPF router performs its own SPF calculation, and becomes the root of its own **Shortest Path Tree (SPT)**.

Even when routers share the exact same LSDB information, the resulting SPF tree is always calculated from the perspective of whichever router is running it.

## Shortest Path Tree and OSPF cost

The Shortest Path Tree (SPT) is the result of the SPF calculation — it represents the shortest known paths from the local router to every other destination in the topology.

OSPF assigns a cost to each interface, and the total cost of a path is simply the sum of the individual link costs along the way. Suppose R1 needs to reach R4:

```text
        R1
       /  \
     10    30
     /      \
   R2        R3
     \       /
      20    5
        \  /
         R4
```

```text
R1 → R2 → R4   =  10 + 20 = 30
R1 → R3 → R4   =  30 + 5  = 35
```

SPF selects R1 → R2 → R4, since its total cost (30) is lower — even though both paths take exactly 2 hops. The number of hops is never the deciding factor in OSPF; accumulated cost always is.

## Equal-cost paths

Sometimes SPF finds multiple paths with the exact same total cost:

```text
R1 ──10── R2 ──10── R4
 │
 └──10── R3 ──10── R4
```

Both paths total 20. When this happens and the platform supports it, OSPF can install both next-hops into the routing table through **Equal-Cost Multi-Path (ECMP)**, using both paths simultaneously instead of picking just one.

## SPF and topology changes

OSPF is a dynamic link-state protocol — when a router detects a topology change (an interface or link going down, for example), it originates updated link-state information, which gets flooded through the appropriate OSPF scope. Other routers receive it, update their LSDBs, and recalculate SPF to find the new shortest paths.

This is one of OSPF's core characteristics: routers never simply exchange routing-table entries with each other. They exchange link-state information, maintain their own LSDB, and independently calculate their own best paths.

## From SPF to the routing table

SPF doesn't create the routing table directly — it determines the shortest paths through the topology, and OSPF uses that result to determine the appropriate routes and next hops:

- RIB (Routing Information Base): the routes actually selected by the router
- FIB (Forwarding Information Base): what the forwarding plane uses to make packet-forwarding decisions

Each of these represents a different stage: the LSDB is topology *knowledge*, the SPT is the *result* of the SPF calculation, the RIB is the *selected* routing information, and the FIB is what's actually used to *forward* packets.

## SPF and OSPF route types

Not every OSPF route is calculated the same way. OSPF distinguishes between Intra-Area, Inter-Area, and External routes.

### Intra-area routes

A destination located within the same OSPF area as the router — calculated directly from the detailed topology in that area's LSDB.

### Inter-area routes

A destination located in another OSPF area. Networks in other areas are advertised via **Summary LSAs (Type 3)**, and the router uses that information to determine how to reach the destination through the appropriate ABR.

### External routes

A route that originates outside the OSPF domain entirely, introduced by an ASBR — represented as **Type 5** (or **Type 7** inside an NSSA). Calculating an external route involves extra information: the external metric, and the path cost to reach the ASBR itself, which is why external route selection shouldn't be treated the same as basic intra-area SPF.

 own LSDBs, independently run SPF, and determine their own best paths based on topology and cost.
