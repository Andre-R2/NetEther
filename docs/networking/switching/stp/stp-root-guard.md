# Root Guard

## Overview

In STP, all switches exchange BPDUs, and in those BPDUs each switch announces the Root Bridge it currently knows about. If a switch receives a BPDU with a lower Bridge ID (BID), it updates its own information — that's how STP elects the Root Bridge.

## The problem Root Guard solves

Imagine you have 3 switches running STP. You only want those 3 switches to be eligible to become the Root Bridge — STP elects one, and everything's fine.

The problem shows up when someone connects another switch with a lower priority. Since STP always looks for the best possible root, that new switch will now become the Root Bridge.

But that's not what you want — an access switch, or one connected by mistake, shouldn't be able to change the entire topology.

## What Root Guard does

That's exactly what Root Guard prevents — it's a network policy, not a loop-prevention mechanism like the other STP protections.

If a switch connected to a Root-Guard-enabled port has a lower BID than the switches that are actually allowed to become Root Bridge, Root Guard blocks only that link and isolates the switch that tried to become Root.

The port enters a Root Inconsistent state — meaning it's blocking the link because it's receiving BPDUs that shouldn't exist there. It stays in that state until the connected switch stops advertising itself as Root.

Root Guard still allows switches to be connected — it just prevents them from announcing a better Root Bridge than expected. If it receives a superior BPDU, it temporarily blocks the port to preserve the Root Bridge's intended location in the network.

These two get mixed up often, but they solve different problems:

- BPDU Guard assumes a port should never see a BPDU at all (typically a PortFast/access port) — if one shows up, the port goes to err-disabled and stays down until you manually recover it.
- Root Guard assumes a port can see BPDUs (it's connected to a real switch) — it just won't allow that neighbor to become Root. The port goes to root-inconsistent, and it recovers automatically as soon as the superior BPDUs stop arriving — no manual intervention needed.

---

