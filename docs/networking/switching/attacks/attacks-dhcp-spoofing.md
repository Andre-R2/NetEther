## DHCP Spoofing

DHCP spoofing is a Layer 2 attack in which an attacker deploys a rogue DHCP server within a network. This rogue server responds to DHCP requests before the legitimate DHCP server, allowing the attacker to provide malicious network configuration to client devices.

When a new device connects to a network, it requires basic network parameters such as an IP address, subnet mask, default gateway, and DNS server. To obtain this information, the client initiates the DHCP process by broadcasting a DHCP Discover message.

Any DHCP server that receives this request can respond with a DHCP Offer. The complete DHCP process is known as DORA:

- Discover
- Offer
- Request
- Acknowledge

Normally, the client accepts the DHCP Offer from the server that responds first.

The main problem is that DHCP does not provide any mechanism to authenticate DHCP servers. As a result, a rogue DHCP server can answer faster than the legitimate one and provide malicious network settings to the victim.

For example, the attacker can assign:

- A malicious default gateway.
- A malicious DNS server.
- Incorrect IP addressing information.

Once the victim accepts this configuration, all of its network traffic can be redirected through infrastructure controlled by the attacker.

At this point, the attacker may:

- Capture and analyze network traffic.
- Perform Man-in-the-Middle (MitM) attacks.
- Redirect users to malicious websites through rogue DNS responses.
- Steal sensitive information such as credentials or session cookies.
- Manipulate or block network communications.

Because DHCP clients automatically trust the first DHCP server that responds, DHCP spoofing can compromise an entire network segment if no protection mechanisms are implemented.

The most effective mitigation against DHCP spoofing is DHCP Snooping. This Layer 2 security feature allows switches to distinguish between trusted and untrusted ports, ensuring that DHCP responses are accepted only from legitimate DHCP servers while blocking unauthorized DHCP Offer and DHCP Acknowledge messages sent by rogue devices.