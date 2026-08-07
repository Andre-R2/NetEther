# EUI-64

## Overview

Extended Unique Identifier 64 (EUI-64) is a method used to automatically generate the 64-bit interface identifier from a device's 48-bit MAC address.

As we already know, an IPv6 address is normally split into two parts:

- The first 64 bits: the network prefix
- The last 64 bits: the interface identifier (Interface ID)

With EUI-64, those last 64 bits are built automatically using the device's MAC address.

## Example

Suppose a network card has the following MAC address:

```text
00:1A:2B:3C:4D:5E
```

This address is 48 bits, but IPv6 needs a 64-bit interface identifier, so EUI-64 performs the following procedure:

### 1. Split the MAC into two halves

```text
00:1A:2B
3C:4D:5E
```

### 2. Insert FF:FE between both halves

```text
00:1A:2B:FF:FE:3C:4D:5E
```

This already gives us a 64-bit identifier.

### 3. Flip the Universal/Local (U/L) bit

The next step is to flip the Universal/Local (U/L) bit, which corresponds to the 7th bit of the first byte of the MAC address.

In this example, the first byte is:

```text
00
```

In binary:

```text
00000000
```

Flipping the U/L bit gives:

```text
00000010
```

Which in hexadecimal is:

```text
02
```

So the original MAC:

```text
00:1A:2B:3C:4D:5E
```

becomes:

```text
02:1A:2B:FF:FE:3C:4D:5E
```

## Resulting IPv6 address

Suppose a router advertises the following prefix via Router Advertisement:

```text
2001:db8:1234:1::/64
```

The device uses that prefix and automatically generates the interface identifier via EUI-64, producing the following IPv6 address:

```text
2001:db8:1234:1:021A:2BFF:FE3C:4D5E
```

## Disadvantage

Although EUI-64 simplifies automatic IPv6 address generation, it has a significant drawback: the interface identifier ends up directly tied to the device's MAC address.

Since a MAC address normally doesn't change, the same device can generate practically the same interface identifier across different networks — making the device easier to track and creating a privacy problem.

For this reason, modern operating systems typically use Privacy Extensions, generating randomized interface identifiers instead of relying on EUI-64.

---
