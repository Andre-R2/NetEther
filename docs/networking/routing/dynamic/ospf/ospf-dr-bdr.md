# OSPF DR/BDR

In OSPF, a **Designated Router (DR)** and a **Backup Designated Router (BDR)** are used on multiaccess networks to reduce the number of full adjacencies required between OSPF routers.

## Why DR and BDR Are Needed

On a broadcast multiaccess network, multiple OSPF routers can share the same Layer 2 segment.
All routers belong to the same IP subnet and can discover each other as OSPF neighbors.

Without a DR, every router could potentially establish a full adjacency with every other router:

```text
R1 ↔ R2
R1 ↔ R3
R1 ↔ R4
R1 ↔ R5
R2 ↔ R3
R2 ↔ R4
R2 ↔ R5
R3 ↔ R4
R3 ↔ R5
R4 ↔ R5
```

The number of possible adjacencies increases rapidly as the number of routers increases.
The number of possible point-to-point relationships in a fully meshed network is:

```text
n(n - 1) / 2
```

For example:

```text
5 routers  → 10 possible adjacencies
10 routers → 45 possible adjacencies
20 routers → 190 possible adjacencies
```

Maintaining full adjacencies between every router would create unnecessary control-plane overhead.
OSPF solves this problem by introducing a Designated Router and a Backup Designated Router.

## DR and BDR Roles

The Designated Router (DR) acts as the central OSPF coordination point for the multiaccess segment.
The Backup Designated Router (BDR) provides redundancy and is prepared to assume the DR role if the current DR fails.
Routers that are neither the DR nor the BDR are commonly referred to as **DROthers**.
The DR and BDR are OSPF control-plane roles. 

They do not function as gateways for user traffic, and user packets do not need to pass through the DR.
The DR does not eliminate OSPF neighbor relationships.
Routers on the same multiaccess segment can still discover each other using OSPF Hello packets.
However, not every neighbor relationship needs to become a full adjacency.

For example:

```text
DR ↔ DROther     FULL
DR ↔ DROther     FULL
DR ↔ BDR         FULL

DROther ↔ DROther     2-WAY
```

This significantly reduces the number of full adjacencies required.

The important distinction is:

- Neighbor relationship — Routers can discover and communicate with each other through OSPF.
- Full adjacency — Routers synchronize the link-state information required for an OSPF adjacency.

Therefore, two DROther routers can remain in the `2-WAY` state without establishing a full adjacency with each other.

## How LSA Flooding Works

When a DROther detects a topology change, it originates the appropriate LSA.

The DR/BDR mechanism organizes the exchange of link-state information within the multiaccess segment without requiring every DROther to maintain a full adjacency with every other DROther.

Conceptually:

```text
DROther
   ↓
New LSA
   ↓
DR / BDR mechanism
   ↓
Other OSPF routers
   ↓
LSDB updated
```

The DR does not maintain a single master LSDB for the entire segment. Every OSPF router maintains its own LSDB.

The DR is therefore not a centralized database or a routing gateway. Its role is related to the organization of OSPF control-plane communication on the multiaccess segment.

## DR Election

The DR and BDR are selected through an OSPF election process.

The primary election criterion is the **OSPF interface priority**.

The router with the highest eligible priority becomes the DR.

The router with the second-highest eligible priority becomes the BDR.

For example:

```text
R1 → Priority 1
R2 → Priority 100
R3 → Priority 50
R4 → Priority 1
```

The result is:

```text
R2 → DR
R3 → BDR
R1 → DROther
R4 → DROther
```

### Priority 0

An interface configured with an OSPF priority of `0` cannot become the DR or BDR.

For example:

```text
R1 → Priority 100
R2 → Priority 50
R3 → Priority 0
R4 → Priority 0
```

The result is:

```text
R1 → DR
R2 → BDR
R3 → DROther
R4 → DROther
```

R3 and R4 can still participate in OSPF on the segment, but they are not eligible for DR/BDR election.

## Router ID as a Tie-Breaker

If multiple routers have the same OSPF interface priority, the **highest OSPF Router ID** is preferred.

For example:

```text
R1 → Priority 1 → RID 1.1.1.1
R2 → Priority 1 → RID 2.2.2.2
R3 → Priority 1 → RID 3.3.3.3
R4 → Priority 1 → RID 4.4.4.4
```

Because all routers have the same priority:

```text
4.4.4.4 → DR
3.3.3.3 → BDR
2.2.2.2 → DROther
1.1.1.1 → DROther
```

## DR Election Is Not Preemptive

OSPF does not continuously replace the current DR simply because another router has a higher priority or Router ID.

For example:

```text
R1 → DR
R2 → BDR
```

If a new router joins the segment:

```text
R3 → Priority 255
```

R3 does not automatically replace R1 as the DR simply because it has a higher priority.

This behavior prevents unnecessary changes to the DR role and the associated OSPF relationships.

## DR Failure

The BDR provides redundancy for the DR.

Suppose:

```text
R1 → DR
R2 → BDR
R3 → DROther
R4 → DROther
```

If R1 fails:

```text
R1 → DR
     ↓
   Failure
     ↓
R2 → becomes DR
```

The remaining routers then select a new BDR.

The BDR therefore reduces the disruption caused by the failure of the current DR.

## Relationship with Type 2 LSA

DR/BDR operation is closely related to the OSPF Type 2 Network LSA.

On a broadcast multiaccess network, the DR originates the Type 2 LSA representing the multiaccess segment.

The Type 2 LSA describes the shared network and the OSPF routers attached to it.

This allows OSPF to represent the multiaccess segment efficiently in the link-state topology rather than treating every router as if it had an independent point-to-point connection to every other router.

The DR therefore has two closely related responsibilities:

- Coordinates OSPF control-plane communication on the multiaccess segment

- Originates the Type 2 Network LSA
