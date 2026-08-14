# ARP Spoofing

ARP spoofing, also known as ARP poisoning, is a Layer 2 attack in which an attacker sends forged ARP messages to associate their own MAC address with the IP address of another device, such as the default gateway or another host on the network.

ARP (Address Resolution Protocol) is responsible for mapping IPv4 addresses to MAC addresses within a local area network.

Whenever a device needs to communicate with another host on the same LAN, it first checks its ARP cache. If no entry exists, it broadcasts an ARP Request asking, "Who has this IP address?". The device that owns that IP address responds with an ARP Reply containing its MAC address.

The requesting device then stores this IP-to-MAC mapping in its ARP cache and uses it to build Ethernet frames.

The main problem is that ARP provides no authentication. Any device on the LAN can send an ARP Reply, even if nobody requested it. Most operating systems automatically update their ARP cache with the latest information they receive.

## ARP cache poisoning

An attacker exploits this behavior by sending forged ARP Reply messages to the victim.

For example, the attacker claims that the IP address of the default gateway corresponds to the attacker's MAC address instead of the gateway's legitimate MAC address.

As a result, the victim updates its ARP cache with the malicious mapping and begins sending all traffic intended for the default gateway to the attacker instead.

Once the victim's ARP cache has been poisoned, the attacker can:

- Intercept network traffic.
- Perform Man-in-the-Middle (MitM) attacks.
- Capture usernames, passwords, and other sensitive information.
- Modify packets before forwarding them to their destination.
- Drop packets to perform a Denial-of-Service (DoS) attack.

To avoid raising suspicion, attackers usually enable IP forwarding on their machine so that intercepted traffic continues to reach the legitimate destination. This allows communication to continue normally while the attacker silently monitors or manipulates the traffic.

## Mitigation

The most effective mitigation against ARP spoofing is Dynamic ARP Inspection (DAI).

DAI validates ARP packets against the trusted IP-to-MAC bindings learned through DHCP Snooping. If an ARP packet contains invalid or unexpected information, the switch immediately drops it before it reaches other devices on the network.

Static ARP entries can also prevent ARP spoofing because manually configured IP-to-MAC mappings cannot be overwritten by forged ARP replies. However, this approach is generally impractical in enterprise networks due to the administrative overhead required to maintain static entries for every device.