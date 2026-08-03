# How STP Works

## Root Bridge election

STP first analyzes the entire Layer 2 topology and elects a single switch to become the Root Bridge — the reference point for the entire spanning tree, toward which every other switch builds its forwarding path.

When switches boot, none of them knows who the Root Bridge is — each one initially assumes it's the Root Bridge itself. To figure out which switch should actually hold that role, every switch starts sending Bridge Protocol Data Units (BPDUs) out of all STP-enabled ports, each one initially advertising:

```text
Root Bridge = Myself
Root Path Cost = 0
```

Each switch then compares the BPDUs it receives against its own, using the Bridge ID (BID):

```text
Bridge ID (BID) = Bridge Priority + Extended System ID + MAC Address
```

- Lowest Bridge Priority wins.
- If priorities are equal, the lowest MAC Address wins.

**Example:**

| Switch | Priority | VLAN ID | MAC Address |
|---|---:|---:|---|
| SW1 | 32768 | 1 | 00:AA |
| SW2 | 32768 | 1 | 00:BD |
| SW3 | 32768 | 1 | 00:CC |

All three switches share the same priority and VLAN ID, so the MAC address breaks the tie — SW1 is elected Root Bridge, since it has the lowest MAC address.

Once the election is complete, all switches agree on the same Root Bridge.

## BPDUs

BPDUs are what makes all of this possible — switches exchange them continuously to elect the Root Bridge, calculate the best paths, and detect topology changes. Each BPDU carries, among other fields, the Root Bridge ID the switch currently believes is root, the switch's own Bridge ID, and its current Root Path Cost.

## Root path cost

Once the Root Bridge is elected, every non-root switch calculates the lowest-cost path to reach it. Each link has an associated cost, based on its speed:

| Link Speed | Path Cost |
|---|---:|
| 10 Mbps | 100 |
| 100 Mbps | 19 |
| 1 Gbps | 4 |
| 10 Gbps | 2 |

!!! note
    The Root Path Cost is the cumulative cost of all links from a switch to the Root Bridge. STP always selects the path with the lowest total cost.

![STP example](../../../assets/net-assets/stp-example-light.svg#only-light)
![STP example](../../../assets/net-assets/stp-example-dark.svg#only-dark)


**Example** (all links at 1 Gbps, cost 4 each):

```text
SW2 → SW1           = Cost 4
SW3 → SW1           = Cost 4
SW3 → SW2 → SW1     = Cost 8
SW2 → SW3 → SW1     = Cost 8
```

Each switch picks the path with the lowest cumulative cost — so SW3 would use its direct link to SW1 (cost 4) over the indirect path through SW2 (cost 8).

## Port roles

Once the Root Bridge and path costs are known, every switch determines the role of each of its ports.

### Root Port (RP)

Every switch except the Root Bridge has exactly one Root Port — the interface that provides the lowest-cost path toward the Root Bridge. All traffic destined for the Root Bridge is forwarded through it. The Root Bridge itself never has a Root Port.

### Designated Port (DP)

STP analyzes every network segment individually and elects exactly one Designated Port per segment — the port responsible for forwarding traffic for that segment. The Root Bridge has all of its ports in the Designated role, since every path originates from the Root.

If two switches on the same segment advertise the same Root Path Cost, ties are broken in this order:

1. Lowest Bridge ID
2. If still tied, lowest Port ID

**Example:** SW2 and SW3 both have a Root Path Cost of 4. STP compares their Bridge IDs — if SW2's is lower, SW2's port becomes the Designated Port, and SW3's port becomes an Alternate Port.

### Alternate Port (AP)

Once the forwarding topology is settled, any remaining redundant path is kept as a backup: one side stays Designated, the other becomes an Alternate Port. Alternate Ports don't forward user traffic — they stay in a Blocking state, ready to take over if the active path fails.

## Loop prevention

By assigning these roles, STP builds a loop-free logical topology while keeping the physical redundancy intact — the redundant links stay physically connected, but only one forwarding path is active at any time. If that active link fails, STP recalculates and promotes the appropriate Alternate Port to start forwarding, restoring connectivity without reintroducing a loop.

## Port states

| State | Forwards traffic | Learns MACs | Processes BPDUs |
|---|:---:|:---:|:---:|
| Blocking | No | No | Yes |
| Listening | No | No | Yes |
| Learning | No | Yes | Yes |
| Forwarding | Yes | Yes | Yes |
| Disabled | No | No | No |

A port moves through Blocking → Listening → Learning → Forwarding on its way to actively carrying traffic. Disabled just means the port is administratively shut down and takes no part in STP at all.

## What happens when a link fails

Suppose the link between SW1 and SW3 goes down:

```text
SW1
 │
 X
 │
SW3
```

![STP example](../../../assets/net-assets/stp-example-convergence-light.svg#only-light)
![STP example](../../../assets/net-assets/stp-example-convergence-dark.svg#only-dark)


SW3 stops receiving BPDUs through its Root Port, detects that its path to the Root Bridge is gone, and recalculates. The new best path becomes:

```text
SW3 → SW2 → SW1     (Root Path Cost = 8)
```

The port that was previously an Alternate Port is selected as the new Root Port and transitions through the classic STP states — Blocking → Listening → Learning → Forwarding — before it starts forwarding traffic again.

!!! note "Why this takes so long"
    Classic 802.1D uses fairly conservative default timers: Hello every 2 seconds, Max Age 20 seconds, and Forward Delay 15 seconds (applied twice — once for Listening, once for Learning). Add it up, and a topology change can take 30–50 seconds to fully converge. This slowness is exactly the problem that RSTP (802.1w) was designed to fix.

---

**Next:** RSTP (Rapid Spanning Tree Protocol)


