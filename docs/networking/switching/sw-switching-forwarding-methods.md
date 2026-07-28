# Switching Forwarding Methods

## Overview

A switch uses forwarding methods to determine how Ethernet frames are received and transmitted between its ports.

The forwarding method defines when the switch starts forwarding a frame and whether it checks the frame for errors before sending it.

The three main forwarding methods are:

- **Store-and-Forward**
- **Cut-Through**
- **Fragment-Free**

## Store-and-Forward

**Store-and-Forward** is the most commonly used switching method in modern networks.

With this method, the switch receives the entire Ethernet frame before forwarding it to the destination port.

Once the complete frame is received, the switch verifies the Frame Check Sequence (FCS) using Cyclic Redundancy Check (CRC) to detect transmission errors.

If the frame is valid, the switch forwards it. If errors are detected, the frame is discarded.

### Advantages

- Detects corrupted frames before forwarding them.
- Prevents damaged frames from propagating through the network.
- Supports different Ethernet speeds between ports.

### Disadvantage

The main drawback is increased latency because the switch must wait until the complete frame has been received and verified before forwarding it.

## Cut-Through

**Cut-Through** switching reduces latency by forwarding frames before the entire frame has been received.

The switch only reads the beginning of the frame, specifically the destination MAC address. Once the destination port is identified, the switch immediately starts transmitting the frame.

The frame continues entering the switch while it is being forwarded to the destination port.

### Advantages

- Provides lower latency compared to Store-and-Forward.
- Useful in environments where transmission speed is critical.

### Disadvantage

Because the switch does not receive the complete frame, it cannot verify the **FCS**.

If the frame contains errors, the switch will forward the corrupted frame because it cannot detect the problem.

## Fragment-Free

**Fragment-Free** switching is a compromise between Store-and-Forward and Cut-Through.

In this method, the switch waits until it receives the first 64 bytes of the frame before forwarding it.

Historically, Ethernet collisions occurred within the first 64 bytes of a transmission. By checking this portion, the switch could avoid forwarding most collision fragments.

### Advantages

- Lower latency than Store-and-Forward.
- Provides better error detection than Cut-Through.

### Disadvantage

It does not perform a complete FCS check, so some corrupted frames may still be forwarded.

## Comparison

| Method | Waiting time | FCS Check | Latency | Error Detection |
|---|---|---|---|---|
| Store-and-Forward | Entire frame | Yes | Higher | Best |
| Cut-Through | Destination MAC | No | Lowest | Limited |
| Fragment-Free | First 64 bytes | No | Medium | Partial |
