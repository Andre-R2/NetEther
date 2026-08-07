# IPv6 Prefixes

## Overview

In IPv6, the prefix length works the same way as in IPv4: it indicates how many of the initial bits of an IPv6 address represent the network portion, while the remaining bits identify the interface within that network. It's the IPv6 equivalent of the CIDR prefix used in IPv4.

## Example

```text
2001:db8:1234:5678::/64
```

The `/64` indicates that `2001:db8:1234:5678` is the network prefix. The remaining 64 bits identify each device within that network.

## Why /64 is the standard

`/64` is the standard prefix length in most IPv6 networks because it works correctly with SLAAC (Stateless Address Autoconfiguration), where devices generate their own IPv6 address automatically. It's also the block size expected by many protocols and operating systems.

## Common prefix lengths

| Prefix length | Meaning | Common use |
|---|---|---|
| /128 | A single address | Route to a specific host |
| /127 | Two addresses | Point-to-point links |
| /64 | One standard subnet | LANs and user networks |
| /56 | 256 /64 subnets | Homes or small businesses |
| /48 | 65,536 /64 subnets | Enterprises or large sites |
| /32 | Typical ISP allocation | Internet providers |

---
