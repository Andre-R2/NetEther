# Routing Overview

Routing is the process of selecting the best path for a data packet to travel from a source to a destination across a network.

The main difference between routing and switching is that routing is primarily a Layer 3 concept, while switching is primarily a Layer 2 concept. Both perform forwarding functions, but they operate at different layers and use different types of addressing.

When an endpoint sends a packet to a destination outside its local network, it sends the packet to its default gateway. The default gateway is a Layer 3 device that performs routing decisions between different networks. It can be a router, firewall, multilayer switch, or any other device capable of performing Layer 3 routing.

## Routing Table

A routing table is a data structure stored on a router or other Layer 3 device. It acts as a map that tells the device how to reach different destination networks.

A routing table can learn routes in several ways, including:

- Directly connected networks
- Manually configured static routes
- Dynamically learned routes through routing protocols

When a packet arrives at a router, the router examines the IP header, particularly the destination IP address.

The router then looks for the best matching route in its routing table. It uses Longest Prefix Match to determine the most specific route available.

Once the router selects the route, it determines the next hop and/or outgoing interface.

The router then:

1. Decrements the TTL (Time To Live) value.
2. Builds a new Layer 2 frame appropriate for the outgoing interface.
3. Forwards the packet toward the next hop.

This process is repeated by each router along the path until the packet reaches its destination network.

A router does not determine the entire path to the destination at once. Each router makes a forwarding decision for the next hop based on its own routing table.