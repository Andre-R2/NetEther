# OSPF Neighbors

An OSPF Neighbor is another router with which a router has established an OSPF relationship through an interface. Neighbor relationships allow OSPF routers to exchange the information required to build and maintain their routing knowledge.

Two routers being physically connected does not automatically make them OSPF neighbors. Several parameters must be compatible before OSPF can establish a neighbor relationship.

The formation of OSPF neighbors is the first step toward establishing OSPF adjacencies. Once an adjacency is formed, the routers can synchronize their **Link-State Databases (LSDBs)** and exchange link-state information used to calculate the best available paths.

## OSPF Neighbor Requirements

For two routers to establish an OSPF neighbor relationship, the relevant OSPF parameters must be compatible.

### Same Subnet

For the common case of two routers directly connected through an IP network, their OSPF-enabled interfaces must belong to the same IP subnet.

For example:

```text
Router A: 192.168.1.1/24
Router B: 192.168.1.2/24
```

Both interfaces belong to `192.168.1.0/24` and can communicate directly at the IP layer.

### OSPF Enabled

OSPF must be active on both routers, and the interfaces connecting them must participate in OSPF.

If OSPF is not enabled on one of the interfaces, the routers cannot establish an OSPF neighbor relationship through that link.

### Same OSPF Area

The interfaces participating in the neighbor relationship must belong to the same OSPF area.

For example:

```text
Router A → Area 0
Router B → Area 0
```

If one router places the interface in Area 0 while the other places its corresponding interface in Area 1, the OSPF neighbor relationship will not form.

### Compatible Network Type

The routers must use compatible OSPF network types on the link.

For example, both sides may operate as a broadcast network or as a point-to-point network, depending on the type of connection and configuration.

The network type affects how OSPF forms relationships and, in some network types, whether a DR/BDR election takes place.

### Matching Hello and Dead Intervals

The Hello and Dead intervals must match between OSPF neighbors.

A common configuration is:

```text
Hello: 10 seconds
Dead: 40 seconds
```

If one router uses `10/40` while the other uses `5/20`, the OSPF neighbor relationship will not form.

The Hello interval determines how frequently OSPF Hello packets are sent. The Dead interval determines how long a router waits without receiving a Hello packet before considering the neighbor unreachable.

### Compatible Authentication

If OSPF authentication is configured, both routers must use compatible authentication parameters.

An authentication mismatch prevents the routers from successfully establishing an OSPF neighbor relationship.

### OSPF Packets Must Be Reachable

The routers must be able to exchange OSPF packets across the link.

OSPF uses IP protocol 89 rather than TCP or UDP. Therefore, an ACL or firewall that blocks OSPF protocol 89 can prevent the routers from establishing or maintaining a neighbor relationship.

### Unique Router IDs

Each OSPF router must have a unique Router ID within the OSPF routing domain.

Two routers using the same Router ID cannot operate correctly as separate OSPF routers because OSPF would not be able to distinguish them reliably.

When the required parameters are compatible and the routers can exchange OSPF packets, they can begin the OSPF neighbor formation process.
