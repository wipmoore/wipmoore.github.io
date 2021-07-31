---
layout: post
title: "Layer 3 Networking"
date: 2021-07-31 08:25:00 +0100
categories:
---

IPv4 uses 32-bit addresses which limits the address space to 4294967296 addresses.  They are most commonly represented in dot notation which represents the four octets of the address expressed individually in decimal number format separated by periods.

```
e.g.  192.0.2.235, 0xC00002EB, 0xC0.0x00.0x20.0xEB
```

An IP address is divided into two parts: network/host which are identified by a network mask.   The network mask is a binary bit map that defines the number of high order bits that should be set to 1 when applying an _and_ operation against the IP address.  The result from the _and_ operation tell you the hosts network address.

```
CIDR network mask notation /24 11111111.111111111.11111111.00000000
```

When a host needs to send a packet to another machine it will use the destination address and its network mask to decided where to send it.  It identifies whether the intended recipient is on the same same network using the destination address and its network address.

If the destination is on the same network:

- Look up the MAC address in the ARP cache
  - If there is not an entry from the IP address:
    - ARP request is broadcast ( Layer 2 ) for the IP address
    - ARP cache is populated with the entry
- Send a Layer 2 frame that encapsulates the IP Packet to the MAC address.

If the destination is not on the same network:

- Look up in the routing table what device handles requests for this network:
- Look up the MAC address for the device in the ARP cache
- Send a Layer 2 frame that encapsulates the IP Packet to the MAC address.
  - __Note__ the destination IP address of the encapsulated packet is the intended recipient not the IP address of the gateway, the layer 2 frame is addressed to the gateway.


__Note__ This is only a brief overview of IP networking just enough to understand kubernetes networking rather than to gain an understanding of the different internet routing approaches.