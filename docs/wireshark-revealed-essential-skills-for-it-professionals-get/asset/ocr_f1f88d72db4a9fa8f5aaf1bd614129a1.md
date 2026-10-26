# A UDP header

The size of a usual UDP header is 8 bytes; the data that is added with the header can be theoretically 65,535 (practically 65,507) bytes long. A UDP header is quite small when compared to a TCP header; it has just four common fields: Source Port, Destination Port, Packet Length, and Checksum. Refer to the UDP header shown here:

| Source Port | Destination Port |
| Packet Length | Checksum |
| Application Data | |

8 bytes

+   - **Source port**: This is the port number used by the sending side to receive any replies if needed. Most of the time, in a TCP and UDP, the port number chosen to be the part of the socket is ephemeral. On the other side of the communication, the port number comes in the category of well-known port numbers.
+   - **Destination port**: This field of the header identifies the port number used by the server or receiving side, and all data will be transmitted to this port. This port number is assigned to a particular service by IANA, and definitely, it is permanently assigned to the same service specifically. For example, port 53 is for DNS and cannot be assigned to any other service (not advisable).
+   - **Packet length**: This field specifies the length of the packet, starting from the header to the end of the data; the minimum length you will observe will be 8 bytes every time, that is, the length of the UDP header.
+   - **Checksum**: As discussed earlier, checksum is performed over data, that is, the packet of the packet to ensure data integrity that is what is sent from the sender side is the same what receiver got and to verify this there are couple of checksum algorithms which comes to the rescue. Sometimes, while working with a UDP, you will see that the checksum value is 0 in the packet we received. This means that the checksum is not required to be validated.