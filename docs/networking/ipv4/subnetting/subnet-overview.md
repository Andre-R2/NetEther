# Subnetting

## Overview

Subnetting is the process of dividing a larger IP network into multiple smaller logical subnets.

Before subnetting, the traditional classful IPv4 addressing system presented two major problems: inefficient address allocation and excessive broadcast traffic.

## The problem: wasted addresses

Organizations were assigned entire network blocks regardless of how many IP addresses they actually needed. As a result, a large number of IPv4 addresses remained unused, contributing to the rapid depletion of the available address space.

## The problem: broadcast traffic

Another major issue was broadcast traffic. In a large network, broadcast messages are received by every device within the same broadcast domain, even if most of them don't need that information. As the network grows, this unnecessary traffic can reduce overall network performance.

Subnetting solves this problem by dividing a large network into smaller logical subnets. Each subnet becomes its own broadcast domain, keeping broadcast traffic isolated and preventing it from spreading across the entire network.

## Other benefits

**Easier management.** Subnetting allows departments, buildings, or physical locations to be organized into separate subnets. This makes troubleshooting, administration, and future network expansion much easier.

**Contained impact.** Although subnetting isn't a security mechanism by itself, the logical separation between subnets helps contain failures and limits the impact of security incidents or malware outbreaks to a single network segment.

**Scalable addressing.** Instead of assigning one large network to every organization, networks can be designed according to actual requirements, making much more efficient use of the available IPv4 address space.

## How subnetting works

Subnetting works by **borrowing bits from the host portion** of an IPv4 address and using them to create additional network identifiers. This allows a single network to be divided into multiple smaller subnets.

Subnetting relies on **subnet masks** and **CIDR prefix notation** to define how the network and host portions of an IPv4 address are divided. 

---
