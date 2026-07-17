# Power over Ethernet (PoE)

## Overview

**PoE (Power over Ethernet)** is a technology that allows data and electrical power to be transmitted over the same Ethernet cable.

A device without PoE would need one connection for data and a separate one for power. A **PoE switch** or a **PoE injector** supplies electrical power through the same UTP cable that carries the data — both share the same cable without interfering with each other.

PoE is ideal for devices installed where there's no nearby power outlet, such as **IP phones**, **access points**, and **IP security cameras**.

## How PoE is delivered

PoE can be supplied in two ways:

- **PoE switch** — the switch delivers power directly through the Ethernet cable.
- **PoE injector** — if the switch doesn't support PoE, an injector can be placed between the switch and the device to supply the power instead.

## PoE standards

| Standard | Name | Max power at the port |
|---|---|---|
| 802.3af | PoE | 15.4 W |
| 802.3at | PoE+ | 30 W |
| 802.3bt | PoE++ | 60 W – 90 W |

Although a switch has multiple ports, not all of them can deliver their maximum power simultaneously. A 24-port PoE switch, for example, typically has a total power budget of around **370 W** — shared across all connected devices.

