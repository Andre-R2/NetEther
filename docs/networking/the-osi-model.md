# The OSI model

## Overview

The Open Systems Interconnection (OSI) Model is a conceptual framework developed to standardize how systems communicate across a network. It divides network communication into seven abstract layers, providing a common reference model for the communication of applications, operating systems, and network devices.

Although the OSI model is not directly implemented in modern networks, it remains an essential tool for understanding how data travels across a network and for helping network engineers isolate and troubleshoot problems in a structured way.

## Why was the OSI model created?

During the early 1980s, computer systems became increasingly interconnected. However, each manufacturer developed its own networking technologies and communication protocols, resulting in many systems that were not interoperable with one another.

Because every vendor followed different rules, computers and network equipment often could not communicate correctly, creating a major compatibility problem.

The development of TCP/IP represented a significant step forward by defining a common set of protocols that allowed different networks to communicate. However, there was still a need for a more general reference model that manufacturers could use when designing networking technologies.

To address this problem, the OSI model was created. Instead of defining specific communication protocols, it provides a standardized framework that divides network communication into seven independent layers, allowing technologies from different vendors to be designed following the same general principles.

## OSI model layers

The OSI model organizes network communication into seven layers. Each layer has a specific responsibility and works together with the layers above and below it.

### Layer 1 – Physical

Responsible for transmitting raw bits through the physical medium. Depending on the network infrastructure, bits may travel as electrical signals (copper), light pulses (fiber optic), or radio waves (wireless).

### Layer 2 – Data Link

Responsible for delivering data between directly connected devices. It organizes bits into frames, uses MAC addresses to identify devices within the local network, and controls how devices access the transmission medium.

### Layer 3 – Network

Responsible for moving packets between different networks using logical addressing, such as IP addresses. Devices like routers operate primarily at this layer, forwarding packets by consulting their routing tables and selecting the most appropriate path toward the destination.

### Layer 4 – Transport

Provides end-to-end communication between devices. It's responsible for ensuring that data is delivered correctly, reliably, and in the proper order when required. Protocols such as TCP and UDP operate at this layer.

### Layer 5 – Session

Establishes, manages, and terminates communication sessions between applications. It keeps conversations synchronized and ensures that communication can continue correctly between both endpoints.

### Layer 6 – Presentation

Responsible for translating, formatting, and encrypting data so that it can be correctly interpreted by the receiving system, regardless of differences in data representation.

### Layer 7 – Application

The closest layer to the end user. It provides network services directly to applications and is where protocols such as HTTP, HTTPS, DNS, SMTP, and many others operate.

## Layers and their PDU

Each layer refers to the data it handles by a different name — this is called the **Protocol Data Unit (PDU)**.
A PDU is the unit of data that a layer of the OSI model processes and transmits. As data moves down the protocol stack, each layer adds its information.

| Layer | Name | PDU |
|---|---|---|
| 7 | Application | Data |
| 6 | Presentation | Data |
| 5 | Session | Data |
| 4 | Transport | Segment |
| 3 | Network | Packet |
| 2 | Data Link | Frame |
| 1 | Physical | Bit |

!!! tip "Mnemonic"
    A common way to remember the order (Layer 7 down to Layer 1) is: **P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way — Application, Presentation, Session, Transport, Network, Data Link, Physical.

## Conclusion

The OSI model remains one of the most important concepts in networking. Although modern networks are primarily based on the TCP/IP model, the OSI model provides a universal way to understand how network communication works, design interoperable systems, and troubleshoot problems by separating the communication process into clearly defined layers.

---

**Next:** [The TCP/IP model](tcpip/the-tcpip-model.md)