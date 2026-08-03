# PortFast

## Overview

PortFast is an STP feature that allows a port to skip the Listening and Learning states and transition directly to **Forwarding** as soon as a device connects.

Before activating a port, the STP algorithm normally transitions it through Listening and Learning first — that transition is what lets the switch verify that turning this new port on won't create a loop.

Technically, switches always assume that whatever gets connected to a port **could be another switch**, which is why they run the full process on every port by default.

## Why this is a problem for endpoints

When you're connecting an endpoint (a PC, an IP phone, a CCTV camera, etc.), you don't need that port to go through the full transition — for example, a PC needs to receive its network configuration from a DHCP server, but if the port still has to complete the entire STP process first, those DHCP packets can be lost or time out before the port even starts forwarding.

## Where PortFast helps

This is exactly where PortFast has the most value: configuring an access port in PortFast mode allows the device connected to it to enter Forwarding mode **immediately**, skipping the delay entirely.

## PortFast doesn't mean ignoring STP

PortFast doesn't mean STP is disabled on that port. It means the switch **trusts** that the port connects to an endpoint and lets it enter Forwarding immediately. The switch port still keeps sending and monitoring for BPDUs — even though the connected endpoint doesn't participate in STP or generate BPDUs of its own.

## PortFast in RSTP (Edge Ports)

In RSTP (802.1w), the equivalent of PortFast is an **Edge Port**. An Edge Port is a port assumed to be connected to an end device so it can move immediately to the Forwarding state without waiting for the normal convergence process.

Unlike classic STP, RSTP converges much faster, thanks to a more efficient BPDU exchange mechanism and quicker detection of topology changes. Because of this, Edge Ports don't exist to compensate for slow convergence — their purpose is to identify ports where another switch isn't expected to connect.

If an Edge Port receives a BPDU, RSTP assumes there's actually a device on the other end that participates in STP. As a result, the port automatically loses its Edge Port status and starts behaving like a normal RSTP port, participating in the convergence algorithm. If BPDU Guard is also enabled, the port is placed into **err-disabled** state instead, since this is treated as a violation of network policy.
