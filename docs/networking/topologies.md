# Network topology architectures

*CCNA blueprint 1.2*

Before you can talk about routing or switching, you need to know **how networks are physically and logically shaped**. This is CCNA exam topic 1.2 — it's all "describe" level, so no configs here, just concepts you need to explain clearly.

## 2-tier (collapsed core)

Two layers: **core/distribution** (combined) and **access**. Used in smaller networks where a dedicated distribution layer isn't worth the cost.

- Access layer: where endpoints connect (switches with PoE, port security, etc.)
- Core layer: does the routing/switching between VLANs and out to the WAN

**My lab:** built this in Packet Tracer with 2 access switches uplinked to a single L3 core switch.

![2-tier topology in Packet Tracer](../assets/networking/2-tier-topology.png)

!!! note "What tripped me up"
    I kept forgetting the core switch needs `ip routing` enabled before inter-VLAN routing works on the SVIs — spent 20 minutes debugging a "connected but no ping" issue because of this.

## 3-tier (core / distribution / access)

Adds a dedicated **distribution layer** between core and access. Used in larger networks.

- **Access:** endpoint connectivity
- **Distribution:** aggregates access switches, applies policies (ACLs, QoS), handles inter-VLAN routing
- **Core:** high-speed backbone, moves traffic fast, minimal policy processing

Rule of thumb for the exam: *distribution = policy, core = speed*.

## Spine-leaf

Used in **data centers**. Every leaf switch connects to every spine switch (full mesh between the two layers), and endpoints only ever connect to leaf switches.

- Predictable latency: any leaf-to-leaf path is always exactly 2 hops (leaf → spine → leaf)
- No leaf-to-leaf or spine-to-spine direct links
- Scales horizontally: need more capacity? Add another spine.

## WAN topologies

- **Hub-and-spoke:** central site connects to multiple branch sites; branches don't talk directly to each other. Cheaper, but the hub is a single point of failure.
- **Full mesh:** every site connects to every other site directly. Best redundancy, most expensive (link count grows fast: `n(n-1)/2`).
- **Partial mesh:** middle ground, only some sites have direct links.

## SOHO (Small Office/Home Office)

Simple setup: one multi-function device (router + switch + AP + firewall, all-in-one) connecting a handful of devices to the internet. Think of a typical home router.

## On-premises vs cloud

- **On-premises:** you own and manage the physical infrastructure
- **Cloud:** infrastructure hosted by a provider (AWS, Azure, GCP), you rent capacity instead of owning hardware
- **Hybrid:** mix of both — common in real companies, e.g. sensitive data on-prem, everything else in the cloud

## Quick recap

| Topology | Best for | Key trait |
|---|---|---|
| 2-tier | Small networks | Core + access combined |
| 3-tier | Large enterprise | Dedicated distribution layer |
| Spine-leaf | Data centers | Predictable 2-hop latency |
| Hub-and-spoke | WAN, cost-sensitive | Single point of failure at hub |
| Full mesh | WAN, high redundancy | Expensive, scales poorly |
| SOHO | Home/small office | All-in-one device |

---

*Next up: 1.3 — comparing physical interface and cabling types.*