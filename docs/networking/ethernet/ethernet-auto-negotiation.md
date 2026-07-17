# Auto-Negotiation

## Overview

**Auto-Negotiation** is a mechanism defined by the **IEEE 802.3** standard that allows two Ethernet devices to automatically exchange their capabilities and agree on the best common link configuration.

Instead of requiring manual configuration, both devices negotiate the highest compatible **speed** and **duplex mode**, ensuring that the link operates as efficiently as possible.

## How Auto-Negotiation Works

When an Ethernet cable is connected, both devices exchange **Fast Link Pulses (FLPs)** that advertise their supported capabilities, such as:

- Link speed (10, 100, 1000 Mbps, or higher)
- Duplex mode (Half or Full Duplex)

After exchanging this information, both devices select the highest performance configuration supported by both ends. In most modern networks, this results in a **Full Duplex Gigabit Ethernet** connection.

## Duplex Mismatch

Problems can occur when one device is configured manually while the other remains in Auto-Negotiation mode.

In this situation, the device using Auto-Negotiation can usually detect the link speed but cannot reliably determine the duplex mode, often defaulting to **Half Duplex**.

This creates a **Duplex Mismatch**, where one device operates in **Full Duplex** while the other operates in **Half Duplex**.

A duplex mismatch can cause:

- Poor network performance
- Frame errors
- Late collisions
- Packet retransmissions

For this reason, both ends of an Ethernet link should either use **Auto-Negotiation** or be configured manually with matching speed and duplex settings.