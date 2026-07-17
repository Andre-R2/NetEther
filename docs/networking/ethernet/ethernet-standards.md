# Ethernet Standards

## Overview

Ethernet standards are specifications defined by the **IEEE** (Institute of Electrical and Electronics Engineers), mainly under the **IEEE 802.3** family. These standards define how a given Ethernet technology should work — transmission speed, physical medium (copper or fiber), maximum distance, and signaling type.

Thanks to these standards, a Cisco switch can communicate with an HP computer or a Dell server, regardless of manufacturer.

## Naming convention

Most standards follow this format:

```
Speed + BASE + Medium
```

Example: **1000BASE-T**

### Speed

| Code | Speed |
|---|---|
| 10 | 10 Mbps |
| 100 | 100 Mbps |
| 1000 | 1 Gbps |
| 10G | 10 Gbps |
| 40G | 40 Gbps |
| 100G | 100 Gbps |

### BASE

BASE stands for **baseband** — the cable uses its entire bandwidth for a single data signal. Practically every modern Ethernet standard is baseband.

### Physical medium

| Code | Meaning | Typical medium | Range |
|---|---|---|---|
| **T** | Twisted pair | Copper | Short (100 m) |
| **SX** | Short wavelength | Multimode fiber | Short |
| **LX** | Long wavelength | Single-mode fiber (usually) | Longer |
| **SR** | Short reach | Multimode fiber | Short (common at 10 Gbps) |
| **LR** | Long reach | Single-mode fiber | Long-distance |

## Standards 

| Standard | Speed | Medium | Max distance | Connector | Notes |
|---|---|---|---|---|---|
| 10BASE-T | 10 Mbps | Copper (UTP) | 100 m | RJ-45 | |
| 100BASE-TX (Fast Ethernet) | 100 Mbps | Copper | 100 m | RJ-45 | |
| 1000BASE-T (Gigabit Ethernet) | 1 Gbps | Copper | 100 m | RJ-45 | One of the most common standards in current networks |
| 10GBASE-T | 10 Gbps | Copper | 100 m (with adequate cabling, e.g. Cat6A) | RJ-45 | |
| 1000BASE-SX | 1 Gbps | Multimode fiber | Short distances | LC/SC | |
| 1000BASE-LX | 1 Gbps | Single-mode fiber | Longer than SX | LC/SC | Can also run over multimode fiber under certain conditions |
| 10GBASE-SR | 10 Gbps | Multimode fiber | Short links | LC | Common in data centers |
| 10GBASE-LR | 10 Gbps | Single-mode fiber | Long-distance links | LC | |

---

**Next:** IP addressing fundamentals