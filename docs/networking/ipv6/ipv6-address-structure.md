## Structure

An IPv6 address is composed of 128 bits, four times larger than an IPv4 address (32 bits).

It is represented using hexadecimal notation, divided into eight groups of 16 bits, separated by colons.

Example:

```text
2001:0db8:0000:0000:0000:ff00:0042:8329
```

Each group contains four hexadecimal digits, where each digit can be any value from 0 to 9 or A to F.

In most networks, an IPv6 address is divided into two logical parts:

- 64 bits for the network prefix.
- 64 bits for the interface identifier (host portion).

## IPv6 Address Abbreviation

IPv6 addresses can be shortened using two rules.

### Rule 1: Omit leading zeros

Leading zeros in any 16-bit group may be removed.

Example:

```text
2001:0db8:0000:0000:0000:ff00:0042:8329
```

becomes

```text
2001:db8:0:0:0:ff00:42:8329
```

### Rule 2: Compress consecutive zero groups

A single continuous sequence of one or more groups containing only zeros may be replaced with a double colon (`::`).

Example:

```text
2001:db8:0:0:0:ff00:42:8329
```

becomes

```text
2001:db8::ff00:42:8329
```

> The double colon (`::`) can only be used **once** in a single IPv6 address.