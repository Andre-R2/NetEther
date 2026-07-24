# Variable Length Subnet Mask (VLSM)

## Overview

Variable Length Subnet Mask (VLSM) is a subnetting technique that allows different subnet masks to be assigned within the same parent network. Unlike traditional subnetting, where every subnet has the same size, VLSM lets each subnet have a size that matches its actual requirements.

Traditional subnetting has one major limitation: it's rigid. Once a network is divided, every subnet has the same prefix length and therefore the same number of available hosts. While this makes the design simple, it often wastes a large number of IP addresses.

For example, imagine a company that needs three different networks:

- One network for 100 devices
- One network for 30 devices
- One network for only 2 devices

With traditional subnetting, all three subnets would have the same size, meaning many addresses would never be used. VLSM solves this problem by allowing each subnet to receive only the number of addresses it actually needs.

## How VLSM works

The overall process is almost identical to traditional subnetting. The main difference is that every subnet can have a different prefix length.

The recommended process is:

1. Sort the required subnets from the largest to the smallest number of hosts.
2. Determine the appropriate prefix for each subnet.
3. Assign the first available block to the largest subnet.
4. Continue assigning the remaining subnets using the next available valid block.

Sorting subnets from largest to smallest is important because it prevents fragmentation. Large networks are allocated first, while smaller networks can easily fit into the remaining address space without creating conflicts or wasting addresses.

## Choosing the correct prefix

For each subnet, the first step is to determine how many host bits are required.

- Add **2** to the number of hosts needed — one for the **Network ID** and one for the **Broadcast Address**, since both are always reserved.
- Find the smallest power of two that is greater than or equal to that number.
- Once the required number of host bits is known, subtract it from the **32 bits of an IPv4 address** to obtain the subnet prefix.

### Example

A subnet needs **25 hosts**.

Add the 2 reserved addresses: `25 + 2 = 27` addresses minimum.

```text
2⁴ = 16   → too small
2⁵ = 32   → enough
```

**32** is the smallest power of two that covers 27 addresses, so **5 host bits** are required.

Subtract those 5 host bits from the 32 bits of an IPv4 address:

```text
32 (bits in an IPv4 address) − 5 (host bits) = 27
```

Therefore, the subnet uses a **/27** prefix.

## Valid network boundaries

One of the most important concepts in VLSM is understanding that **the next subnet does not necessarily begin at the next IP address**.

Every subnet must start at a **valid network boundary**, which depends on its block size.

For example, a **/28** subnet has a block size of **16**. Therefore, valid network IDs are:

```text
0, 16, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240
```

A /28 subnet **cannot** begin at addresses such as `49`, `75`, `113`, or `150` — even if those addresses are technically free.

If the next available IP doesn't match a valid boundary, the allocation must move forward to the next valid block. This is one of the most common mistakes when learning VLSM.

## Traditional subnetting vs VLSM

| | Traditional subnetting | VLSM |
|---|---|---|
| Subnet sizes | All equal | Different, based on need |
| Address efficiency | Wastes addresses | Maximizes usage |
| Complexity | Simpler | Requires tracking block boundaries |

## Summary

The basic calculations used in VLSM are exactly the same as in traditional subnetting. The difference lies in the allocation strategy:

- Calculate the required prefix for each subnet.
- Sort subnets from largest to smallest.
- Assign each subnet to the first available valid network boundary.
- Repeat until all subnets have been allocated.

In practice, VLSM is less about learning new formulas and more about correctly applying subnetting while respecting block boundaries and allocating address space efficiently.

---

**Next:** Wildcard masks