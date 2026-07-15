# Application Layer

## Overview

The Application Layer is the layer closest to the user. It's responsible for providing the services and protocols that applications use to communicate across the network.

This layer solves the problem that different types of applications or services need to communicate in different ways.

The Internet isn't only used to access web pages, but also to send emails, resolve domain names, transfer files, manage servers, make video calls, and many other services. Each of these exchanges information following specific rules.

For this reason, the Application Layer defines the protocols that allow two applications to understand each other and exchange information correctly.

## Common protocols

| Protocol | Purpose | Common port |
|---|---|---|
| HTTP | Web browsing | 80 |
| HTTPS | Secure web browsing | 443 |
| DNS | Domain name resolution | 53 |
| SMTP | Sending email | 25 |
| FTP | File transfer | 20/21 |
| SSH | Secure remote access | 22 |

## Protocol Data Unit (PDU)

The PDU at the Application Layer is simply called **Data** — at this level, it hasn't been segmented, packaged, or framed yet; that happens as it moves down through the lower layers.

---

**Next:** Communication protocols