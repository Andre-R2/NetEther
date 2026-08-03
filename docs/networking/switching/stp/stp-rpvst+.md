# Rapid PVST+

Rapid Per VLAN Spanning Tree Plus (Rapid PVST+) is Cisco's implementation of Rapid Spanning Tree Protocol (IEEE 802.1w) running independently for every VLAN.

Rapid PVST+ combines the fast convergence mechanisms introduced by RSTP with the independent spanning tree instances provided by PVST+, allowing each VLAN to build and maintain its own loop-free topology while recovering much faster from topology changes.

## Why Rapid PVST+ exists

PVST+ solved one of the biggest limitations of classic STP by allowing every VLAN to have its own spanning tree instance. This made it possible to distribute traffic across redundant links instead of forcing every VLAN to use the same forwarding path.

However, PVST+ still inherited the slow convergence timers of IEEE 802.1D. A topology change could still take 30 to 50 seconds before traffic started flowing again.

Rapid PVST+ was introduced to solve that problem by replacing the underlying STP algorithm with RSTP while keeping one spanning tree instance per VLAN.

## Benefits

- Fast convergence after topology changes.
- One Rapid Spanning Tree instance per VLAN.
- Independent Root Bridge election for every VLAN.
- Independent path selection for every VLAN.
- Better utilization of redundant links.
- Layer 2 loop prevention with significantly faster recovery.

## Example

Suppose VLAN 10 and VLAN 20 are carried over two redundant trunk links between the same switches.

For VLAN 10:

```text
Trunk A → Forwarding
Trunk B → Blocking
```

For VLAN 20:

```text
Trunk A → Blocking
Trunk B → Forwarding
```

Both physical links are actively transporting traffic, but not for the same VLAN. Each VLAN builds its own Rapid Spanning Tree topology, allowing traffic to be distributed across redundant links while maintaining a loop-free network.

## Summary

Rapid PVST+ does not introduce a new spanning tree algorithm.

Instead, it combines:

- The fast convergence mechanisms of RSTP.
- The independent spanning tree instances of PVST+.

The result is a protocol that provides rapid recovery from topology changes while allowing every VLAN to maintain its own spanning tree and forwarding topology.