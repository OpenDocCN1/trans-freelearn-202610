packet was received, the traceroute utility can build and display a progressive list of router interface IP addresses in the path and the RTT delay to each router.

# ICMP control message types

A sampling of the most commonly seen types of ICMP control messages, including their type and code (subtype) numbers, are provided in the following table:

| Type | Code | Description |
|------|------|-------------|
| 0 | 0 | This indicates echo reply (ping) |
| 3 | 0 | This indicates destination network unreachable |
| 3 | 1 | This indicates destination host unreachable |
| 3 | 4 | This indicates fragmentation required and do not fragment bit set |
| 3 | 6 | This indicates destination network unknown |
| 3 | 7 | This indicates destination host unknown |
| 5 | 0 | This indicates redirect datagram for the network |
| 5 | 1 | This indicates redirect datagram for the host |
| 8 | 0 | This indicates echo request (ping) |
| 11 | 0 | This indicates TTL expired in transit (seen in traceroutes) |

The Wireshark packet details fields for the ICMP packet illustrated in the following screenshot depict a Time-to-live exceeded message as seen in a typical traceroute capture: