# Loop Guard

Loop Guard is an STP protection feature designed to prevent Layer 2 loops caused by the unexpected loss of Bridge Protocol Data Units (BPDUs) on non-designated ports.

When STP builds the spanning tree, redundant paths are intentionally blocked to eliminate switching loops. On a redundant link, one side becomes the Designated Port while the other becomes the Alternate Port.

The Alternate Port remains in the Blocking state (Discarding in RSTP) because it continuously receives BPDUs from the Designated Port, indicating that a better path to the Root Bridge already exists. As long as those BPDUs continue to arrive, the Alternate Port stays blocked.

## The problem

Consider a unidirectional link failure.

In this scenario, the physical link remains up, but communication only works in one direction. For example, Switch A can still send frames to Switch B, but Switch B can no longer send traffic back to Switch A.

As a result, the Alternate Port suddenly stops receiving BPDUs.

In classic STP, if a blocked port stops receiving BPDUs for long enough, it assumes that the path to the Root Bridge has disappeared. The port eventually transitions to the Forwarding state in an attempt to restore connectivity.

The problem is that the original forwarding path may still be active. If both paths begin forwarding traffic simultaneously, a Layer 2 loop can be created.

## How Loop Guard works

Loop Guard prevents this situation.

Instead of allowing the Alternate Port to transition to Forwarding simply because BPDUs have disappeared, Loop Guard places the port into a special state called **Loop Inconsistent**.

In this state, the port remains blocked until valid BPDUs are received again.

Once BPDUs resume, the port automatically leaves the Loop Inconsistent state and returns to its normal STP role without requiring manual intervention.

## Important

Loop Guard does **not** interfere with normal STP convergence.

If the link actually fails and the interface goes down, STP or RSTP recalculates the topology normally and activates the appropriate backup path.

Loop Guard only acts when the link remains physically up but BPDUs unexpectedly stop arriving, since this usually indicates a unidirectional link failure rather than a legitimate topology change.

## BPDU Guard vs Loop Guard

Although both features protect STP, they solve different problems.

BPDU Guard protects ports that should never receive BPDUs, such as PortFast access ports connected to end devices. If a BPDU is received, the port is immediately disabled to prevent an unauthorized switch from participating in the spanning tree.

Loop Guard protects ports that should always receive BPDUs, such as Alternate Ports and Root Ports. If those BPDUs unexpectedly stop arriving while the physical link remains up, Loop Guard prevents the port from incorrectly transitioning to the Forwarding state, avoiding a potential Layer 2 loop.