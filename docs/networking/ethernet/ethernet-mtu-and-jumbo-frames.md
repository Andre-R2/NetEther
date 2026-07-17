# MTU & Jumbo Frames

## MTU

The **Maximum Transmission Unit (MTU)** is the maximum size, measured in **bytes**, of an IP packet that can be transmitted over a network interface without fragmentation.

In standard Ethernet networks, the default MTU is **1500 bytes**.

The MTU acts as a size limit for packets transmitted across the network. If a packet exceeds the MTU of an interface, it must either be **fragmented** into smaller packets (IPv4) or discarded, requiring retransmission by the sender (IPv6).

Using an appropriate MTU improves network efficiency by reducing fragmentation and minimizing protocol overhead.

## Jumbo Frames

**Jumbo Frames** are Ethernet frames that support payloads larger than the standard **1500-byte MTU**, typically around **9000 bytes** (depending on the equipment).

Their primary purpose is to improve network efficiency by allowing more data to be transmitted in each frame.

Compared to standard Ethernet frames, Jumbo Frames offer several advantages:

- Fewer packets are required to transfer the same amount of data.
- Less protocol overhead from Ethernet, IP, and TCP headers.
- Lower CPU utilization, since network devices process fewer frames.
- Improved throughput in high-bandwidth environments.

Jumbo Frames are commonly used in **data centers**, **storage networks**, **virtualization platforms**, and **high-performance computing**.

> **Note:** Every device along the communication path must support the same MTU. If one device does not support Jumbo Frames, communication issues or packet drops may occur.