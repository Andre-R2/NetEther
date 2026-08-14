# Link-State Advertisement (LSA)

## Overview

A Link-State Advertisement (LSA) is a data structure used by OSPF to describe information about the topology of an OSPF network.

LSAs contain information about routers, links, networks, and the relationships between them. OSPF routers use this information to build and maintain their **Link-State Database (LSDB)** — a collection of LSAs known by a router within its OSPF area, providing the topology knowledge the **Shortest Path First (SPF)** algorithm needs to calculate the best paths.

An LSA doesn't simply represent a route. Instead, it describes link-state information that OSPF uses to understand how routers and networks are actually connected.

## LSA fields

LSAs contain information such as router and link data, connected networks, link costs, neighbor relationships, Router IDs, and external routing information — along with header fields that let routers identify, compare, validate, and manage LSAs during synchronization and flooding.

| Field | Purpose |
|---|---|
| LS Age | Indicates the age of the LSA |
| LS Options | Contains OSPF capability and option information |
| LS Type | Identifies the type of LSA |
| Link State ID | Identifies the link-state information represented by the LSA |
| Advertising Router | Identifies the router that originated the LSA |
| LS Sequence Number | Identifies the version of the LSA |
| LS Checksum | Used to verify the integrity of the LSA |
| Length | Indicates the size of the LSA |

The **LS Sequence Number** matters the most in practice — it's what routers use to determine which version of an LSA is newer when multiple versions are floating around.

## LSA types

| Type | Name |
|---|---|
| 1 | Router LSA |
| 2 | Network LSA |
| 3 | Summary LSA |
| 4 | ASBR Summary LSA |
| 5 | AS External LSA |
| 7 | NSSA External LSA |

### Type 1 — Router LSA

Originated by every OSPF router within an area. Its job is to describe the router's own OSPF links, interfaces, connected networks, neighboring routers, and their associated link costs — providing the information needed to represent that router within its area's topology.

### Type 2 — Network LSA

Originated by the **Designated Router (DR)** on a multiaccess network. It describes the multiaccess segment and the OSPF routers connected to it, representing that segment in the topology without requiring a full adjacency between every pair of routers on it — which reduces the number of adjacencies needed on multiaccess networks.

### Type 3 — Summary LSA

Originated by an **Area Border Router (ABR)** to advertise networks from one OSPF area into another. This lets routers in one area learn about networks in other areas without needing the full topology of those areas in their LSDB — Type 3 LSAs provide inter-area routing information.

### Type 4 — ASBR Summary LSA

Also originated by an ABR, but with a narrower job: providing reachability information for an **Autonomous System Boundary Router (ASBR)** located in another area. It doesn't advertise the external network itself — just how to reach the ASBR that's injecting those external routes into OSPF.

### Type 5 — AS External LSA

Originated by an **ASBR** to advertise external routes into the OSPF domain — routes that can come from sources outside OSPF entirely, like static routes, BGP, or another routing protocol. The ASBR redistributes that external information into OSPF, and Type 5 LSAs carry it throughout the domain, subject to the rules of the area type.

### Type 7 — NSSA External LSA

Used to represent external routes inside a **Not-So-Stubby Area (NSSA)**. An ASBR inside an NSSA originates Type 7 LSAs to advertise external routes into that area, and an ABR can translate Type 7 into Type 5 when those routes need to reach the rest of the OSPF domain.

## LSA flooding

When a router originates an LSA, it doesn't just send it to one neighbor — OSPF floods it throughout its appropriate scope.

When a router receives an LSA, it compares it against the version already in its LSDB, using the LS Sequence Number, LS Age, and LS Checksum to determine whether the received copy is newer or otherwise relevant:

```text
Receive LSA
    ↓
Compare with existing LSA
    ↓
Determine whether the received LSA is newer
    ↓
Install or update LSDB if required
    ↓
Flood to appropriate interfaces
    ↓
Acknowledge with LSAck
```

If the received LSA is already known and doesn't contain newer information, the router doesn't need to replace its existing copy. The **Link-State Acknowledgment (LSAck)** packet confirms receipt of LSAs during this flooding process — and through this mechanism, every router within the appropriate OSPF scope ends up with a consistent view of the topology in its LSDB.
