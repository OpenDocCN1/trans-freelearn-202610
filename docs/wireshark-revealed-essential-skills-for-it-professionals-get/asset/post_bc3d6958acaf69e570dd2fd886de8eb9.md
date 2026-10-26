
workstation as possible. This has the advantages of offering the ability to perform the following actions:

- View and verify the actual problem that the user is reporting
- Measure round-trip times to the target server(s)
- Capture and view the TCP handshake process upon session initiation
- Capture and investigate the login and any other background processes and traffic
- Look for indications of network problems (lost packets and retransmissions) as they are experienced by the user's device
- Measure the apparent network throughput to the user's workstation during data downloads
- Eliminate the need to use a capture filter; the amount of traffic to/from a single workstation should not be excessive

A capture at a user workstation, server, or other device should be conducted with the use of an aggregating **Test Access Point (TAP)** versus using a switch SPAN port (as discussed in [Chapter 3](Chapter 3), Capturing All the Right Packets), or as a last resort by installing Wireshark on the user's workstation or server (if authorized).

## Troubleshooting connectivity issues

Single user or small group connectivity issues can be resolved by confirming that the networking functions required for a user workstation to access local and remote network resources are functioning properly. The basic requirements or items to confirm include:

- Enabling the correct network interface(s) (workstation configuration)
- Confirming layer 1 (physical) connectivity
- Obtaining an IP address, subnet mask, and default gateway for each interface (DHCP)
- Obtaining the MAC address of the default gateway or other local network services (ARP)
- Obtaining the IP address of a network service (DNS)
- Connecting to a network service (TCP handshake or UDP response)

We'll briefly discuss each of these in order; while the first two steps will not involve using Wireshark, they are a necessary part in a troubleshooting approach. If the connectivity issue is affecting a group of users or a whole office, the first step is probably not applicable.

### Enabling network interfaces

While it may seem obvious that network interfaces need to be enabled, the assumption that they are automatically enabled (especially for the wireless connectivity) by default upon device boot up may be false.

On Windows, you can use the command-line utility `ipconfig` to view the status and basic configuration (IP address, subnet mask, and default gateway) of network interfaces; on Linux or Mac devices, the equivalent command is `ifconfig` or `ip`.

### Confirming physical connectivity

If a connectivity problem is isolated to a single user's workstation, the physical connections are suspected. There are a few items to check, and the troubleshooting steps that can be taken are as follows:

- If there is a problem with the Ethernet cable from the workstation to a wall jack, you need to swap the cable with a different one.
- If there is a problem with the cabling from the user's wall jack to the switch port, you need to temporarily plug the user's Ethernet cable into another (known good) wall jack.
- If there is a problem with the switch, switch port, or port configuration, you need to temporarily plug the user's port cable into another (known good) port. Be aware that some network security policies call to disable switch ports until they are needed or configuring the port to be associated with a single, specific MAC address. If so, a port may not work when you plug into it although there is nothing physically wrong with it.

### Obtaining the workstation IP configuration

Unless the workstation was manually configured, it will need to get its IP address, subnet mask, default gateway, and DNS server settings from a DHCP server. If this does not appear to be working properly (after checking the configuration using `ipconfig` (Windows) or `ifconfig` (Linux or Mac OS X)), you need to perform a packet capture during the workstation initialization/boot-up process using a TAP or SPAN port and investigate the DHCP requests and responses.

There are eight DHCP message types (not to be confused with the two Bootstrap Protocol types, Boot Request and Boot Reply):

| Message type number | Message type | Description |
|---|---|---|
| 1 | DHCP Discover | A client broadcast to locate an available DHCP server |
| 2 | DHCP Offer | A server to client response to a DHCP Discover to offer configuration parameters |
| 3 | DHCP Request | A client message to a DHCP server to either one of the following conditions:<br>- Request offered parameters from one server and decline offers from other DHCP servers<br>- Confirm correctness of previously allocated address after a reboot<br>- Extending the lease on an IP address |
| 4 | DHCP Decline | Client message to DHCP server indicating the offered address is not acceptable |
| 5 | DHCP Acknowledgment | Server to client with configuration parameters including a committed network address |
| 6 | DHCP Negative Acknowledgement | Server to client indicating client's address is incorrect or expired |
| 7 | DHCP Release | Client to server releasing a network address and canceling a lease |
| 8 | DHCP Informational | Client to server asking for local configuration parameters only |

For a workstation that is booting up and was previously working on the network, you'll generally see the DHCP Request and Acknowledgment packets verifying that the workstation can still use a previously leased address. On an entirely cold start up, the first two DHCP packets will be DHCP Discover and DHCP Offer packets, followed by the Request and ACK packets.

In a DHCPv6 environment, the typical packet sequence is DHCPv6 Solicit, DHCPv6 Advertise, DHCPv6 Request, and DHCPv6 Reply.

The fields to verify in a DHCP Response packet (or similar fields in a DHCPv6 Advertise packet) include the following four fields:

- **Your (client) IP Address:** This is the offered IP address for this workstation
- **Subnet Mask:** This is the subnet mask to use on this network
- **Domain Name Server:** This is the DNS server IP address
- **Router:** This is the IP address of the default gateway to use

This is the minimum data required for any network communications; an example of these fields being provided in a DHCP Reply packet is illustrated in the following screenshot:

```
Bootstrap Protocol (ACK)

Message type: Boot Reply (2)
Hardware type: Ethernet (0x01)
Hardware address length: 6
Hops: 0
Transaction ID: 0x28a0655c

Seconds elapsed:
[Expert Info (Note/Protocol): Seconds elapsed appears to be encoded as little-endian]
[Severity level: Note]
[Group: Protocol]

Bootp flags: 0x8000 (Broadcast)
1... .... .... .... = Broadcast flag: Broadcast

Client IP address: 0.0.0.0 (0.0.0.0)
Your (client) IP address: 192.168.1.115 (192.168.1.115)

Next server IP address: 192.168.1.1 (192.168.1.1)
Relay agent IP address: 0.0.0.0 (0.0.0.0)
Client MAC address: 00:1c:25:99:d8:85 (00:1c:25:99:d8:85)
Client hardware address padding: 00000000000000000000
Server host name: ecosystem.home.cisco.com
Boot file name not given
Magic cookie: DHCP

Option: (53) DHCP Message Type (ACK)
Length: 1
DHCP: ACK (5)

Option: (54) DHCP Server Identifier
Length: 4
DHCP Server Identifier: 192.168.1.1 (192.168.1.1)

Option: (51) IP Address Lease Time
Length: 4
IP Address Lease Time: (86400s) 1 day

Option: (58) Renewal Time Value
Length: 4
Renewal Time Value: (43200s) 12 hours

Option: (59) Rebinding Time Value
Length: 4
Rebinding Time Value: (75600s) 21 hours

Option: (1) Subnet Mask
Length: 4
Subnet Mask: 255.255.255.0 (255.255.255.0)

Option: (28) Broadcast Address
Length: 4
Broadcast Address: 192.168.1.255 (192.168.1.255)

Option: (81) Client Fully Qualified Domain Name
Length: 15
Flags: 0x03
0000 .... = Reserved flags: 0x00
.... 0... = Server DDNS: Some server updates
.... .0.. = Encoding: ASCII encoding
.... ..1. = Server overrides: override
.... ...1 = Server: Server
A-RR result: 255
PTR-RR result: 255
Client name: ThinkPadW500

Option: (6) Domain Name Server
Length: 4
Domain Name Server: 192.168.1.1 (192.168.1.1)

Option: (3) Router
Length: 4
Router: 192.168.1.1 (192.168.1.1)

Option: (255) End
Option End: 255
```
