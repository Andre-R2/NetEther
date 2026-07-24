# Wildcard Masks

## Overview

A **Wildcard Mask** is a mask used to specify which bits of an IP address must match and which bits can vary during a comparison.

Imagine you need to apply a policy to multiple IP addresses. To do that, you need a way to specify exactly which addresses the policy should apply to. This is precisely what wildcard masks do — they act as a filter, looking for **matches** between an IP address and a defined pattern.

## Example

Suppose you want to apply a security policy to every host in the following network:

```text
192.168.1.0/24
```

Without a wildcard, you'd have to specify every host individually — highly inefficient. With a wildcard mask, you simply configure:

```text
192.168.1.0 0.0.0.255
```

This means:

- The first three octets (**192.168.1**) **must match**.
- The last octet **can vary**.

In a wildcard mask:

- **0** → the corresponding bit **must match**
- **1** → the corresponding bit **can vary** (ignored during the comparison)

Since wildcard masks are commonly represented by full octets: **0** means the entire octet must match, **255** means the entire octet can vary.

If instead you wanted to apply a rule to a single host, you'd configure:

```text
192.168.1.5 0.0.0.0
```

Here, all four octets must match exactly — the rule only matches host **192.168.1.5**.

!!! note "Rule of thumb"
    `0` → must match · `1` (or `255` per octet) → can vary

## Advantages

Wildcard masks allow you to:

- Represent ranges of IP addresses compactly
- Reduce the number of rules in **Access Control Lists (ACLs)** and routing protocols
- Simplify network configurations
- Make IP address comparisons more flexible by specifying exactly which bits are relevant

## Relationship with subnet masks

A wildcard mask is the logical inverse of a subnet mask:

- A **subnet mask** defines which bits belong to the **network** and which belong to the **host**.
- A **wildcard mask** defines which bits **must be compared** and which bits **can be ignored** during a match.

---

**Next:** Route summarization