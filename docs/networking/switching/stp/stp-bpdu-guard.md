# BPDU Guard

## Overview

When you enable PortFast on an access port, you're telling the switch that an endpoint will be connected there, so it doesn't need to go through the Blocking → Listening → Learning → Forwarding transition.

## Why it's essential in classic STP

In classic STP, BPDU Guard is indispensable because STP takes a few seconds to converge — that's a window of time where loops could form. If PortFast is enabled on a port, it moves to Forwarding immediately; if a switch (rather than an endpoint) happens to be connected there, it can create a loop during that window. That's why you need BPDU Guard: it disables the port the moment it receives a BPDU.

## Its role in RSTP

In RSTP, the risk of a temporary loop is much lower, since convergence is very fast and an Edge Port that receives a BPDU immediately loses that status and starts behaving like a normal RSTP port. Here, BPDU Guard isn't as necessary to compensate for convergence speed — its main purpose becomes **enforcing network policy**: if a port was meant exclusively for endpoints, it should never receive a BPDU in the first place.

!!! note
    When BPDU Guard shuts a port down, it goes into **err-disabled** state — and it stays that way. The port doesn't recover on its own; you either have to manually cycle it (`shutdown` / `no shutdown`) or configure **errdisable recovery**, which automatically re-enables the port after a set timeout. 

---

**Next:** BPDU Filter