# Public vs Private IPv4 Addresses

## Overview

Originally, every IPv4 address was public.

During the early days of ARPANET and the Internet, the network was relatively small, and the IPv4 address space of approximately 4.3 billion addresses seemed practically unlimited. As a result, there was no distinction between public and private addresses.

However, as the commercial Internet expanded during the early 1990s, the number of connected organizations, home networks, and devices grew rapidly. At the same time, the classful addressing system wasted large blocks of IPv4 addresses, accelerating address exhaustion.

CIDR, introduced in 1993, significantly improved address allocation efficiency, but it could not completely solve the problem.

To further conserve the remaining IPv4 address space, RFC 1918 introduced private IPv4 address ranges. Devices inside private networks could use these internal addresses, while only the network's router required a globally unique public IPv4 address.

## Public IPv4 Addresses

A public IPv4 address is globally unique and routable across the Internet.

Public addresses are assigned by an Internet Service Provider (ISP) and allow networks to communicate with devices outside their local network.

Public addresses may be either static or dynamically assigned.

## Private IPv4 Addresses

Private IPv4 addresses are intended for use only inside private networks.

They are never routed across the public Internet and can be reused by millions of independent networks without causing conflicts.

The three private IPv4 ranges are:

| Range | CIDR |
|--------|------|
| 10.0.0.0 – 10.255.255.255 | /8 |
| 172.16.0.0 – 172.31.255.255 | /12 |
| 192.168.0.0 – 192.168.255.255 | /16 |

Communication between private networks and the public Internet is made possible through Network Address Translation (NAT), which allows multiple private devices to share a single public IPv4 address.

