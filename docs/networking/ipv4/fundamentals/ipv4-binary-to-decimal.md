# Binary to Decimal

## Overview

Binary is the numbering system used internally by computers and digital devices to represent data and instructions. It uses only two digits, **0** and **1**.

Computers use binary because modern hardware is built from billions of microscopic electronic switches called **transistors**. A transistor has only two stable states: **on** and **off**, making binary the natural way to represent information. A value of **1** represents the on state, while **0** represents the off state.

As a result, all information processed by a computer is ultimately represented in binary, including IPv4 addresses.

Humans, however, use the decimal numbering system because it is much easier to read and interpret. For example, the IPv4 address `192.168.1.1` is far easier to recognize than its binary representation.

## Bits and Bytes

A **bit** (binary digit) is the smallest unit of information in computing. A bit can have only two possible values: **0** or **1**.

To represent more complex information, bits are grouped together. A group of **8 bits** is called a **byte** (or **octet** in networking).

An IPv4 address is **32 bits** long, which is equivalent to **4 bytes (4 octets)**.

Since an octet contains 8 bits, it can represent **256 different values**, ranging from **0** to **255**. This is why each octet in an IPv4 address can only have a decimal value between **0** and **255**.

## Binary Place Values

In the binary numbering system, each bit position has a specific value. Starting from the right, each position is worth twice the value of the previous one.

```
128 | 64 | 32 | 16 | 8 | 4 | 2 | 1
```

Each position represents a power of two, allowing any number between **0** and **255** to be represented using eight bits.

### Example: Binary to Decimal Conversion

![Binary to decimal](../../../assets/net-assets/convert-binary-example-light.svg#only-light)
![Binary to decimal](../../../assets/net-assets/convert-binary-example-dark.svg#only-dark)

*In this example, we convert the binary number 00001111 to its decimal equivalent, which is the number 15*

## IPv4 Representation

The same conversion process is used for IPv4 addresses. Instead of converting a single octet, an IPv4 address contains **four independent octets**, each converted separately between binary and decimal.

![IPv4 Address Representation](../../../assets/net-assets/ipv4-address-rep-light.svg#only-light)
![IPv4 Address Representation](../../../assets/net-assets/ipv4-address-rep-dark.svg#only-dark)

*Now, we perform the same process but converting an IPv4 address: 192.168.100.30 into its binary equivalent 11000000 10101000 01100100 00011110*

Understanding binary is essential because technologies such as **subnet masks, CIDR notation, subnetting, and VLSM** all rely on binary calculations.

---

**Next:** IPv4 Address Structure
