In the following Wireshark packet details screenshot, the Ethernet II frame **Destination** and **Source** MAC addresses, **Type** (indicating that the next layer protocol is IP), and **Frame check sequence** are circled, as is the **Frame** summary.

# Note

Wireshark displays a summary of each frame that includes frame sizes, captured timestamps and interframe times, and other useful information. This is metadata calculated by Wireshark to aid in analysis and not a part of the captured frame.

The following screenshot highlights the significant fields of an Ethernet frame:

![](bbox=[0.07, 0.305, 0.925, 0.52])

# Note

Any additional analysis provided by Wireshark in any area of the Packet Details pane that is calculated or otherwise not part of actual packet contents will be encased in brackets.

# Layer 3 – the network layer

The network layer (called the Internet layer in the DARPA model) primarily handles the routing of packets across and to other networks along the path from source computers to destination hosts based on the destination IP address. The two most common protocols seen at this layer are Internet Protocol and Address Resolution Protocol.

# Internet Protocol

The most common protocol in use at this layer is **Internet Protocol Version 4 (IPv4)**, which includes several essential fields to accomplish the task of routing packets across