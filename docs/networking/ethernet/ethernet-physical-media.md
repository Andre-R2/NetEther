# Ethernet Cabling

## Copper Ethernet

Copper Ethernet is an implementation of Ethernet that uses copper cables to transmit data between devices on a network. For example, when you connect a PC to a switch using the typical network cable with an **RJ-45** connector, that's copper Ethernet.

It's the most common option in LANs — homes, offices, and small to medium businesses.

Besides being inexpensive, it's easy to install, supports high speeds, and is compatible with nearly all modern network equipment.

It can be affected by electrical interference if the installation isn't done properly, and it has a recommended maximum distance of **100 meters per link**. For longer distances, fiber optic is the better option.

## Fiber Optic

Ethernet can also use fiber optic cables to transmit data between devices.

The difference with copper Ethernet is that fiber transmits data as **pulses of light** through a very thin strand of glass or plastic. This gives it a few key advantages: it doesn't depend on electricity to carry data, it isn't affected by many of the problems that impact copper, and light can travel much longer distances than an electrical signal.

Fiber optic is most commonly found in data centers, ISPs, and large enterprise networks.

It does have some disadvantages: higher cost, more delicate cables, and installations that require more care.

## Connectors

| Connector | Used with | Notes |
|---|---|---|
| **RJ-45** | Copper (UTP/STP) | 8 pins, used with 4 twisted-pair cables. The typical LAN connector — computers, routers, printers, etc. |
| **LC** | Fiber | The most widely used fiber connector. Small and high-density, very common in SFP/SFP+ modules. |
| **SC** | Fiber | Another fiber connector, larger than LC. Simple to connect, widely used in telecom installations. |

## Cable types

Ethernet cables fall into two broad categories: copper cables and fiber cables.

### Copper cables

| Type | Shielding | Notes |
|---|---|---|
| **UTP** | None | The most common type. 4 twisted pairs (8 wires), no metallic shielding. |
| **STP** | Yes | Similar to UTP but includes a metallic shielding layer that reduces electromagnetic interference. More rigid, more expensive, and requires proper grounding to actually benefit from the shielding. |

### Fiber cables

| Type | Core | Range | Cost |
|---|---|---|---|
| **Multimode** | Wider core, light travels through multiple paths | Shorter range | Cheaper |
| **Single-mode** | Thinner core, light travels through a single path | Longer range | More expensive |

## Copper wiring schemes

Copper cables can also be classified by how they're wired internally:

| Scheme | Used to connect | Example |
|---|---|---|
| **Straight-through** | Different types of devices | PC ↔ Switch, Router ↔ Switch |
| **Crossover** | The same type of device | PC ↔ PC, Switch ↔ Switch |
| **Rollover** | Managing a switch/router from a computer | Console port connection |

!!! note
    Most modern switches support **Auto-MDIX**, which automatically detects the cable type and adjusts internally — meaning you can often use a straight-through cable even where a crossover would traditionally have been required.

---

**Next:** IP addressing fundamentals