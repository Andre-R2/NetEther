# Rapid Spanning Tree Protocol (RSTP)

Rapid Spanning Tree Protocol (RSTP), standardized as IEEE 802.1w, is an evolution of the original Spanning Tree Protocol (STP, IEEE 802.1D).

RSTP preserves the same loop prevention algorithm and uses the same Root Bridge election process as STP. The primary difference is its ability to dramatically reduce convergence time after a topology change. While classic STP may require between 30 and 50 seconds to restore connectivity after a link failure, RSTP typically converges within a few seconds.

RSTP was designed to solve the biggest limitation of classic STP: slow convergence.

## The problem with classic STP

When a topology change occurs in IEEE 802.1D STP, ports cannot begin forwarding traffic immediately. Instead, they must transition through several states before reaching the Forwarding state.

```text
Blocking
      ↓
Listening (~15 s)
      ↓
Learning (~15 s)
      ↓
Forwarding
```

This behavior was intentionally conservative. Before allowing traffic to flow, STP waited for its timers to expire to ensure that no Layer 2 loops could be created. Although effective, this process could leave parts of the network disconnected for 30–50 seconds after a failure.

RSTP eliminates most of this waiting by replacing timer-based convergence with synchronization between neighboring switches.

## What remains the same

RSTP does not replace the spanning tree algorithm. It preserves the same fundamental concepts introduced by STP:

- Root Bridge election
- Bridge ID comparison
- Root Path Cost calculation
- Root Port election
- Designated Port election
- Loop-free Layer 2 topology

## Simplified port states

RSTP reduces the five STP port states to three operational states.

| RSTP State | Description |
|---|---|
| Discarding | The port does not forward user traffic and does not learn MAC addresses. It still processes BPDUs. |
| Learning | The port begins learning MAC addresses but still does not forward user traffic. |
| Forwarding | The port forwards user traffic, learns MAC addresses, and processes BPDUs. |

The Discarding state replaces the Blocking and Listening states used in classic STP.

## Port roles

RSTP maintains the same Root Port and Designated Port roles used by STP, but introduces additional roles that allow faster recovery after failures.

### Root Port

Each non-root switch has exactly one Root Port. It represents the lowest-cost path toward the Root Bridge.

### Designated Port

Each network segment has one Designated Port responsible for forwarding traffic toward that segment.

The Root Bridge always has all of its active ports in the Designated role.

### Alternate Port

An Alternate Port provides a backup path toward the Root Bridge.

Unlike classic STP, where a blocked port simply waits for timers to expire after a failure, an Alternate Port is already aware that it is the standby path. If the active path fails, it can quickly transition to the forwarding state.

### Backup Port

A Backup Port is a secondary path to the same shared network segment. It is rarely seen in modern switched Ethernet networks because it only appears when multiple ports from the same switch connect to the same shared collision domain.

## BPDU improvements

One of the biggest architectural differences between STP and RSTP is how Bridge Protocol Data Units (BPDUs) are handled.

In classic STP, the Root Bridge originates the BPDUs, while the remaining switches primarily propagate those messages throughout the network.

RSTP changes this behavior. Every switch actively generates and transmits its own BPDUs every Hello interval, maintaining a continuous exchange of information with its directly connected neighbors.

Because switches constantly communicate with one another, they can detect failures much faster without waiting for long timer expirations.

## Proposal / Agreement synchronization

The most important improvement introduced by RSTP is the Proposal/Agreement synchronization mechanism.

Instead of waiting for multiple timers to expire before activating a new forwarding path, neighboring switches negotiate whether the new path can safely begin forwarding traffic.

The process works as follows:

1. The Designated Bridge sends a Proposal BPDU to its neighboring switch.
2. The neighboring switch temporarily places its non-edge ports into the Discarding state to guarantee that enabling the new path cannot create a Layer 2 loop.
3. After verifying that the topology is safe, it responds with an Agreement BPDU.
4. Both switches immediately transition the new port to the Forwarding state.

This synchronization mechanism replaces the long timer-based waiting process used by classic STP and is the primary reason why RSTP converges so quickly.

## Rapid Transition

Rapid Transition is the direct result of the Proposal/Agreement mechanism.

Instead of waiting for the traditional Listening and Learning timers, RSTP allows eligible ports to move directly from the Discarding state to the Forwarding state as soon as both switches agree that enabling the link cannot introduce a loop.

This allows traffic to resume almost immediately after many topology changes.

## Edge Ports

RSTP also introduces the concept of an Edge Port.

An Edge Port is a switch port connected directly to an end device, such as:

- PCs
- Servers
- Printers
- IP phones

Since these devices cannot create Layer 2 loops, Edge Ports bypass the normal synchronization process and immediately enter the Forwarding state when the link comes up.

If an Edge Port later receives a BPDU, it automatically loses its Edge status and begins participating in RSTP like any other switch port.

## STP vs RSTP

| STP (IEEE 802.1D) | RSTP (IEEE 802.1w) |
|---|---|
| Converges in 30–50 seconds | Typically converges in a few seconds |
| Relies heavily on timers | Uses synchronization between switches |
| Five port states | Three port states |
| Root Bridge originates BPDUs | Every switch generates BPDUs |
| Slow recovery after failures | Fast failover using Alternate Ports |
| Waits through Listening and Learning timers | Uses Proposal/Agreement for rapid transitions |

RSTP achieves faster convergence without changing the spanning tree algorithm itself. Instead, it improves how neighboring switches communicate and synchronize, allowing the network to recover from topology changes in a fraction of the time required by classic STP.