---
layout: post
title: Unsung benefits of IPv6
excerpt: Discussion of the advantages of IPv6 — other than increased address space — such as mandatory encryption and extensibility.
---

For the past year or so the internet community has been attempting to migrate (or at least attempting to prepare to migrate) from [IPv4](http://en.wikipedia.org/wiki/IPv4) to [IPv6](http://en.wikipedia.org/wiki/IPv6). The main reason for the transition is to enable — out of necessity — a larger IP address space. While IPv4 allocates 32 bits for an individual address, giving \\(2^{32}=4294967296\\) unique IP addresses, IPv6 allocates 128 bits, allowing \\(2^{128}=340282366920938463463374607431768211456\\) different addresses to be uniquely referenced.

However, IPv6 affords us many other benefits aside from the increased address space. While none of these may necessitate a transition by themselves, they are still welcome additions to the protocol.

## Multicasting

IPv6 allows a packet to be transmitted to multiple recipients with a single operation, rather than having to specify each recipient individually (and use up bandwidth in the process). By sending a packet to the `ff02::1` multicast group a packet will be transmitted to all hosts on the link.

## Stateless address autoconfiguration (SLAAC)

[ICMP](http://en.wikipedia.org/wiki/Internet_Control_Message_Protocol) — the Internet Control Message Protocol — and [DHCP](http://en.wikipedia.org/wiki/Dhcp) — the Dynamic Host Configuration Protocol — are also getting v6 version bumps with the introduction of IPv6. ICMPv6 and DHCPv6 allow automatic configuration of hosts connected to an IPv6 network. ICMPv6 allows stateless configuration, meaning that the local router will automatically respond to a multicast broadcast advertising its configuration parameters. If this is unavailable (depending on environment and application) DHCPv6 can be used as a fallback in much the same way as DHCPv4.

## Mandatory network-layer security

Perhaps the most important additional enhancement, IPv6 now handles IPsec traffic as standard. IPsec, like HTTPS, enforces encryption and authentication of packets, but is protocol-agnostic. Although engineered into IPv4, IPsec will be mandatory for IPv6 connections and will hugely increase the security of all internet use.

## Simplified processing by routers

Multiple enhancements have been made to simplify the process of packet forwarding and make it more efficient.

* Due to the additional features of IPv6, the packet headers are on the order of twice as large as IPv4 headers. However, the layout has been simplified with fields that are rarely used being moved to optional header extensions (see "Options extensibility" below). For example, the [Selective Directed Broadcast Mode](http://tools.ietf.org/html/rfc1770) option, providing unreliable UDP delivery to a set of IP addresses — which can range in size from 6 to 38 bytes — is rarely used and can be safely moved to the optional header extensions section of the packet.
* There is no fragmentation of packets between routers over an IPv6 network. To ensure the correct MTU is set, hosts can perform end-to-end MTU discovery, end-to-end packet fragmentation, or use the default minimum of 1280 bytes. This eases the load on the hosts to reassemble fragmented packets, but should also mean that numerous VPN solutions work much more reliably.
* The somewhat-redundant Internet-layer checksum has been removed. Since both the link layer (e.g. Ethernet) and transport layer (e.g. TCP) typically perform checksum calculations, it is very unlikely that such a checksum would detect an error not already caught at a higher or lower layer. This also eases the burden on intermediate IPv6 routers, as when the packet is altered due to the "time to live" (TTL) being decremented, they do not have to recalculate the checksum.
* The TTL field has been renamed to "Hop Limit", an all round more intuitive term for what it represents.

## Mobility

Mobile IPv6, i.e. IPv6 on mobile devices, is the IPv6 implementation that allows users to move from one network to another while maintaining a permanent (or at least sticky) IP address. (Otherwise, when dynamically switching from a 2G to a 3G network on a mobile device, any partially-downloaded files would become unusable, and voice / video calls would likely glitch as the transition is made.) Mobile IPv6 avoids the problem of [triangular routing](http://en.wikipedia.org/wiki/Triangular_routing) that plagues mobile IPv4.

## Options extensibility

As noted above, optional header options are implemented as additional extensions separate from the main IPv6 header, which has a fixed size of 40 bytes. This allows the protocol to be completely extensible in the future, allowing quality of service, security and mobility features to be added without redesigning the base protocol.

## Jumbograms

The most fun-sounding addition has been saved for last. IPv4 packets have a fixed maximum size of 65535 bytes. By specifying the "Jumbo Payload Option" header, IPv6 nodes can handle packets larger than this limit, known as jumbograms. This can increase performance over high-MTU links, but is also fun to say.