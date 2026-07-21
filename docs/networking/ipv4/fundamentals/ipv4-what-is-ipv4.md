# What is IPv4?

## Overview

Internet Protocol version 4 (IPv4) is a Layer 3 protocol that provides logical addressing and enables packets to be routed across interconnected networks.

Unlike Layer 2 technologies such as Ethernet, which only operate within a local network, IPv4 allows devices to communicate across multiple interconnected networks, making large-scale internetworking possible.

## Why was IPv4 created?

IPv4 was created to solve one of the biggest challenges in networking: connecting independent networks into a single global internetwork.

Its primary purpose is to provide a standardized method for identifying devices logically and enabling routers to forward packets between different networks, regardless of the underlying hardware or communication technology.

This standardization allowed networks built by different manufacturers to communicate using a common protocol, laying the foundation for the modern Internet.

## Why aren't MAC addresses enough?

MAC addresses alone are not sufficient for communication beyond a local network.

A MAC address identifies a network interface at Layer 2 and is only meaningful within the local broadcast domain. Switches use MAC addresses to forward Ethernet frames, but routers do not use them to determine paths across multiple networks.

IPv4 introduces hierarchical logical addressing, allowing routers to identify both the destination network and the destination host. This makes scalable routing across interconnected networks possible.

## What problem did IPv4 solve?

Before IPv4, early networks lacked a universal protocol capable of interconnecting different networks.

One of the protocols used at the time was the **Network Control Program (NCP)**, which was designed for ARPANET. Although it allowed communication between connected systems, it wasn't designed to support a large, interconnected network of independent networks.

IPv4 addressed this limitation by introducing a standardized Layer 3 protocol that:

- Provides logical addressing independent of the underlying hardware
- Enables routers to forward packets between different networks
- Standardizes how data is packaged and delivered across interconnected networks
- Allows networks built with different technologies and by different vendors to communicate using a common protocol

These capabilities became the foundation of the modern Internet.

## Where does IPv4 fit in the TCP/IP model?

IPv4 operates at the **Internet layer** of the TCP/IP model.

Its responsibility is to provide logical addressing and route packets between networks. While Layer 2 protocols deliver frames within a local network, IPv4 enables communication across multiple interconnected networks.

## What does IPv4 actually do?

IPv4 is responsible for:

- Providing logical addressing for network interfaces
- Encapsulating data into IP packets
- Delivering packets using a best-effort model
- Enabling routers to forward packets between networks

IPv4 does not guarantee that packets will arrive, arrive in order, or arrive only once. Those responsibilities are handled by higher-layer protocols such as TCP.

## Why is IPv4 still used today?

Despite the introduction of IPv6, IPv4 remains the most widely deployed Internet Protocol.

Its continued use is largely due to the enormous amount of existing infrastructure built around it. Technologies such as **Network Address Translation (NAT)** have also extended its lifespan by allowing many devices to share a single public IPv4 address.

As a result, IPv4 and IPv6 coexist in many modern networks.

---

**Next:** Binary fundamentals