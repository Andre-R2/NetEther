# Traffic Delivery Methods

Traffic delivery methods describe how information is transmitted between devices across a network.

The four primary traffic delivery methods are Unicast, Broadcast, Multicast and Anycast. Each one determines how data is delivered from a sender to one or more receivers.

## Unicast

Unicast is a communication method where one device sends data to a single destination. It establishes a one-to-one communication between a sender and a specific receiver.

It is the most common type of traffic on modern networks. Data is delivered exclusively to the intended destination; no other devices receive the traffic except the networking equipment responsible for forwarding it, such as switches and routers.

Most client-server communications use Unicast, including web browsing, SSH sessions, file transfers and the majority of Internet services.

## Broadcast

Broadcast is a communication method that sends data to every device within the same local network or broadcast domain.

Unlike Unicast, the sender does not need to know the address of every device on the network. A single broadcast message is transmitted and all hosts in the LAN receive it.

Routers do not forward broadcast traffic, meaning Broadcast is limited to the local network.

Broadcast is primarily used for device and service discovery. Protocols such as ARP and DHCP rely on broadcast messages during part of their operation.

## Multicast

Multicast allows one sender to transmit data to a specific group of receivers.

Unlike Broadcast, where every device receives the traffic, Multicast delivers data only to devices that have joined the multicast group.

This significantly reduces bandwidth consumption by avoiding multiple copies of the same data. It is commonly used for IPTV, video conferencing, live streaming and routing protocols such as OSPF. Multicast traffic is commonly carried over UDP due to its low overhead.

## Anycast

Anycast is a traffic delivery method in which multiple servers share the same IP address.

When a client sends traffic to that address, routers use their routing tables to forward the traffic only to the server offering the best available path, which is usually the closest one in terms of network topology.

Anycast is widely used by global DNS providers, CDNs and cloud platforms because it improves availability, reduces latency and distributes traffic efficiently across multiple geographic locations.

## Comparison

| Method | Communication | Receivers | Typical use cases |
|----------|---------------|-----------|-------------------|
| Unicast | One → One | A single destination | HTTP, HTTPS, SSH, FTP |
| Broadcast | One → All | Every device in the local LAN | ARP, DHCP Discover |
| Multicast | One → Many | Only members of a multicast group | IPTV, Streaming, OSPF |
| Anycast | One → Closest | The server with the best available route | DNS, CDNs, Cloud Services |