# Network, Broadcast and Host Addresses

When a subnet is created, not every IPv4 address within that subnet can be assigned to devices.

Two addresses are always reserved because they perform special functions within the network.

The first address is the Network Address. It uniquely identifies the subnet itself and is used by routers and hosts to recognize the network to which an IP address belongs. Because it identifies the entire subnet, it cannot be assigned to an individual device.

The last address is the Broadcast Address. It is reserved for sending a packet to every host within the subnet simultaneously. Since this address represents all devices on the subnet, it also cannot be assigned to a single host.

As a result, the **assignable host addresses** are all the addresses between the Network Address and the Broadcast Address.

For example:

```text
Network Address      192.168.1.0
First Host           192.168.1.1
...
Last Host            192.168.1.254
Broadcast Address    192.168.1.255
```

This means that a **/24** network contains 256 total addresses, but only 254 are usable for hosts because the first and last addresses are reserved.