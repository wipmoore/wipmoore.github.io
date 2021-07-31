---
layout: post
title: "Layer 2 Networking"
date: 2021-07-30 17:44:00 +0100
categories:
---

- [MAC Addresses](#mac-addresses)
- [Frames](#frames)
- [Collision Domains](#collision-domains)
- [Broadcast domain](#broadcast-domain)


## MAC Addresses

A Media Access Control (MAC) is an address that uniquely identifies a network node.  The manufacture hard codes a the MAC address into physical Network Interface Card.  A device can not connect to a local area segment without a MAC address.

It is six octets in length which is defined by the IEEE, the first three octets are the _Organization Unique Identifier_ and the second set of three octets are _Network Interface Controller_ specific.  The _Organization Unique Identifier_ is assigned by the IEEE

### Unicast Addresses

Used when a frame is intended for a single recipient.

### Broadcast

Used when the frame is intended for all devices on the LAN, it has a values of FFFF.FFFF.FFFF.

### Multicast

Used when the frame is intended for more than one but not all nodes on the LAN.

## Frame

- Preamble (7 bytes)
- Start Frame Delimiter (1 byte)
- Destination MAC Address (6 bytes)
- Source MAC Address (6 bytes)
- Length (2 byte)
- Type (2 bytes)
- Data  (46-1500 bytes)
- Frame Check sequence (4 bytes)

__Note__ Either the _length_ or the _type_ is present but not both.

__Note__ If jumbo frames is enabled the maximum frame size can be increased to nine thousand bytes.

## Collision Domains

A collision domain is an area of the network where two of more frames could electrically collide if more than an single frame is transmitted at the same time.

### Bridges

Are a network device that divides a LAN into multiple segments.  They are used to reduce network traffic by dividing an network into two segments, recording all the maC addresses on the network and only forwarding frames to the segment that contains the destination address.

### Switches

Are a network device that acts like a multi-port bridge.

## Broadcast domain

Is the collection of nodes that receive the same broadcast messages.  They span bridges and switches but __not__ _routers_
