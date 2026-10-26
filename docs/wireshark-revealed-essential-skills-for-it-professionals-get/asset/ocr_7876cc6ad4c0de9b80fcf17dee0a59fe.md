+   - **Flow label**: These are used to identify sequences of packets that are labeled as a set. An IPv6 flow is defined by the 20-bit **Flow Label** field and the source and destination IPv6 address fields.
+   - **Payload length**: This is the length of the IPv6 payload, not including any packet padding.
+   - **Next header**: This field indicates what's coming next in the packet. This is equivalent to the IPv4 **Protocol** field. In the preceding example, the next layer is a normal **TCP (6)** header.
+   - **Hop limit**: This field is roughly equivalent to the **Time To Live** field in IPv4; it is decremented by one by each device that forwards the IPv6 packet. When the value reaches one, the packet cannot be forwarded.
+   - **Source and Destination addresses**: These are the 128-bit IPv6 source and destination addresses.

IPv6 supports extension headers that provide additional information fields and that also extend the length of the IPv6 header. There is specific **Next Header** code that indicates the presence of this added functionality.

# ## IPv6 transition methods

As part of the transition to **IPv6**, the current TCP/IP devices support dual stacks (IPv4 and IPv6 simultaneously) and the ability to encapsulate and tunnel IPv6 packets inside IPv4 packets so that they can be routed by IPv4 networks. The three of the most popular encapsulation methods are:

+   - **6to4 tunneling**: In this tunneling method, an IPv6 header follows an IPv4 header; the **Protocol** field of the IPv4 header will contain 41 (IPv6), and the source IPv6 address in the IPv6 header will start with `2002`.
+   - **Teredo**: In this tunneling method, an IPv6 header is encapsulated inside a UDP packet. This method was developed to accommodate NAT devices that do not handle protocol 41. Teredo tunneling can be identified in the UDP packet header by a destination port of `3544`.
+   - **ISATAP**: This tunneling method uses a locally assigned IPv4 address to create a 64-bit interface identifier. For example, in ISATAP, the IPv4 address `24.6.173.220` becomes `::0:5EFE:1806:ADDC`. ISATAP encapsulates IPv6 headers within IPv4 as in 6to4 tunneling.

# ## Wireshark IPv6 filters

**Capture filter(s):** `ip6 host fe80::1 ip proto 41` (capture IPv6-over-IPv4 tunneled traffic)