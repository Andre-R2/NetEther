# OSPF Overview

Open Shortest Path First (OSPF) is a dynamic Interior Gateway Protocol (IGP) that uses a link-state routing approach.

Unlike distance-vector protocols such as RIP, OSPF does not determine the best path simply by comparing the distance reported by neighboring routers. Instead, routers running OSPF exchange information about their connected links and use that information to build a view of the network topology within their OSPF area.

Each router stores this information in a **Link-State Database (LSDB)**. Once the database has been built, the router runs the **Shortest Path First (SPF) algorithm**, based on Dijkstra's algorithm, to calculate the shortest paths to the known destinations.

Because OSPF works with link-state information, a router can make routing decisions using a broader view of the network rather than relying only on the information received from a single neighbor.

OSPF uses cost as its routing metric. On Cisco IOS, the default cost is derived primarily from the bandwidth of the interface. This allows OSPF to distinguish between paths based on link characteristics instead of treating every router-to-router hop as having the same cost, as RIP does.

OSPF also supports areas, which provide a hierarchical structure for larger networks. Routers maintain detailed topology information for their own area, while routing between areas is handled according to OSPF's hierarchical design.

Another important difference from RIP is how routing information is exchanged. RIP periodically sends routing updates to its neighbors, while OSPF uses Hello packets to maintain neighbor relationships and distributes link-state information when changes occur in the topology.

These characteristics make OSPF more suitable for larger and more complex networks than traditional distance-vector protocols such as RIP.

