# MAC Spoofing

MAC Spoofing is a Layer 2 attack in which an attacker changes the MAC address of a network interface to impersonate another device on the network.

Although every Network Interface Card (NIC) is manufactured with a unique MAC address, most operating systems allow this address to be temporarily modified through software. The hardware MAC address is not permanently changed; instead, the operating system instructs the NIC to use a different MAC address while communicating on the network.

By impersonating another device, an attacker may bypass security mechanisms that rely solely on MAC address identification.

## Common uses

One common objective of MAC Spoofing is to bypass MAC-based access controls.

For example, suppose a switch port is configured to allow only the MAC address `AAAA.AAAA.AAAA`. If an attacker discovers that address, they can configure their own device to use the same MAC and potentially gain unauthorized access to the network.

MAC Spoofing is also commonly used together with other Layer 2 attacks, such as ARP Spoofing, where impersonating another device helps redirect traffic through an attacker-controlled machine.

For this reason, MAC addresses alone should never be considered a secure authentication mechanism.

