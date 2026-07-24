# CIDR Notation

CIDR (**Classless Inter-Domain Routing**) is a notation used to represent an IPv4 address together with its network prefix.

For example:

```text
192.168.10.0/24  
```

The number after the forward slash (/) indicates how many of the 32 bits belong to the Network ID.

For example, a /24 prefix means that the first 24 bits identify the network, while the remaining 8 bits identify the hosts within that network.

Unlike the traditional classful addressing system, CIDR no longer relies on fixed network classes (A, B, or C). Instead, the network boundary is determined solely by the prefix length.

This flexible addressing model allows networks to be allocated according to their actual size, significantly reducing IPv4 address waste.

CIDR also made it possible to use **Variable Length Subnet Masks (VLSM)**, allowing network administrators to create subnets of different sizes based on the number of required hosts instead of being limited by the rigid boundaries imposed by the classful addressing system.