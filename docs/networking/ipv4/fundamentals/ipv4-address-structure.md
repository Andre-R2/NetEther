# IPv4 Address Structure

An IPv4 address is 32 bits long and is divided into four 8-bit groups known as octets.

![IPv4 Structure](../../../assets/net-assets/ipv4-structure-light.svg#only-light)
![IPv4 Structure](../../../assets/net-assets/ipv4-structure-dark.svg#only-dark)

Although computers process IPv4 addresses as binary values, they are commonly represented using dotted-decimal notation to make them easier for humans to read and write.

Every IPv4 address consists of two logical portions:

- The **network portion**, which identifies the destination network.
- The **host portion**, which identifies a specific network interface within that network.

![IPv4 portion](../../../assets/net-assets/ipv4-portion-light.svg#only-light)
![IPv4 portion](../../../assets/net-assets/ipv4-portion-dark.svg#only-dark)

All devices connected to the same network share the same network portion, while the host portion must be unique within that network.

Just as MAC addresses identify network interfaces at Layer 2, IPv4 addresses identify those same interfaces at Layer 3. For this reason, every network interface requires both a MAC address and an IPv4 address.

The boundary between the network and host portions is determined by the subnet mask, which specifies how the 32 bits are divided. Subnet masks are covered in detail later in this module.