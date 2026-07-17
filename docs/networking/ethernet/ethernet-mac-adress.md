# MAC Address

## Overview

A **MAC (Media Access Control) Address** is a **48-bit identifier** assigned by the manufacturer to a **Network Interface Card (NIC)**. Its purpose is to uniquely identify a network interface within a Layer 2 network.

It's important to note that a MAC address **does not identify the entire device**, but rather each individual network interface. For example, a server with four network interfaces will have four different MAC addresses.

A MAC address is programmed into the network interface during manufacturing and, under normal conditions, remains constant throughout its lifetime. However, many operating systems allow it to be temporarily modified through software, a technique known as **MAC spoofing**.

Within a **Local Area Network (LAN)**, switches make forwarding decisions based on **MAC addresses**. When a switch receives an Ethernet frame, it examines the destination MAC address and forwards the frame through the appropriate port toward the destination device.

Without MAC addresses, switches would have no way to determine where a frame should be delivered, making efficient communication within a LAN impossible.

## MAC addresses vs IP addresses

MAC addresses play a role similar to IP addresses, but at a different layer of the network:

- **IP addresses** identify hosts across multiple interconnected networks and are used for routing.
- **MAC addresses** identify network interfaces within the local network and allow switches to forward Ethernet frames correctly.

## Structure of a MAC address

![mac](../../assets/net-assets/mac-structure.svg)


A MAC address consists of **48 bits (6 bytes)**, represented as **six hexadecimal octets**, for example:

```text
3C:52:82:34:4D:32
```

The address is divided into two parts:

### Organizationally Unique Identifier (OUI)

The **first 24 bits (3 octets)** correspond to the **Organizationally Unique Identifier (OUI)**, a code assigned by the **IEEE** to each hardware manufacturer. This allows the manufacturer of a network interface to be identified.

```text
3C:52:82
```

### Device identifier

The **last 24 bits (3 octets)** are assigned by the manufacturer and uniquely identify that specific network interface.

```text
34:4D:32    
```

Together, both fields make each MAC address globally unique.

## What would happen without MAC addresses?

Without MAC addresses, switches would not be able to identify the source or destination of Ethernet frames. As a result, they would have no reliable way to determine through which port a frame should be forwarded, making communication within a local network inefficient or even impossible.

---

**Next:** IP addressing fundamentals