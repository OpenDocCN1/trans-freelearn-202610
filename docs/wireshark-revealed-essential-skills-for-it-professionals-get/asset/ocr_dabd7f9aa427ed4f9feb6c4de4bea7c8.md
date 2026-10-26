![](bbox=[0.07, 0.05, 0.93, 0.36])

# Address Resolution Protocol

Another protocol you'll see at the network layer is Address Resolution Protocol (ARP), which is used by a device to obtain the MAC address of another device when it only knows that device's IP address.

In the following Wireshark packet details screenshot, note that the Ethernet frame destination MAC address is Broadcast (ff:ff:ff:ff:ff:ff), Type is ARP (0x0806), and the station has provided its own MAC and IP address in the ARP protocol Sender fields (which other stations listen to and use to build a table of MAC and IP addresses). It provides the IP address of the target device and puts all zeros in the Target MAC Address field. The target device should return a similar ARP packet addressed to the requestor with its MAC address in the Sender field.

A station will send an ARP request only in the following situations:

+   - The station that requires a MAC address for a target device hasn't heard a previous broadcast of that station's MAC address, or its ARP table has timed out (ARP entries are only kept for a period).

+   - The station that requires a MAC address for a target device has calculated (from the target's IP address and its own subnet mask) that the target device should be on the same LAN. Otherwise, the station assumes the target device is on a different network and sends its first session initiation packet to the default gateway (router)