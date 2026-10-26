connection that is commonly known as the handshake process.

+   - **ACK (acknowledgement):** These packets are used to confirm that the data packets have been received, and this also confirms the initiation and tear down of the connections.
+   - **RST (reset):** These packets signify that the connection you were trying to create has been shut down or may be the application we were trying to communicate with is not accepting connections.
+   - **FIN (finish):** These packets indicate that the connection is being torn down after the successful delivery of data packets. Both the sender and receiver send the FIN packets to gracefully terminate the connection. If they want to communicate again, they will start from the beginning, that is, from the three-way handshake process.
+   - **PSH (push):** These packets indicate that the incoming data should be passed on directly to the application instead of getting buffered. To state this simply, the other host should receive data without waiting for it.
+   - **URG (urgent):** Marked packets indicate that the data that the packet is carrying should be processed immediately by the TCP stack and the urgent pointer field should be examined if it is set.
+   - **CWR (congestion window reduced):** These packets are used by the sender to inform the receiver that due to the transmit, the buffer is getting overfilled, and because of congestion, both the parties should slow down the transmission process to avoid any packet loss that might happen.

+   - **Window size:** This field in the header indicates the amount of data that the sender can send, . The amount is decided during the handshake process where both the hosts that communicate match the buffer size compatible for transmission. Flow control can be achieved through this field.
+   - **Checksum:** To cross check the integrity of the data that is being received, this field is used, where the contents of the TCP segments are validated.
+   - **Urgent pointer:** This field tells us about the value that the urgent pointer contains. It specifically indicates the sequence number of the octet that lies before the data.
+   - **Options:** This field length can vary due to the presence of various options. This field has three parts: the first part specifies the length of the option field, the second part denotes the options being used, and the third actually contains the options in use. One of the important options **maximum segment size (MSS)** is also part of this field.
+   - **Data:** The last part in the TCP header is the real data that travels around.

The preceding information gives us an overview regarding TCP headers and the