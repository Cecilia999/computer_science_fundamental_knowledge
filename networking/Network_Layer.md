# Network Layer

The transport layer provides various forms of process-to-process communication by relying on the network layer’s **host-to-host** communication service.

1. The data plane is the part of the network that actually forwards the data/packets.
2. The control plane is the part of the network that decides how to route and forward packets to a different location.

# Data Plane

## 1. IPv4

1. what is IP address

An IP address is a unique address that identifies a device on the internet or a local network. IP stands for "Internet Protocol," which is the set of rules governing the format of data sent via the internet or local network.

2. what is IPv4 address

Each IP address is 32 bits long (equivalently, 4 bytes), and there are thus a total of 2^32 (or approximately 4 billion) possible IP addresses. These addresses are typically written in dotted-decimal notation, in which each byte of the address is written in its decimal form and is separated by a period (dot) from other bytes in the address. e.g: 127.255.255.255

### 1.1 IPv4 Addressing

#### 1. classful addressing

1. What are the different classes of IPv4?

IPv4 classes are differentiated based on the number of hosts it supports on the network. The types of IPv4 classes and are based on the network/subnet portions of an IP address, which were constrained to be 8, 16, or 24 bits in length.

- class A support large size network
- class b support medium size network
- class c support small size network

![ipv4_classes2](../image/ipv4_classes1.jpg)
![ipv4_classes1](../image/ipv4_classes1.jpg)

#### 2. CIDR (pronounced cider) - Classless Interdomain Routing

The Internet’s address assignment strategy is known as **CIDR** (pronounced cider). CIDR generalizes the notion of subnet addressing.

With subnetaddressing, the 32-bit IP address is divided into two parts and again has the dotted-decimal form a.b.c.d/x, where x indicates the number of bits in the first part of the address.

- x referred to as the **prefix (or network prefix)** of the address
- The remain part refere to host

### 1.2 IPv4 Datagram Format

![ipv4_datagram_format](../image/ipv4_datagram_format.jpg)

### IPv4 Datagram Fragmentation

### what is NAT

Network Address Translation (NAT)

### IPv6

## 4.4 Generalized Forwarding and SDN

### 4.4.1 Match

### 4.4.2 Action

### 4.4.3 OpenFlow Examples of Match-plus-action in Action

# Control Plane
