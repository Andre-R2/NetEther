# Internet Layer

The Internet Layer is responsible for addressing and routing data across interconnected networks. Its primary function is to deliver packets from their source to their destination, regardless of how many networks they must traverse along the way.

This layer makes global communication possible. It is the reason why a packet can travel from one side of the world to another using IP addressing and routing.

Unlike the Data Link Layer, which only delivers frames between directly connected devices, the Internet Layer is responsible for delivering packets across multiple interconnected networks using logical addressing.

Routers examine the source and destination IP addresses contained in each packet. They then consult their routing tables to determine the next hop or the most efficient path toward the destination network.

## Protocol Data Unit (PDU)

The Protocol Data Unit (PDU) of the Internet Layer is the packet.

At this layer, the data received from the Transport Layer is encapsulated by adding an IP header containing information required for routing, such as the source IP address and destination IP address.

## Common Protocols

Some of the most important protocols operating at this layer include:

- IPv4
- IPv6
- ICMP
- IPsec

## Main Device

The primary device associated with the Internet Layer is the router.

Routers are responsible for forwarding packets between different networks. They examine destination IP addresses, consult their routing tables, and determine the next hop that brings the packet closer to its final destination.

![Layer 3 IPv4 Packet](../../assets/net-assets/ipv4-packet.svg)

## Without the Internet Layer

Without the Internet Layer, communication would only be possible between devices connected to the same local network.

There would be no standardized way to route packets across multiple interconnected networks, making global communication over the Internet impossible.
