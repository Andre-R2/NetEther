# Autonomous Systems

An Autonomous System (AS) is a collection of networks and routers under the control of a single organization or administrative authority. The organization operates the networks according to a common routing policy.

An AS can represent an Internet Service Provider (ISP), a large enterprise, a cloud provider, or another organization that manages its own routing domain.

Each Autonomous System is identified by an **Autonomous System Number (ASN)**, which is used to identify the AS when exchanging routing information with other autonomous systems.

For example:

```text
AS100 → ISP A
AS200 → ISP B
AS300 → ISP C
```

### Why Autonomous Systems Exist

The Internet is composed of a large number of independent networks operated by different organizations. It would not be practical for every router on the Internet to maintain detailed knowledge of the complete global topology.

An internal routing protocol such as OSPF is designed to maintain detailed information about the topology within a routing domain. Applying the same model to the entire Internet would require routers to maintain and process an enormous amount of topology information.

Autonomous Systems provide a way to divide the Internet into independent routing domains.

Each organization can manage its own internal network and choose the routing protocols and policies appropriate for its environment. Routers within an organization therefore do not need to maintain a detailed view of the internal topology of every other organization connected to the Internet.

Within each AS, the organization can use an Interior Gateway Protocol (IGP) to exchange routing information between its internal routers.

Between different ASes, routing information is exchanged using an Exterior Gateway Protocol (EGP).

## Interior Gateway Protocols

An Interior Gateway Protocol (IGP) is a routing protocol designed to exchange routing information within an Autonomous System.

The organization operating the AS can choose the IGP that best fits its network requirements.

Examples of IGPs include:

- OSPF
- EIGRP
- RIP
- IS-IS

## Exterior Gateway Protocols

An Exterior Gateway Protocol (EGP) is used to exchange routing information between different Autonomous Systems.

For example:

The routers participating in the interconnection can exchange information about which networks are reachable through each AS.

The protocol used for modern inter-AS routing is Border Gateway Protocol (BGP).

BGP allows autonomous systems to exchange routing information while applying routing policies that determine which paths should be preferred or advertised.

Autonomous Systems therefore provide the administrative and routing boundaries that allow the global Internet to operate as an interconnected collection of independent networks rather than as a single routing domain.