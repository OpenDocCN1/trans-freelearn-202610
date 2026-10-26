
## How it works...

Here are some explanations to the filters we saw in the How to do it... section of this recipe.

### Ethernet broadcasts

In Ethernet, broadcasts are packets that are sent to addresses with all 1s in the destination field and this is why, to find all broadcasts in the network, we insert the filter `eth.dst == ffff.ffff.ffff`.

### IPv4 multicasts

IPv4 multicasts are packets that are sent to an address in the address range 224.0.0.0 to 239.255.255.255 that is in binary of the address range 11100000.00000000.00000000.00000000 to 11101111.11111111.11111111.11111111.

If you look at the binary representation, a destination multicast address is an address that starts with three 1s and a 0, and therefore, a filter to IPv4 multicast destinations will be `ip.dst == 224.0.0.0/4`. That is, an address that starts with four 1s (224), and a subnet mask of 4 bits (`/4`) will indicate a network address range from 224 to 239 that will filter multicast addresses.

### IPv6 multicasts

IPv6 multicasts are packets that are sent to an address that starts with ff (first two hex digits = ff), then one-digit flags, and scope. Therefore when we write the filter `ipv6.dst == ff00::/8`, it means to display all the packets in IPv6 that are sent to an address that starts with the string ff, that is, IPv6 multicasts.

## See also

- For more information on Ethernet, refer to [Chapter 7](#), Ethernet, LAN Switching, and Wireless LAN

## Configuring TCP/UDP filters

TCP and UDP are the main protocols in layer 4 that provide connectivity between end applications. Whenever you start an application from one side to another, you start the session from a source port, usually a random number equal or higher than 1,024, and connect to a destination port, which is a well-known or registered port that waits for the session on the other side. These are the port numbers that identify the application that works over the session.

Other types of filters refer to other fields in the UDP and TCP headers. In UDP we have a very simple header with very basic data, while in TCP we have a more complex header that we can get much more information from.

In this recipe we will concentrate on the possibilities while configuring TCP and UDP display filters.

### Getting ready

As done earlier, we should plan precisely what we want to display and prepare the filters accordingly.

For TCP or UDP port numbers use the following display filters:

- `tcp.port == <value>` or `udp.port == <value>`: This is used for specific TCP or UDP ports (source or destination)
- `tcp.dstport == <value>` or `udp.dstport == <value>`: This is used for specific TCP or UDP destination ports
- `tcp.srcport == <value>` or `udp.srcport == <value>`: This is used for specific TCP or UDP source ports

In UDP, the header structure is very simple: source and destination ports, packet length, and checksum. Therefore, the only significant information here is the port number.

TCP on the other hand is more complex and uses connectivity and reliability mechanisms that can be monitored by Wireshark. Using `tcp.flags`, `tcp.analysis`, and other smart filters will help you resolve performance problems (retransmissions, duplicate ACKs, zero windows, and so on), protocol operation issues such as resets, half-opens, and so on.

Common display filters in this category are as follows:

- `tcp.analysis`: This is used for TCP analysis criteria such as retransmission, duplicate ACKs, or window issues. Examples for this filter are as follows (you can use the autocomplete feature to get the full list of available filters):
  - `tcp.analysis.retransmission`: This is used to display packets that were retransmitted.
  - `tcp.analysis.duplicate_ack`: This is used to display packets that were acknowledged several times.
  - `tcp.analysis.zero_window`: This is used to display packets when a device on the connection end sends a zero-window message (that tells the sender to stop sending data on this connection, until window size increases again).

**Tip:**

The `tcp.analysis` filters do not analyze the TCP header; they provide a protocol analysis through the Wireshark expert system.

- `tcp.flags`: These are used to find out if a flag(s) is set or not. Examples of this filter are as follows:
  - `tcp.flags.syn == 1`: This is used to check if the SYN flag is set.
  - `tcp.flags.reset == 1`: This is used to check if the RST flag is set.
  - `tcp.flags.fin == 1`: This is used to check if the FIN flag is set.

**Tip:**

For TCP flags, the `tcp.flags` filter will be used to find out whether a specific flag is set or not.

- `tcp.window_size_value < <value>`: This is used to look for small TCP window sizes that are in some cases indications for slow devices.

### How to do it...

Some examples for filters in TCP/UDP filters:

- To filter all the packets to the HTTP server, use `tcp.dstport == 80`
- To filter all the packets from the network 10.0.0.0/24 to the HTTP server, use `ip.src==10.0.0.0/24 and tcp.dstport == 80`
- For all the retransmissions in a specific TCP connection, use `tcp.stream eq 16 && tcp.analysis.retransmission`

To isolate a specific connection, place the mouse on a packet in the connection you want to watch, right-click on it, and choose **Follow TCP Stream**. A TCP stream is the data that is transferred between the two ends of the connection from the connection establishment to the connection tear down. The string `tcp.stream eq <value>` will appear in the display filter window. This is the stream you can work on now. In the preceding example, it came out as stream 16, but it can be any stream number (starting the count from stream 1 in the capture file).

Retransmissions are TCP packets that are sent again. It can be due to several reasons, as explained in Chapter 9, UDP/TCP Analysis.

**Tip:**

While monitoring phenomena such as retransmissions, duplicate ACKs, and others that influence performance, it is important to remember that these phenomena refer to a specific TCP connection.

Other examples of the types of TCP filters are as follows:

- To filter all the window problems in a specific connection:
  - `tcp.stream eq 0 && (tcp.analysis.window_full || tcp.analysis.zero_window)`
  - `tcp.stream eq 0 and (tcp.analysis.window_full or tcp.analysis.zero_window)`
- To filter all the packets from 10.0.0.5 to the DNS server: `ip.src == 10.0.0.5 && udp.port == 53`
- To filter all the packets or protocols in TCP (for example HTTP) that contains the string cacti (case sensitive): `tcp contains "cacti"`
- To check all the packets that are retransmitted from 10.0.0.3: `ip.src == 10.0.0.3 and tcp.analysis.retransmission`
- To filter all the packets to any HTTP server: `tcp.dstport == 80`
- To check all the connections opened from a specific host (if in a form of scan, can be a worm!): `ip.src==10.0.0.5 && tcp.flags.syn==1 && tcp.flags.ack==0`
- To check all the cookies sent from and to a client: `ip.src==10.0.0.3 && (http.cookie || http.set_cookie)`

### How it works...

The following are illustrations of the IP and TCP header structures respectively. UDP is quite simple; it has only source and destination port numbers, length, and checksum. In the following diagram we see the IP header structure:

![](img/da40f92928403866599198344271d63e_415_0.png)

Some important factors in the IP packet are as follows:

- **Ver:** The version is either 4 or 6.
- **Header length (HL):** The header length is 20 to 24 bytes, with options.
- **Type of Service (ToS):** This is usually implemented with **Differentiated Services (DiffServ)** and provides priority to preferred services.

**Tip:**

IP standard (RFC 791 from September 1981) has named this field Type of Service (ToS) and defined its structure. The standards for Differentiated Services were published later (RFCs 2474, 2475 from December 1998 and others) and are used for the implementation of the ToS byte in majority of the applications.

- **Length:** This field indicates the total datagram length in bytes.
- **16-bit identifier, flags, and Fragment offset:** Every packet has its own packet ID. When fragmented along with the flags and offset, these will enable the receiver to reassemble it.
- **Time to live (TTL):** This starts with 64, 128, or 256 (depending on the operating system that sends the packet), when every router on the way decrements the value by one. This comes to prevent packets from traveling endlessly through the network. The router that sees 1 in the packet decrements it to 0 and drops the packet.
- **Upper layer:** This field consists of upper-layer protocols such as TCP, UDP and ICMP.
- **Checksum:** This field represents the packet checksum. The idea here is that the sender uses an error-checking mechanism to calculate a value over the packet. This value is set in the checksum field while the receiver of the packet calculates it again. If the sent value is not equal to the received value, it will be considered as a checksum error.
- **32-bit source and destination IP addresses:** As the names suggest, these are source and destination IP addresses.
- **Options:** This field is usually not in use in IPv4. In the following diagram you see the TCP header:
