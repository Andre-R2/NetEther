# BPDU Filter

BPDU Filter is an STP feature that suppresses the transmission and reception of Bridge Protocol Data Units (BPDUs) on a port.

Normally, STP relies on BPDUs to discover neighboring switches, elect the Root Bridge, calculate the spanning tree, and detect topology changes. By filtering these messages, the port effectively stops participating in the STP process.

## How it works

When BPDU Filter is enabled on a port, the switch suppresses the exchange of BPDUs.

As a result, the port no longer takes part in STP calculations, since it is no longer advertising or processing spanning tree information.

This behavior can be useful in very specific scenarios where a connected device should never participate in STP.

## Why it should be used carefully

Although BPDU Filter may seem similar to BPDU Guard, they serve completely different purposes.

BPDU Guard protects the network by disabling a port if a BPDU is received.

BPDU Filter, on the other hand, simply suppresses BPDU communication.

If BPDU Filter is accidentally enabled on a port connected to another switch, STP may become unaware of that connection. Without exchanging BPDUs, the switches cannot properly calculate the spanning tree, increasing the risk of creating a Layer 2 loop.

For this reason, BPDU Filter should only be used in very specific situations and only when the network design explicitly requires it.

## BPDU Guard vs BPDU Filter

| BPDU Guard | BPDU Filter |
|------------|-------------|
| Disables the port when a BPDU is received. | Suppresses the transmission and reception of BPDUs. |
| Protects the network from unauthorized switches. | Prevents the port from participating in STP. |
| Commonly used on PortFast access ports. | Rarely used and only in specific scenarios. |