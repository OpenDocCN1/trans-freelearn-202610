# Summary

Domain name system/Service is a protocol used to resolve website names to an IP address. Using this domain name service, your machine can communicate on an IP-based network. Using zone transfer (if enabled), unauthenticated malicious users can ask for zone data form name servers, which is considered highly malicious and dangerous.

File transfer protocol has been used to transfer files from one machine to another since the Internet came into existence and is still being used in today's modern networks. The most unsecure part about FTP is that the data is passed in plain text and can be easily captured using protocol analyzers, unless you are using some encrypted form of the FTP client-server infrastructure.

The web browsers are used to present and transfer the web-based content back and forth uses hypertext transfer protocol. It is commonly also referred to as the request/response model, where a host requests for a certain resource and the server responds with a status code and the resource if available. Status codes greater than 399 should be watched closely, I would suggest is to apply different colorization schemes.

SMTP protocol is used to send e-mails. It is an unencrypted protocol where commonly authentication mechanism is not used. Every SMTP command and its corresponding arguments are passed over the wire in plain text that can be easily sniffed using Wireshark.

VoIP traffic is made up of two things: RTP for data transfer and SIP protocol used to create the session. Signaling protocol creates and manages a session where real-time transport protocol is used to carry the voice itself. Using Wireshark, anyone can capture and reassemble the packets back to listen to a communication session. One should take care of congestion, jitter, lag, and echoing problems while dealing with these protocols in order to maintain the quality of service.