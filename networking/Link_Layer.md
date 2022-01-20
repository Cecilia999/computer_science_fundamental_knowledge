# Link Layer

## 1. MAC address

1. what is MAC address

A **MAC address (Media Access Control)** is a pysical address of a device, is a globally unique identification number used to identify individual devices on the network. MAC address is **12 byte** long in **hexadecimal notation**.

Just like each house has it's own postal address, every device connected on a network has a Media Access Control (MAC) address, that uniquely identifies it.

Packets that are sent on the ethernet are always coming from a MAC address and sent to another MAC address. If a network adapter is receiving a packet, it is comparing the packet’s destination MAC address to the adapter’s own MAC address.

2. When is this MAC address used?: **ff:ff:ff:ff:ff:ff**

When a device sends a packet to the broadcast MAC address (FF:FF:FF:FF:FF:FF​), it is delivered to all stations on the local network (all devices connected to the switch). Ethernet broadcasts are used to resolve IP addresses to MAC addresses (by ARP) at the datalink layer .

3. where does MAC address come from?

A MAC address is given to a network adapter when it is manufactured. It is hardwired or hard-coded onto your computer’s **network interface card (NIC)** and it globally unique. MAC 地址是网卡决定的，是固定的。

4. `ifconfig` command can check the MAC address of your laptop

## 2. ARP - Address Resolution Protocol

1. what is ARP

   ARP is a network-level protocol used to convert the logical address i.e. IP address to the device's physical address i.e. MAC address. It can also be used to get the MAC address of devices when they are trying to communicate over the local network.

   IP address is used to locate a device on the internet vs MAC address is what identify the actual device.

2. Each host and router has a **ARP table** in memory
   - IP address + MAC address + TTL
3. `arp -a`

4. https://www.youtube.com/watch?v=cn8Zxh9bPio

## 3. Ethernet

1. what is Ethernet

   - Ethernet simply refers to the most common type of wired **Local Area Network (LAN)** used today.
   - A LAN—in contrast to a WAN (Wide Area Network), which spans a larger geographical area—is a connected network of computers in a small area, like your office, college campus, or even home.

2. Ethernet Frame structure
   - Data field carryies IP datagram
   - Src and Dest address are 6 byte MAC address

## 4. Link-Layer Switch

A switch is a device in a computer network that connects other devices together.

1. **forwarding and filtering**

   - **Filtering** is the switch function that determines whether a frame should be forwarded to some interface or should just be dropped.
   - **Forwarding** is the switch function that determines the interfaces to which a frame should be directed, and then moves the frame to those interfaces.
   - Switch filtering and forwarding are done with a **switch table**.

2. **self-learning**

   its switch table is built automatically, dynamically, and autonomously by self-learning —- without any intervention from a network administrator or from a configuration protocol.

3. Switches are **plug-and-play** devices because they require no intervention from a network administrator or user.

4. Switches are also **full-duplex**, meaning any switch interface can send and receive at the same time.

## 5. Switches Versus Routers

Router and Switch are both network connecting devices. Router works at network layer and is responsibe to find the shortest path for a packet whereas Switch connects various devices in a network. Router connects devices across multiple networks.

## 6. VLAN - Virtual Local Area Network
