# Switching Overview

## Overview

In the early days of computer networking, if multiple computers needed to communicate with each other, one option was to connect every device directly to every other device. However, this approach quickly became impractical because the number of required cables increased dramatically as more devices were added.

To solve this problem, the hub was introduced, allowing every device to connect to a single central point.

At first glance, a hub and a switch appear to perform the same function, since both connect devices within a Local Area Network (LAN). The difference lies in what they do when they receive an Ethernet frame.

A hub has no knowledge of MAC addresses, no memory, and no MAC table. In other words, it has no awareness of the network topology. Its only job is to copy the incoming electrical signal and retransmit it through every other port.

As a result, every transmission behaves like a physical broadcast, forcing every connected device to receive and process every frame, even when it is not the intended recipient.

## The problem with hubs

The biggest drawback of hubs was **collisions**.

If two devices transmitted simultaneously, their electrical signals collided, corrupting the transmitted data.

To mitigate this problem, Ethernet networks relied on CSMA/CD (Carrier Sense Multiple Access with Collision Detection), a mechanism that detects collisions, stops the transmission, waits for a random amount of time, and then retransmits the frame.

As networks grew larger, collisions became more frequent, significantly reducing network performance.

Because all devices connected to a hub share the same communication medium, the entire hub forms a single collision domain.

## The solution: switches

Switches were designed to eliminate these limitations.

A **switch** is a network device that connects multiple devices within a Local Area Network (LAN), forwarding Ethernet frames only to their intended destination instead of sending them to every connected device.

Unlike a hub, a switch examines each Ethernet frame, reads the destination MAC address, and consults its MAC Address Table (CAM Table) to determine the correct outgoing port.

For example, if the switch has learned that a particular device is connected to port 5, it forwards the frame only through that port, preventing every other device from unnecessarily processing the traffic.

Another major improvement is that every switch port represents its own collision domain, allowing multiple devices to communicate simultaneously without interfering with one another.

Modern switches also operate in **Full-Duplex** mode, allowing devices to transmit and receive data simultaneously. Hubs, on the other hand, operated in **Half-Duplex**, where a device could either transmit or receive, but never both at the same time.

The combination of MAC-based forwarding, independent collision domains, and Full-Duplex communication made collisions virtually disappear in modern Ethernet networks.

## What's next?

In order to forward Ethernet frames to the correct destination, a switch must first learn where every device is connected.

It accomplishes this by building and maintaining a **MAC Address Table (CAM Table)**.