# Layer 2 Loops

## Overview

A Layer 2 loop is one of the worst failures that can occur in an Ethernet network.

A loop occurs when two or more active Layer 2 paths exist between switches, allowing an Ethernet frame to return to the same switch through a different path.

Unlike IP packets, Ethernet frames do not contain a Time To Live (TTL) field or any other mechanism that limits how long they can remain in the network. As a result, a frame caught in a switching loop can circulate indefinitely.

Layer 2 loops have several serious consequences:

- Broadcast storms
- MAC address table instability (MAC flapping)
- Duplicate frames
- High CPU and memory utilization

## Broadcast Storm

One of the most destructive effects of a Layer 2 loop is a broadcast storm.

Consider a simple ARP Request. Since it is a broadcast frame, every switch floods it out all ports except the one on which it was received.

Because a loop exists, the same broadcast frame eventually returns to the original switch through a different path. The switch treats it as a new broadcast frame and floods it again.

This process repeats continuously, causing the number of broadcast frames to grow exponentially until they consume nearly all available network bandwidth.

## MAC Address Table Instability (MAC Flapping)

Switches learn MAC addresses by recording the source MAC address of every frame they receive and associating it with the ingress port.

Suppose a host with MAC address `AAAA.AAAA.AAAA` is initially learned on interface `GigabitEthernet0/1`.

Because of the Layer 2 loop, the same frame later returns through `GigabitEthernet0/2`.

The switch assumes the device has moved to another port and updates the CAM table accordingly.

Moments later, another copy of the same frame arrives again through `GigabitEthernet0/1`, forcing the switch to update the entry once more.

This constant movement of the same MAC address between different interfaces is known as MAC flapping and causes continuous instability in the MAC address table.

## Duplicate Frames

A Layer 2 loop also causes duplicate frames.

Since Ethernet has no mechanism to determine whether a frame has already been processed, every copy of the frame is treated as a completely new frame.

As the frame continues circulating through the loop, switches keep forwarding additional copies, causing hosts to receive the same frame multiple times.

## High CPU and Memory Utilization

Every frame received by a switch requires processing.

The switch must:

- Process BPDUs
- Learn source MAC addresses
- Update the CAM table
- Perform forwarding decisions
- Replicate broadcast and multicast traffic

During a Layer 2 loop, the number of frames increases dramatically, forcing the switch to spend most of its resources processing duplicate traffic instead of forwarding legitimate user traffic.

If the loop is not eliminated, CPU utilization, memory consumption, and bandwidth usage can all reach critical levels, potentially causing the entire Layer 2 network to become unavailable.