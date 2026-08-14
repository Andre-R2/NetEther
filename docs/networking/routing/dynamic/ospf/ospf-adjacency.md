# OSPF Adjacency

An OSPF adjacency is a relationship between OSPF neighbors that allows them to synchronize their link-state information.

An adjacency cannot be established without a previously established **OSPF neighbor relationship**. Once two routers have become neighbors, they can proceed through the OSPF adjacency formation process.

The adjacency process allows routers to exchange information about their Link-State Databases (LSDBs), determine which link-state information is missing, and synchronize the information required for consistent OSPF operation.

## Adjacency Requirements

### Existing Neighbor Relationship

The routers must first establish an OSPF neighbor relationship.

A router cannot form an OSPF adjacency with another router if the initial neighbor relationship has not been established.

### Compatible MTU

The interfaces must have compatible Maximum Transmission Unit (MTU) values for the adjacency to complete successfully.

During the exchange of **Database Description (DBD)** packets, the routers include MTU information that is used to verify compatibility.

For example:

```
R1 MTU = 1500
R2 MTU = 1500

Compatible
    ↓
EXSTART
    ↓
EXCHANGE
```

With an MTU mismatch:

```
R1 MTU = 1500
R2 MTU = 1400

MTU mismatch
    ↓
Potential problem during adjacency formation
    ↓
EXSTART / EXCHANGE
    ↓
Adjacency may fail to reach FULL
```

An MTU mismatch is therefore a common cause of an OSPF neighbor becoming stuck during adjacency formation.

## Database Description Exchange

During adjacency formation, the routers exchange **Database Description (DBD)** packets.

DBD packets contain summaries of the link-state information known by each router. They do not contain the complete LSAs themselves. Instead, they allow each router to determine which link-state information it already has and which information it needs to request.

## Master and Slave

During the `EXSTART` state, the routers determine which one will act as the **master** and which one will act as the **slave** for the DBD exchange.

The router with the higher Router ID becomes the master.

The master controls the sequencing of the DBD exchange, while the slave responds according to that sequence.

## LSDB Comparison

After exchanging DBD information, each router compares the information described by the neighbor with its own Link-State Database.

For example:

```
R1 LSDB:
A
B
C
D

R2 LSDB:
A
B
C
E
```

R1 determines that it needs information about `E`.

It then sends a **Link-State Request (LSR)** asking the neighbor for the required link-state information.

R2 responds with a **Link-State Update (LSU)** containing the requested LSA.

R1 acknowledges the received information using a **Link-State Acknowledgment (LSAck)**.

The exchange can therefore be summarized as:

```
DBD
 ↓
Determine missing LSAs
 ↓
LSR
 ↓
LSU
 ↓
LSAck
```

This process continues until the routers have synchronized the link-state information required for the adjacency.

## OSPF Neighbor States

OSPF uses a state machine to establish, maintain, and terminate relationships between neighboring routers.

The state of an OSPF neighbor indicates the current stage of the relationship.

The main OSPF states are:

```
Down
  ↓
Init
  ↓
2-Way
  ↓
ExStart
  ↓
Exchange
  ↓
Loading
  ↓
Full
```

Not every OSPF neighbor necessarily progresses to `FULL`. The final state depends on the OSPF network type and the relationship between the routers.

### Down

The `Down` state indicates that no valid OSPF Hello packet has been received from the neighbor within the expected interval.

This is the initial state of an OSPF neighbor relationship.

### Init

The `Init` state indicates that a Hello packet has been received from the neighbor, but the local router's own Router ID has not yet been found in the neighbor's Hello packet.

Communication has been detected, but bidirectional communication has not yet been confirmed.

### 2-Way

The `2-Way` state indicates that bidirectional communication has been established.

The router's own Router ID has been found in the neighbor's Hello packet, confirming that both routers can see each other.

On broadcast and other multiaccess networks, not every neighbor necessarily progresses beyond `2-Way`. Whether a full adjacency is formed depends on the OSPF network type and the DR/BDR relationship.

### ExStart

In the `ExStart` state, routers that will form an adjacency negotiate the master/slave relationship and begin establishing the sequence used for the Database Description exchange.

The router with the higher Router ID becomes the master.

### Exchange

In the `Exchange` state, the routers exchange Database Description (DBD) packets containing summaries of their LSDB information.

Each router compares the information described by the neighbor with its own LSDB to determine which LSAs it needs.

### Loading

In the `Loading` state, the routers request missing or newer LSAs using **Link-State Request (LSR)** packets.

The requested information is sent using **Link-State Update (LSU)** packets and acknowledged using **Link-State Acknowledgment (LSAck)** packets.

This process continues until the required link-state information has been synchronized.

### Full

The `Full` state indicates that the adjacency has been fully established and the relevant link-state information between the routers has been synchronized.

At this point, the routers can use the synchronized LSDB information as part of the OSPF SPF calculation.

The exact progression depends on the OSPF network type. In particular, routers on multiaccess networks may establish a neighbor relationship and remain in `2-Way` without forming a full adjacency with every neighbor.