# Subnet Masks

## Overview

Every IPv4 address is divided into two logical portions: the **Network ID** and the **Host ID**.

But how does a device know where the network portion ends and the host portion begins? That's precisely the problem solved by the **subnet mask**.

A subnet mask is a **32-bit value** that acts as a logical filter, indicating which bits belong to the network identifier and which bits belong to the host identifier.

## Binary representation

Although subnet masks are commonly written in dotted-decimal notation (for example, **255.255.255.0**), computers process them internally in binary.

As explained in [Binary to Decimal](binary-to-decimal.md), each octet contains **8 bits**. When all eight bits are set to **1**, the binary value is:

```text
11111111
```

which is equal to **255** in decimal.

Therefore, the subnet mask:

```text
255.255.255.0
```

is internally represented as:

```text
11111111.11111111.11111111.00000000
```

## What a subnet mask actually answers

In essence, the subnet mask allows the operating system to answer one fundamental question before sending a packet:

> Does the destination belong to my local network?

To answer that question, the device performs the following process.

Suppose a host has the following network configuration:

```text
IP Address:   192.168.1.25/24
Subnet Mask:  255.255.255.0
```

and wants to send a packet to:

```text
192.168.1.15/24
```

Before transmitting any packet, the operating system must determine whether the destination belongs to the same local network.

## Step 1 — Calculate the source device's Network Address

The first step is to calculate its own **Network Address** by performing a logical **AND** operation between its IP address and its subnet mask.

An AND operation is simple: the result bit is `1` only when **both** bits being compared are `1`. Otherwise, it's `0`.

```text
IP Address:   11000000.10101000.00000001.00011001   (192.168.1.25)
Subnet Mask:  11111111.11111111.11111111.00000000   (255.255.255.0)
AND -------------------------------------------------------------
Network:      11000000.10101000.00000001.00000000   (192.168.1.0)
```

The source device's Network Address is **192.168.1.0**.

## Step 2 — Calculate the destination device's Network Address

Next, the operating system performs the exact same calculation using the destination IP address.

```text
IP Address:   11000000.10101000.00000001.00001111   (192.168.1.15)
Subnet Mask:  11111111.11111111.11111111.00000000   (255.255.255.0)
AND -------------------------------------------------------------
Network:      11000000.10101000.00000001.00000000   (192.168.1.0)
```

The destination device's Network Address is also **192.168.1.0**.

## Step 3 — Compare both Network Addresses

Finally, both Network Addresses are compared.

- If both Network Addresses are **identical**, both devices belong to the same local network. The packet can be delivered directly after resolving the destination MAC address using **ARP**.
- If the Network Addresses are **different**, the destination belongs to another network. In that case, the packet is forwarded to the **default gateway**, which is responsible for routing it toward the destination network.

In this example, both results are **192.168.1.0** — so both devices are on the same local network, and the packet can be delivered directly.

This entire decision-making process happens before the first Ethernet frame is even transmitted, making the subnet mask one of the fundamental components of IPv4 networking.

---

**Next:** How subnetting works