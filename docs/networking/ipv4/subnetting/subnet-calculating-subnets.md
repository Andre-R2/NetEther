# Calculating Subnets

## Overview

Subnetting consists of borrowing bits from the host portion and converting them into network bits in order to create multiple logical subnets.

For example, consider the following network:

```text
192.168.1.0/24
```

Suppose four subnets are required. Since:

```text
2² = 4
```

two bits must be borrowed from the host portion.

Original:

```text
Network:  11111111.11111111.11111111
Host:     00000000
```

After borrowing two bits:

```text
11111111.11111111.11111111.11000000
```

The network prefix changes from **/24** to **/26**.

Borrowing additional network bits reduces the number of available host bits while increasing the number of available subnets.

## Subnet calculations

| What | Formula |
|---|---|
| Number of subnets | `2^(borrowed bits)` |
| Number of usable hosts | `2^(host bits) − 2` |

Two addresses are reserved in every subnet, which is why usable hosts is always `2^n − 2`:

- **Network Address** — identifies the subnet itself
- **Broadcast Address** — used to reach every host within that subnet

## Subnet increment (block size)

The subnet increment — also called the **block size** — is calculated as:

```text
256 − Subnet Mask value (of the interesting octet)
```

The "interesting octet" is simply the octet where the subnet mask stops being `255` and isn't yet `0` — that's where the borrowed bits live, and where the actual math happens.

The increment represents the distance between the starting addresses of consecutive subnets.

## Address calculation shortcuts

Once the current Network Address and the subnet increment are known, everything else follows automatically:

- **First Host** = Current Network + 1
- **Next Network** = Current Network + Increment
- **Broadcast** = Next Network − 1
- **Last Host** = Broadcast − 1

## Putting it all together

Back to the original example — `192.168.1.0/24` split into 4 subnets (`/26`, increment = `256 − 192 = 64`):

| Subnet | Network Address | First Host | Last Host | Broadcast |
|---|---|---|---|---|
| 1 | 192.168.1.**0**/26 | .1 | .62 | .63 |
| 2 | 192.168.1.**64**/26 | .65 | .126 | .127 |
| 3 | 192.168.1.**128**/26 | .129 | .190 | .191 |
| 4 | 192.168.1.**192**/26 | .193 | .254 | .255 |

Notice how each Network Address is just the previous one plus the increment (`64`) — and every Broadcast is one less than the *next* subnet's Network Address. This is the entire mechanism behind subnetting: once you know the increment, the rest is just addition and subtraction.

---

**Next:** Subnetting by hosts