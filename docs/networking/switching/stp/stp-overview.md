# Spanning Tree Protocol (STP)

## Overview

Imagine you need to transport multiple VLANs between two switches. To do this, you configure a trunk port on each switch and connect them using a single physical link.

Everything works correctly, but relying on only one link creates a **single point of failure**. If that link fails, communication between both switches is lost.

This introduces one of the most important concepts in networking: **redundancy**. Redundancy is essential in modern networks because it provides fault tolerance, allowing communication to continue even if a physical link goes down.

To achieve this, you add a second physical connection between the switches and configure both interfaces as trunk ports. Now, if one link fails, traffic can continue flowing through the other.

However, introducing redundant Layer 2 paths also creates a new problem.

## What happens to broadcast traffic?

We know that when a switch receives a broadcast Ethernet frame, it floods the frame out every port except the one on which it was received.

Suppose Switch A sends a broadcast frame through the first trunk link. Switch B receives it and floods it through all of its other ports, including the second trunk link. The same frame is then received again by Switch A through the second link. Since Switch A also treats it as a broadcast frame, it floods it once more.

This process repeats indefinitely, creating a Layer 2 switching loop.

Unlike IP packets, Ethernet frames don't contain a Time To Live (TTL) field or any other mechanism that limits how long they can remain in the network. As a result, a frame caught in a switching loop can circulate forever.

Broadcast storms aren't the only symptom of a Layer 2 loop. The same looping traffic also causes MAC table instability — switches see the same source MAC address arriving on multiple ports within milliseconds of each other, constantly relearning it — and end devices can receive duplicate copies of the same frame. In a real network, all of this together tends to bring the switches (and anything connected to them) to a crawl very quickly.

## What STP does about it

Spanning Tree Protocol (STP) is a Layer 2 protocol designed to prevent switching loops by logically blocking redundant paths while preserving the physical redundancy. If the active path fails, STP automatically recalculates the topology and activates one of the previously blocked links.

---

