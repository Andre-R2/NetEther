# Dynamic ARP Inspection (DAI)

Dynamic ARP Inspection (DAI) is a Layer 2 security feature that protects a network against ARP spoofing attacks by validating ARP packets before they are forwarded.

ARP has the same fundamental weakness as DHCP: it provides no authentication. Any device connected to the network can claim to own any IP address.

When a host needs to send traffic outside its local network, it already knows the destination IP address (the default gateway). However, to build the Ethernet frame, it also needs the gateway's MAC address.

To obtain this information, the host broadcasts an ARP Request asking, "Who has this IP address?". Every device on the LAN receives the request, but only the legitimate gateway should respond with an ARP Reply containing its MAC address. The requesting host then stores this IP-to-MAC mapping in its ARP cache.

The problem is that an attacker connected to the same network can send a forged ARP Reply pretending to be the default gateway. The attacker does not even need to receive an ARP Request first. ARP allows unsolicited ARP messages, and many operating systems accept them in order to keep their ARP caches up to date.

As a result, the victim updates its ARP cache with the attacker's MAC address instead of the legitimate gateway's MAC address. From that moment on, traffic intended for the router is redirected to the attacker, making Man-in-the-Middle (MitM) attacks possible.

## DHCP Snooping Binding Table

Dynamic ARP Inspection relies on the DHCP Snooping Binding Table.

As DHCP clients successfully obtain their network configuration, DHCP Snooping builds a trusted table containing the relationship between:

- IP address
- MAC address
- VLAN
- Switch interface

Because this information is learned from the legitimate DHCP server, it is considered trustworthy.

## ARP validation

Dynamic ARP Inspection intercepts every ARP packet arriving on untrusted ports before forwarding it.

For each ARP packet, the switch compares the sender's IP address and MAC address against the corresponding entry in the DHCP Snooping Binding Table.

- If the information matches, the ARP packet is forwarded normally.
- If the information does not match, the switch immediately drops the packet.

By validating ARP packets against the trusted DHCP Snooping Binding Table, Dynamic ARP Inspection effectively prevents ARP spoofing attacks and protects hosts from malicious IP-to-MAC mappings.