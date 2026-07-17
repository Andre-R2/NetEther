# Ethernet

## Overview

Ethernet is the most widely used **Local Area Network (LAN)** technology in the world. It defines how devices communicate within a local network by specifying how data is formatted into frames, how devices are identified using **MAC addresses**, and how information is transmitted over the physical medium.

Standardized under **IEEE 802.3**, Ethernet enables interoperability between network devices from different manufacturers, making it the foundation of modern wired networks.

## Why Ethernet became the standard

Ethernet was designed to provide a simple, reliable, and standardized method for connecting devices within a LAN. Over time, its low cost, scalability, and continuous evolution allowed it to replace older technologies such as **Token Ring**, **ARCNET**, and **FDDI**, becoming the dominant networking standard worldwide.

Today, Ethernet supports a wide range of transmission speeds, physical media, and network topologies while maintaining backward compatibility across generations.

## Where Ethernet operates

Ethernet operates across the first two layers of the TCP/IP networking stack:

- **Physical Layer (Layer 1):** Defines how bits are transmitted over the physical medium using electrical or optical signals.
- **Network Access Layer (Layer 2):** Defines how devices exchange frames within the same local network using MAC addresses.

## How Ethernet handles data

Ethernet encapsulates data into **frames**, the Protocol Data Unit (PDU) of the Data Link Layer.

Each Ethernet frame contains:

- Source MAC Address
- Destination MAC Address
- EtherType / Length
- Payload
- Frame Check Sequence (FCS)

The frame is then transmitted over the physical medium, where the Physical Layer converts it into electrical or optical signals for delivery to the next device.

## Why Ethernet matters

Without Ethernet, devices connected to the same local network would have no standardized way to exchange data, identify one another, or reliably transmit information across a shared physical medium.

Ethernet provides the foundation upon which technologies such as **ARP**, **switching**, **VLANs**, and many other Layer 2 protocols operate.

---

**Next:** MAC Address