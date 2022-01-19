# Link Layer

## 面试题

1. what is MAC address?

A **MAC address (Media Access Control)** is a pysical address of a device, is a globally unique identification number used to identify individual devices on the network.

Just like each house has it's own postal address, every device connected on a network has a Media Access Control (MAC) address, that uniquely identifies it.

Packets that are sent on the ethernet are always coming from a MAC address and sent to another MAC address. If a network adapter is receiving a packet, it is comparing the packet’s destination MAC address to the adapter’s own MAC address.

2. When is this MAC address used?: ff:ff:ff:ff:ff:ff

When a device sends a packet to the broadcast MAC address (FF:FF:FF:FF:FF:FF​), it is delivered to all stations on the local network (all devices connected to the switch). Ethernet broadcasts are used to resolve IP addresses to MAC addresses (by ARP) at the datalink layer .

3. where does MAC address come from?

A MAC address is given to a network adapter when it is manufactured. It is hardwired or hard-coded onto your computer’s **network interface card (NIC)** and it globally unique. MAC 地址是网卡决定的，是固定的。

`ifconfig` command can check the MAC address of your laptop

4. [what is **ARP - Address Resolution Protocol**](https://www.youtube.com/watch?v=cn8Zxh9bPio)

`arp -a`

ARP is a network-level protocol used to convert the logical address i.e. IP address to the device's physical address i.e. MAC address. It can also be used to get the MAC address of devices when they are trying to communicate over the local network.

IP address is used to locate a device on the internet vs MAC address is what identify the actual device.
