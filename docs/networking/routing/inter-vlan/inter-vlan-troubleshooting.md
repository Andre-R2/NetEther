# Inter-VLAN Troubleshooting

The normal Inter-VLAN Routing process is:

An endpoint located in one VLAN generates traffic destined for an endpoint in another subnet. The traffic reaches the access switch, which associates the frame with the corresponding VLAN. When the frame is forwarded through the trunk toward the Layer 3 switch, it is carried using 802.1Q with the corresponding VLAN ID. The Layer 3 switch receives the traffic, performs routing toward the destination subnet, and forwards it through the appropriate interface.

Troubleshooting should follow the same path as the traffic. Each section verifies one part of that path before moving to the next.

---

## 1. Source Endpoint

Before troubleshooting the network infrastructure, verify that the source endpoint has a valid IP configuration for the network to which it belongs.

### IP Address

The IP address must:

- Belong to the correct subnet.
- Not be the network address.
- Not be the broadcast address.
- Not be duplicated by another device.
- Correspond to the subnet associated with the VLAN where the endpoint is connected.

### Subnet Mask

The subnet mask must match the designed subnet.

An incorrect subnet mask can cause the endpoint to incorrectly determine whether the destination is on the local subnet or on a different network.

If the endpoint interprets the destination incorrectly, it may make the wrong decision about whether to send traffic directly to the destination or forward it to the default gateway.

### Default Gateway

The default gateway must correspond to the Layer 3 interface acting as the gateway for the endpoint's subnet.

An incorrect default gateway can cause the endpoint to have a valid IP address, subnet mask, and physical connectivity while still being unable to correctly forward traffic destined for other networks.

---

## 2. Layer 2 Switch

Once the source endpoint has been verified, check the access switch through which the traffic enters the network.

### 2.1 Physical and Operational Status

First, verify that the switch port connected to the endpoint is operational.

```text
show interfaces status
```

Verify that:

- The port is `connected`.
- The interface is not administratively down.
- The interface is not in a state that prevents traffic forwarding.

---

### 2.2 Access Port

The endpoint's access port must be configured correctly and assigned to the expected VLAN.

```text
show vlan brief
show interfaces fa0/10 switchport
```

Verify:

- The port is operating in access mode.
- The port belongs to the correct VLAN.
- The VLAN exists on the switch.
- The port is operational.

For example, if the endpoint belongs to VLAN 20:

```text
Fa0/10 → VLAN 20
```

If the port is accidentally assigned to another VLAN, the endpoint's IP configuration may be correct while its traffic enters the wrong Layer 2 broadcast domain.

---

### 2.3 VLAN Status

The VLAN must exist and be active on the switch.

```text
show vlan brief
```

Verify:

- The VLAN exists.
- The VLAN is active.
- The endpoint's port is assigned to the expected VLAN.

A missing, inactive, or incorrectly assigned VLAN can prevent traffic from reaching the Layer 3 device.

---

### 2.4 Trunk Toward the Layer 3 Device

The link between the access switch and the Layer 3 switch must be operating correctly as a trunk.

```text
show interfaces trunk
```

Verify:

- The interface is operating as a trunk.
- The expected encapsulation is being used.
- The required VLAN is allowed.
- The VLAN is active.
- The VLAN is being carried across the trunk.

---

### 2.5 Trunk Configuration

For a more detailed inspection of the trunk interface:

```text
show interfaces gi0/1 switchport
```

Look for inconsistencies involving:

- Administrative mode.
- Operational mode.
- Encapsulation.
- Native VLAN.
- Allowed VLANs.

---

### 2.6 Native VLAN

The Native VLAN should be consistent on both ends of the trunk.

```text
show interfaces trunk
```

This check must be performed on both the access switch and the Layer 3 switch.

A Native VLAN mismatch can cause Layer 2 inconsistencies and unexpected behavior on the trunk.

---

### 2.7 Allowed VLANs

Verify that the VLAN involved in the communication is explicitly allowed on the trunk.

For example:

```text
switchport trunk allowed vlan 10,20,30
```

If VLAN 40 is not included, traffic belonging to VLAN 40 will not be able to cross the trunk, even if the rest of the configuration is correct.

---

### 2.8 STP

Verify that the path used by the VLAN is in a forwarding state.

```text
show spanning-tree vlan 20
```

The interface that should carry the VLAN must be in a state that permits forwarding, normally:

```text
FWD
```

If STP is blocking the required path, the VLAN cannot use that path to reach the Layer 3 switch.

---

## 3. Layer 3 Switch

Once the Layer 2 path has been verified, check whether the Layer 3 switch can actually operate as the gateway and perform routing between the VLANs.

### 3.1 Source VLAN SVI

First, verify that the SVI corresponding to the source VLAN exists.

```text
show ip interface brief
```

Example:

```text
Vlan20    172.16.20.1    YES    manual    up    up
```

Verify:

- The SVI exists.
- The IP address is correct.
- The interface is administratively enabled.
- The interface is in an `up/up` state.

The SVI address must match the default gateway configured on the endpoints in that subnet.

---

### 3.2 SVI-to-VLAN Association

The SVI must correspond to the correct VLAN.

It is not enough for `interface Vlan20` to exist. VLAN 20 must also exist on the switch.

The expected relationship is:

```text
VLAN 20
    ↓
interface Vlan20
    ↓
172.16.20.1
```

The SVI represents the Layer 3 interface associated with that VLAN.

---

### 3.3 IP Routing

The Layer 3 switch must have IP routing enabled.

```text
show running-config | include ip routing
```

The configuration should contain:

```text
ip routing
```

If IP routing is not enabled, the switch may have correctly configured SVIs but will not perform routing between them.

---

### 3.4 Routing Table

After verifying the SVIs and IP routing, check the routing table.

```text
show ip route
```

When both VLANs are directly connected to the Layer 3 switch, they should appear as connected routes.

Example:

```text
C    172.16.20.0/26    is directly connected, Vlan20
C    172.16.40.0/27    is directly connected, Vlan40
```

Verify:

- The source network is present.
- The destination network is present.
- The prefixes are correct.
- The associated interfaces correspond to the correct VLANs.

---

### 3.5 Trunk on the Layer 3 Switch

The Layer 3 side of the trunk must also be verified.

```text
show interfaces trunk
```

For a more detailed inspection:

```text
show interfaces gi0/1 switchport
```

Verify:

- The interface is operating as a trunk.
- The source VLAN is allowed.
- The VLAN is active.
- The VLAN is being carried across the trunk.
- The trunk configuration is consistent with the connected access switch.
