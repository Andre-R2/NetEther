# SLAAC

## Overview

SLAAC (Stateless Address Autoconfiguration) is a method that allows a device to create its own unique IPv6 address and join a network without needing a traditional DHCP server.

## How the process works

When a device starts up and needs its network parameters, it sends a message to the multicast group where routers listen — `ff02::2` (all-routers).

The router, depending on how the process is configured, responds with the network prefix (the first 64 bits) and also tells the device whether it should query a DHCPv6 server or generate its own host identifier through SLAAC.

## SLAAC address generation methods

SLAAC is the overall autoconfiguration process, but there are several ways it can generate the Interface ID: EUI-64, Privacy Extensions, and Stable Privacy.

### Privacy Extensions

Instead of deriving the Interface ID from the MAC address (like EUI-64 does), this method generates a completely random value, and after a period of time the address changes periodically.

### Stable Privacy

The most widely used method today. The system calculates the Interface ID (the remaining 64 bits) using a hash function, based on information such as the network prefix, an internal secret stored on the device, and the interface name.

On a given network, the device keeps the same address. Connect to a different network, and it generates a different one. It's stable, doesn't expose the MAC address, and makes it harder to track the device across networks — which is exactly why it's the most common choice today.

!!! note
    Many modern operating systems actually use both methods at the same time, not just one: a stable address (Stable Privacy) for incoming connections, like hosting a service, and a temporary, rotating address (Privacy Extensions) for outbound connections, so browsing traffic can't be easily correlated back to the same device over time.

## Who chooses the method?

The method is chosen by the operating system — using EUI-64, Privacy Extensions, or Stable Privacy depends entirely on the client, not on the router or the network.

---

