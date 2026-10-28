# The gateway pattern

There are cases when services may be partly stored on one part of an organization's LAN, and other services are stored on another LAN, and the only transport to each other is an HTTP or HTTPS tunnel to pass messages to NSB.

The main purpose of the gateway is to allow you to do the same durable fire-and-forget messaging that you are accustomed to with NServiceBus across physically separated sites, where sites are locations in which you run IT infrastructure and not websites.

The gateway only comes into play where you can't use normal LAN-to-LAN VPN tunnels or use internal LAN servers to communicate MSMQ to MSMQ. The purpose of the gateway is to create messages that communicate through HTTP, but it would be preferable to use HTTPS to ensure that messages are secured. The following architecture diagram represents a gateway built using the NSB:

![](bbox=[0.18, 0.4, 0.82, 0.72])