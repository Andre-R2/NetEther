# Types of networks

## Overview

Networks are classified by their geographic scope — how far they reach and how many devices they're meant to connect. The main types are PAN, LAN, MAN, and WAN.

## PAN (Personal Area Network)

A PAN is a very short-range network designed to connect personal devices belonging to a single user. Examples include the connection between a mobile phone and Bluetooth headphones, a smartwatch, or a wireless keyboard.

## LAN (Local Area Network)

A LAN connects devices within a limited geographic area, such as a home, an office, or a building. It's the most commonly used type of network and allows devices to share resources and communicate with each other easil. A LAN is defined by its local Layer 2 infraestructure. VLANs logically segment that infraestructure into multiple independent Layer 2 networks.

VPNs and VLANs illustrate the difference between a network's physical infraestructure and its logical organization.

A VPN extends secure access to a LAN across a WAN, making remote users appear to belong to the corporate network from a logical perspective. 

VLANs, on the other hand, create multiple independent logical VLANs over the same physical switching infraestructure. Although devices are connected to the same switches and cables, each VLAN forms it own separate Layer 2 broadcast domain.

## MAN (Metropolitan Area Network)

A Metropolitan Area Network is a network that interconnects multiple independent physical Local Area Networks (LANs) distributed across the same metropolitan area. 

Unlike VLANs, which logically segment a single LAN infraestructure into multiple Layer 2 networks, a MAN connects separate physical LAN infraestructures located in different geographical locations. 

A company may have several offices distributed across the same city. Each office has its own local network infraestructure, including switches, VLANs, routers and broadcast domains. A MAN provides the communication infraestructure that interconnects those independent LANs, allowing users and services to communicate between different locations.

VLANs divide one physical LAN into multiple logical LANs, while a MAN connects multiple independent physical LANs together. 

## WAN (Wide Area Network)

A WAN connects networks located in different cities, countries, or continents. Its purpose is to enable communication between networks separated by large distances. The best-known example of a WAN is the Internet.

## Quick recap

| Type | Scope | Typical example |
|---|---|---|
| PAN | A single user's personal devices | Phone ↔ Bluetooth headphones |
| LAN | A home, office, or building | Office network |
| MAN | A city or metropolitan area | University or enterprise campus network |
| WAN | Multiple cities, countries, or continents | The Internet |

