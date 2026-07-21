# Reserved IPv4 Addresses

## Overview

Not every IPv4 address was designed to identify a host. Some address ranges were reserved by the IPv4 protocol to perform specific networking functions rather than identify network interfaces.

These reserved addresses solve common networking problems such as local testing, automatic address assignment, multicast communication, and address conservation.

## Reserved IPv4 Address Ranges

### Loopback (`127.0.0.0/8`)

The TCP/IP stack requires a way for a device to communicate with itself without sending traffic onto the physical network.

The loopback range allows applications and services running on the same device to communicate internally. The operating system detects these addresses and ensures that the traffic never leaves the local machine.

---

### Limited Broadcast (`255.255.255.255`)

Sometimes a device needs to send a message to every host on the local network without knowing their individual IPv4 addresses.

The limited broadcast address allows a packet to be delivered to every device within the local broadcast domain.

---

### Multicast (`224.0.0.0/4`)

Sending the same packet individually to multiple devices is inefficient.

Multicast allows a sender to transmit a single packet to a multicast group. The network then delivers that traffic only to devices that have joined the group, reducing unnecessary bandwidth usage.

---

### Link-Local (`169.254.0.0/16`)

Normally, devices obtain an IPv4 address from a DHCP server.

If no DHCP server is available, the operating system can automatically assign an address from the link-local range. This allows devices on the same local network to continue communicating even without manual configuration.

---

### Private Address Ranges

- `10.0.0.0/8`
- `172.16.0.0/12`
- `192.168.0.0/16`

Instead of assigning a unique public IPv4 address to every device, private address ranges allow thousands of devices inside different private networks to reuse the same addresses.

Communication with the public Internet is made possible through Network Address Translation (NAT), allowing multiple devices to share a single public IPv4 address. This significantly extended the usable lifetime of IPv4.

---

### Unspecified Address (`0.0.0.0`)

A device does not always have an IPv4 address.

For example, during the DHCP process, a host has not yet received an address from the DHCP server. In this situation, it uses the unspecified address (`0.0.0.0`) to indicate that no IPv4 address has been assigned yet.

> **Note:** Later, in the Routing module, you'll encounter `0.0.0.0/0`, which represents the default route. Although they look similar, they have different meanings depending on the context.

---

**Next:** IPv4 Address Classes