# Prefix Length

The **prefix length** indicates how many consecutive bits, starting from the left, belong to the Network ID.

A **/24** prefix means that the first 24 bits identify the network, while the remaining 8 bits identify the hosts.

Similarly:

| Prefix | Network Bits | Host Bits |
|--------|-------------:|----------:|
| /8     | 8            | 24 |
| /16    | 16           | 16 |
| /24    | 24           | 8 |
| /25    | 25           | 7 |
| /26    | 26           | 6 |
| /27    | 27           | 5 |
| /30    | 30           | 2 |

As the prefix length increases, more bits are allocated to the network, leaving fewer bits available for hosts. This creates smaller subnets with fewer usable IP addresses.

Conversely, a shorter prefix leaves more bits for hosts, creating larger networks.