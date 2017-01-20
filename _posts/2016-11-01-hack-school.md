---
layout: post
title:  "Hacker's school - Partial excerpt from the explanation section"
date:   2016-11-01 12:00:00 +0900
categories: Development
---

Today, I'd like to explain some words from Hacker's school.

Let's start with basic terms.

- Private IP ... IP for allocation within the LAN
- Global address IP ... IP assigned in the Internet field
- Loop back address ... A virtual address indicating itself (normally: 127.0.0.1 localhost)
- Broadcast address ... An address obtained by assigning all host hosts belonging to the relevant network to 1 (if the host address of 192.168.1.0 is the lower 8 bits, the broadcast address is 192.168.1.255)
- CIDR ... Classless addressing, how you can set the network address to any length
- Subnet mask ... A 32-bit number that defines how many bits from the beginning of the IP address to use for the network address

For example, if the subnet mask is a binary number "11111111 11111111 11111111 00000000" (decimal number "255.255.255.0"),
the upper 24 bits become the network address and the lower 8 bits become the host address.
It is written as "198.168.10.0 / 24" together with the network address, or simply as "/ 24".
The latter notation is called CIDR notation.

## Why manage MAC addresses in network management

By checking the vendor from the OUI of the MAC address and identifying the device,
it is possible to confirm whether or not the network management unit does not use the network device unknown by the network management unit without permission.
Since it is troublesome to connect an individual's PC inside the LAN without permission,
refuse connection from other than the terminal having the MAC address registered by MAC address filtering.

## Why do I need an IP address and MAC address in TCP / IP communication?

MAC address does not have the concept of network.
When only IP address is used, no allocation of address is done at the data link layer at all,
so the OS will judge whether it is addressed to itself for all communication flowing through the physical network, which increases the load.

## Address resolution

In order to communicate with TCP / IP, it is sufficient if there is an IP address and a MAC address.
Once the domain name is known,
the IP address can be identified from the DNS,
so it is only necessary to have a mechanism to derive the MAC address from the IP address.

Address resolution should lead the other from either the IP address or the MAC address.
Generally, however, from the above circumstances, it means to derive an IP address from the MAC address.

There are three major categories of methods.

- Table lookup
- Calculate physical address from protocol address
- Message Exchange

# Communication of communication in the network field

- TCP ... Separate and exchange long data and restore it on the receiving side on the receiving side. even if an error occurs while carrying data, it can be repaired or retransmitted
To do
- UDP ... Simple protocol without recovery function of error. Therefore, the burden on the processor which processes the communication is light, and the operation speeds up because the amount of data flowing through the network is small.

## UDP client behavior

- Name resolution ... To acquire IP from server name
- Create socket object · The OS has a table for managing the communication state and one record corresponds to one socket (program name being communicated, type of protocol, communication status etc)
- Transmission of data ... Check transmission contents, route search, find MAC address using ARP, create / send packet
- Receive response data

The rough flow is the same for TCP, but its contents are different.


## NIC

NIC ... The Network Internet Card is a card for converting digital data into electric signals and sending it, and vice versa to convert electric signals into digital data and import them into a computer.
Because NIC is used for LAN connection, it is also called LAN card.

## OSI Model

[https://en.wikipedia.org/wiki/OSI_model](https://en.wikipedia.org/wiki/OSI_model)

## ICMP

ICMP ... communication protocol for communicating the state of connected devices on the network
It is used for diagnosis of IP network for checking error notification of IP packet and the state of IP network.

[ICMP Message Types](http://www.informit.com/articles/article.aspx?p=26557&seqNum=5)

## DNS namespace

DNS manages names in a tree structure.
Names are assigned to nodes, and the entire tree structure is called DNS namespace (domain name space).
The subtree of domain name space becomes domain.
The depth of the DNS namespace tree is allowed up to 127.

The domain immediately under the root is called *** TLD (Top Level Domain) ***.

## Resource record

A domain has a set of resource records (resource records) associated with domains.
Representative

| Type | value | meaning |
| ------ | ------ | ------ |
| A | 1 | Host IP address |
| NS | 2 | name server authoritative for zone (DNS server) |
| CNAME | 5 | Press the canonical name of the domain corresponding to the alias |
| SOA | 6 | origin of authority within zone |
| WKS | 11 | Well known service |
| PTR | 12 | pointer for reverse lookup of IP address |
| HINFO | 13 | Additional information on host (CPU and OS type) |
| MINFO | 14 | mailbox in charge of mailing list |
| MX | 15 | Mail server (Mail Exchange) |
| TXT | 16 | Simple ASCII string not interpreted by DNS |
| SIG | 24 | Security signature |
| KEY | 25 | Security key |
| GPOST | 27 | Geographic location |
| AAAA | 28 | Host IP address (IPv6) |
| NXT | 30 | Next Domain |
| SRV | 33 | Server Selection |
| * | 255 | Select all records |



