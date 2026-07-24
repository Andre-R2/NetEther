# VLSM Practice Exercises

## Exercise 1 — /24, single octet

**Network:** `192.168.10.0/24`

| Subnet | Hosts | +2 | Prefix | Usable |
|---|---|---|---|---|
| A | 50 | 52 → 64 | /26 | 62 |
| B | 25 | 27 → 32 | /27 | 30 |
| C | 10 | 12 → 16 | /28 | 14 |
| D | 2 | 4 → 4 | /30 | 2 |

Sorted largest → smallest: A, B, C, D.

| Subnet | Network | Range | Broadcast |
|---|---|---|---|
| A | 192.168.10.0/26 | .1–.62 | .63 |
| B | 192.168.10.64/27 | .65–.94 | .95 |
| C | 192.168.10.96/28 | .97–.110 | .111 |
| D | 192.168.10.112/30 | .113–.114 | .115 |

---

## Exercise 2 — /16, octet crossing

**Network:** `172.16.0.0/16`

| Subnet | Hosts | +2 | Prefix | Usable |
|---|---|---|---|---|
| Sales | 500 | 502 → 512 | /23 | 510 |
| Engineering | 300 | 302 → 512 | /23 | 510 |
| IT | 100 | 102 → 128 | /25 | 126 |
| Guest | 50 | 52 → 64 | /26 | 62 |

Sorted largest → smallest: Sales, Engineering, IT, Guest.

/23 block = 2 (3rd octet) · /25 block = 128 (4th octet) · /26 block = 64 (4th octet)

| Subnet | Network | Range | Broadcast |
|---|---|---|---|
| Sales | 172.16.0.0/23 | 172.16.0.1–172.16.1.254 | 172.16.1.255 |
| Engineering | 172.16.2.0/23 | 172.16.2.1–172.16.3.254 | 172.16.3.255 |
| IT | 172.16.4.0/25 | 172.16.4.1–172.16.4.126 | 172.16.4.127 |
| Guest | 172.16.4.128/26 | 172.16.4.129–172.16.4.190 | 172.16.4.191 |

> Sales/Eng math happens in the 3rd octet (/23), IT/Guest in the 4th (/25, /26) — same parent, different octet, depending on prefix.

---

**Next:** Wildcard masks