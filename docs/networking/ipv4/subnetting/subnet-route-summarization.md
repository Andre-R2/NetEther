# Route Summarization

## Overview

Route Summarization (also known as **route aggregation**) is the technique of combining multiple contiguous network routes into a single, more general route by using a shorter network prefix.

Its main objective is to reduce the size of routing tables and make the routing process more efficient by allowing multiple contiguous subnets to be represented as a single summarized route.

This reduces the amount of routing information exchanged between routers, lowers CPU and memory consumption, and improves network scalability.

## Requirements

Route Summarization can only be performed when the subnets satisfy two conditions:

- They are **contiguous**.
- They belong to the **same valid address block** determined by the summary prefix.

For example:

```text
172.16.0.0/24
172.16.1.0/24
172.16.2.0/24
172.16.3.0/24
```

These four consecutive /24 networks belong to the same valid block, so they can be summarized as:

```text
172.16.0.0/22
```

A `/22` contains exactly four consecutive `/24` networks.

## Choosing the summary route

The general criterion is very simple:

> **Find the largest valid block that contains the greatest number of consecutive subnets.**

This is the entire idea behind Route Summarization.

For example, suppose the following networks exist:

```text
172.16.0.0/24
172.16.1.0/24

172.16.4.0/24
172.16.5.0/24
172.16.6.0/24
172.16.7.0/24
```

Notice that there is a gap between:

```text
172.16.1.0
```

and

```text
172.16.4.0
```

Since the subnets are no longer consecutive, they cannot all be summarized into a single route.

Instead, each consecutive block is summarized independently.

First block:

```text
172.16.0.0/24
172.16.1.0/24
```

Summary:

```text
172.16.0.0/23
```

Second block:

```text
172.16.4.0/24
172.16.5.0/24
172.16.6.0/24
172.16.7.0/24
```

Summary:

```text
172.16.4.0/22
```

If multiple summaries are required, each summary represents the largest valid consecutive block possible.

## Summary address

The summarized route is always represented by the **first network address of the block**, also known as the **Network ID**.

For example:

```text
172.16.4.0/24
172.16.5.0/24
172.16.6.0/24
172.16.7.0/24
```

are represented by:

```text
172.16.4.0/22
```

The first network of the block becomes the summary route.

## Relationship with VLSM

VLSM and Route Summarization are closely related but solve different problems.

- **VLSM** divides a large network into smaller subnets in order to optimize IP address allocation.
- **Route Summarization** performs the opposite operation by grouping multiple contiguous subnets into a single summarized route to optimize routing.

In other words:

- VLSM optimizes **address space**.
- Route Summarization optimizes **routing**.

## Benefits

- Reduces routing table size.
- Decreases CPU and memory usage on routers.
- Reduces routing update traffic.
- Improves network scalability.
- Simplifies routing information.

