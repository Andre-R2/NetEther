# How data travels: packets and transmission
 
## Overview
 
Data travels across the network split into small fragments called packets. Depending on the network's infrastructure, these packets can be transmitted as radio waves (Wi-Fi), electrical signals (copper cables), or pulses of light (fiber optics).
 
Each packet carries control information, such as source and destination addresses, along with other data that allows it to be identified and delivered correctly.
 
## From your device to the network
 
In the case of a wireless connection, your device converts the information into radio waves that travel to an access point or a cell tower. From there, packets can continue their journey through different transmission media, such as copper cables or fiber optics, and even cross the submarine cables that connect continents and data centers around the world.
 
## The role of routers
 
Throughout this journey, routers play a fundamental role. They're responsible for forwarding packets between different networks, selecting the best available path based on their routing table and the metrics of the protocol in use.
 
## Putting it back together
 
Finally, when all the packets arrive at the destination device, it organizes them, verifies that no information is missing, and reconstructs the original message or file.
 
But how do devices know what information to add to each packet, and when to add it? To answer that question, we need to understand the OSI model and the encapsulation process.
 
---
 
**Next:** [The OSI model](the-osi-model.md)