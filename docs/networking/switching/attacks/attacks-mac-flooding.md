# MAC Flooding

MAC Flooding is a Layer 2 attack in which an attacker overwhelms a switch's CAM table by sending thousands of Ethernet frames with spoofed source MAC addresses.

Switches use the CAM table to store the association between MAC addresses and the ports where they were learned. Since the CAM table has limited memory, filling it with fake MAC addresses eventually prevents the switch from learning legitimate devices.

Every time a switch receives an Ethernet frame, it learns the source MAC address and associates it with the incoming port.

For example:

```text
MAC AAAA → Gi0/24
MAC BBBB → Gi0/24
MAC CCCC → Gi0/24
```

Under normal operation, these entries allow the switch to forward frames only to the correct destination port.

During a MAC Flooding attack, an attacker connects to the switch and begins sending thousands of Ethernet frames, each one containing a different spoofed source MAC address.

The switch treats every new MAC address as a different device and keeps adding new entries to its CAM table until it reaches its maximum capacity.

Once the CAM table is full, the switch can no longer learn additional MAC addresses.

As a result, when the switch receives frames destined for unknown MAC addresses, it no longer knows which port they belong to. Instead of forwarding them to a single interface, it floods those frames out of all ports within the same VLAN.

This behavior allows an attacker to receive traffic that would normally be forwarded only to its intended destination, making MAC Flooding one of the techniques used to perform traffic sniffing on a switched network.