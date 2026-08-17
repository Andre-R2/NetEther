# LACP

## Link Aggregation Control Protocol

When we want to use multiple physical links between two network devices, the objective can be to combine those links into a single logical interface.

**LACP (Link Aggregation Control Protocol)** is an IEEE-standardized protocol used to negotiate, establish, and maintain link aggregation between compatible devices.

The resulting logical interface is commonly called a **Port-Channel** or **Link Aggregation Group (LAG)**.

```text
SW1                          SW2

Gi0/1  ====================  Gi0/1
Gi0/2  ====================  Gi0/2
              |
         Port-Channel 1
```

## How LACP works

LACP allows both devices to exchange information about their interfaces and determine which links can participate in the aggregation. Once the links are successfully bundled, they become members of the same Port-Channel.

LACP uses **LACP Data Units (LACPDU)** to exchange information between the participating devices, and continuously monitors the state of the member links. If one physical link fails, it can be removed from the aggregation and traffic continues through the remaining members.

## LACP modes

### Active

The device actively sends LACP negotiation messages and attempts to establish the aggregation.

### Passive

The device waits for LACP negotiation messages from the other side. It does not initiate the negotiation itself.

## Traffic distribution

LACP provides the mechanism for negotiating and maintaining the aggregated links, but LACP itself doesn't decide which physical member link carries each individual frame.

Traffic distribution is handled by the device's load-balancing algorithm, which normally uses a hash based on information such as source/destination MAC addresses, IP addresses, or TCP/UDP ports, depending on the platform and configuration.

So having two physical links in a Port-Channel doesn't mean a single flow will use both links simultaneously — a given flow is hashed onto one physical member, while different flows can end up distributed across the others.

## Link failure

If one member link fails, the Port-Channel stays operational through the remaining members
LACP detects that the failed member is no longer participating and removes it from the bundle automatically.

## Configuration

Suppose we want to create an LACP Port-Channel between two switches:

```text
SW1                         SW2

Gi0/1 ===================== Gi0/1
Gi0/2 ===================== Gi0/2
```

### SW1

```text
enable
configure terminal

interface range gigabitEthernet 0/1 - 2
 channel-group 1 mode active
exit

interface port-channel 1
 switchport mode trunk
exit

end
```

### SW2

```text
enable
configure terminal

interface range gigabitEthernet 0/1 - 2
 channel-group 1 mode active
exit

interface port-channel 1
 switchport mode trunk
exit

end
```

Both switches are configured in LACP Active mode, so both sides actively negotiate the aggregation.

## Configuration considerations

The physical interfaces participating in the same Port-Channel should have compatible configurations. For a trunk Port-Channel, the following should be consistent across both sides:

- Trunk mode
- Allowed VLANs
- Native VLAN
- Encapsulation, where applicable
- Speed and duplex compatibility
- Other platform-specific Layer 2 parameters

It's generally preferable to configure common Layer 2 characteristics on the Port-Channel interface itself, rather than configuring conflicting values independently on the member interfaces.

## Verification

```text
show etherchannel summary
```
Verifies that the EtherChannel formed correctly.

```text
show interfaces port-channel 1
```
Verifies the logical interface.

```text
show interfaces trunk
```
Verifies the trunk.

The member interfaces should appear as part of the Port-Channel and should be operational.

