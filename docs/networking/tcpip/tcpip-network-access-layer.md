# Data Link Layer

## Overview

The **Data Link Layer** is responsible for preparing data for transmission across the local network. It receives packets from the Internet Layer, encapsulates them into **frames**, and passes them to the Physical Layer for transmission.

Its main responsibility is to ensure that two **directly connected devices** can exchange information in an organized and reliable manner.

Unlike the Internet Layer, which is responsible for delivering packets across multiple networks, the Data Link Layer only cares about communication between neighboring devices. In other words, it's responsible for delivering the frame to the **next hop** on the local network.

## Frame structure

To accomplish this, it encapsulates each packet inside a frame that contains additional information, including:

- Source MAC Address
- Destination MAC Address
- Protocol Type (EtherType)
- Data (Payload)
- Frame Check Sequence (FCS)

![Layer 2 frame diagram](../../assets/net-assets/Frame.svg)

*Figure 1: Ethernet frame structure at Layer 2*

This information allows the receiving device to identify who sent the frame, determine whether it's the intended recipient, identify the protocol encapsulated inside the frame, and verify whether the frame was corrupted during transmission.

## MAC addresses, not IP addresses

The Data Link Layer works with **MAC addresses**, not IP addresses. It does not understand routing, IP networks, ports, or applications. Its only responsibility is to deliver the frame to the next device on the local network.

## Why it matters

Without the Data Link Layer, the Physical Layer could still transmit bits, but the receiving device would have no reliable way to determine where a frame begins and ends, who sent it, who should receive it, or whether the transmission arrived with errors.

---

**Next:** [Internet layer](tcpip-internet-layer.md)