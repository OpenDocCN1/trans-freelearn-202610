282 -895706969.756684000 172.16.136.1 172.16.136.129 TCP 64 52138-80 [SYN] Seq=0 Win=65535 Len=0
283 -1439969339.488273000 172.16.136.129 172.16.136.1 TCP 60 80-52138 [SYN, ACK] Seq=0 Ack=1 Win=
284 15.671376000 172.16.136.1 172.16.136.129 TCP 52 52138-80 [ACK] Seq=1 Ack=1 Win=13174
285 15.672063000 172.16.136.1 172.16.136.129 HTTP 375 GET / HTTP/1.1
286 1228372207.391617000 172.16.136.129 172.16.136.1 TCP 52 80-52138 [ACK] Seq=1 Ack=324 Win=307
287 15.672711000 172.16.136.129 172.16.136.1 HTTP 503 HTTP/1.1 200 OK (text/html)
288 15.672725000 172.16.136.1 172.16.136.129 TCP 52 52138-80 [ACK] Seq=324 Ack=452 Win=1
289 -895706969.777480000 172.16.136.1 172.16.136.129 TCP 64 52139-80 [SYN] Seq=0 Win=65535 Len=0
290 15.747286000 172.16.136.129 172.16.136.1 TCP 60 80-52139 [SYN, ACK] Seq=0 Ack=1 Win=
291 714245694.355758000 172.16.136.1 172.16.136.129 TCP 52 52139-80 [ACK] Seq=1 Ack=1 Win=13174
292 378319958.968279000 172.16.136.1 172.16.136.129 HTTP 359 GET /favicon.ico HTTP/1.1
293 1580695018.460033000 172.16.136.129 172.16.136.1 TCP 52 80-52139 [ACK] Seq=1 Ack=308 Win=307
294 -459410977.038322000 172.16.136.129 172.16.136.1 HTTP 556 HTTP/1.1 404 Not Found (text/html)
295 15.754902000 172.16.136.1 172.16.136.129 TCP 52 52139-80 [ACK] Seq=308 Ack=505 Win=1
299 20.679013000 172.16.136.129 172.16.136.1 TCP 52 80-52138 [FIN, ACK] Seq=452 Ack=324
300 609634608.344347000 172.16.136.1 172.16.136.129 TCP 52 52138-80 [ACK] Seq=324 Ack=453 Win=1
301 20.761722000 172.16.136.129 172.16.136.1 TCP 52 80-52139 [FIN, ACK] Seq=505 Ack=308
302 -1931345972.395708000 172.16.136.1 172.16.136.129 TCP 52 52139-80 [ACK] Seq=308 Ack=506 Win=1

Figure 5.1: Connection Process:Three-way handshake, data transfer and tear down process

In the packets 282, 283, and 284, it is clearly visible that the client and server are trying to create a dedicated channel. The client initiated the creation by sending a SYN packet in the 282 packet with the SEQ set to 0. Since the server was open for communication, the server responded with a SYN/ACK packet with ACK set to 1 and SEQ set to 0. This is followed by a confirmation sent from the client side that is seen in the packet number 284 with SEQ=1 and ACK=1. This is what a three-way handshake process looks like. This can be seen before any real data transfer that happens that follows the TCP approach.

After the successful completion of channel creation, the client sends a GET request to access the contents of the web-root directory. The server acknowledged this in the packet number 287 and sent the requested content to the client's machine with the 200 OK status message, which is acknowledged by the client in the next packet. As seen in the list pane again, the client was requesting a new resource, which the server wasn't able to find and thus sent a 404 Not Found status message, which was acknowledged by the client in the the packet 295.

After all the data transfer takes place, when the client has nothing left to request, or when the server has nothing left to send, the client sends FIN/ACK packets to properly terminate the connection. The server acknowledges this and sends its own FIN/ACK packets, which are acknowledged by the client as well in the packet number 302. This way of termination is often referred to as the teardown process. Take a look at the