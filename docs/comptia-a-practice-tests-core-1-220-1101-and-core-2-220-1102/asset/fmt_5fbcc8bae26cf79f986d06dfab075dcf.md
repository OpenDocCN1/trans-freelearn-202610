
# 网络配置概念练习题

## 2.5 IP 寻址与子网划分

### 问题 3
It does not have a network ID

It does not have a host ID

### 问题 3
A systems administrator is configuring an IP address on the external port of the firewall that connects to the WAN. Which of the following IP addresses will they use?

1.  `131.107.1.1`
2.  `192.168.1.1`
3.  `169.254.1.223`
4.  `172.16.5.13`
5.  `10.10.15.6`

### 问题 4
Which of the following is the default subnet mask for a Class B IPv4 address?

1.  `255.0.0.0`
2.  `255.255.0.0`
3.  `255.255.255.0`
4.  `169.254.1.1`

### 问题 5
If you work from home and need to connect to the internet, which of the following devices are you most likely to use?

1.  Hub
2.  Router
3.  Switch
4.  WAP

### 问题 6
Which of the following is a valid IPv4 address?

1.  `10.1.1.0`
2.  `255.255.255.128`
3.  `212.15.1.2`
4.  `12.1.1.255`

### 问题 7
A Windows server has been configured with both IPv4 and IPv6 addresses. What is this known as?

1.  Double stack
2.  Double IP addressing
3.  Dual IP addressing
4.  Dual stack

### 问题 8
Which of the following is the reason why a desktop might be allocated an IP address of `169.254.11.2`? (Select all that apply)

1.  The desktop might have a bad cable or cannot send its broadcast to the `DHCP` server
2.  The desktop might not be able to contact a `DNS` server
3.  The desktop might not be able to connect to a domain controller
4.  The desktop might have been allocated a duplicate IP address

### 问题 9
A user is unable to access the internet, and when the system administrator runs the `ipconfig/all` command, they get the following output:

```
IPv4 Address. . . . . . . . . . . : 132.24.0.1
Subnet Mask . . . . . . . . . . . : 255.255.0.0
Default Gateway . . . . . . . . . :
```

Which of the following is the source of this problem?

1.  `DNS`
2.  Gateway
3.  Private IPv4 address
4.  Dynamic IP address

### 问题 1
How many bits does an IPv4 address use?

1.  32
2.  8
3.  16
4.  64

### 问题 2
Which of the following is an IPv6 address?

1.  `10.1.1.1`
2.  `B4-2E-99-C0-75-45`
3.  `169.254.1.5`
4.  `2001:0dC8:0000:0000:0abc:0000:def0:2311`

### 问题 3
An installation engineer has installed a new router in a `SOHO` network that consists of five users. During testing, they noticed that all devices have `APIPA`s. Printers and file shares are still working, but the hosts cannot access the internet. Which of the following has the installation engineer MOST LIKELY forgotten to configure on the router?

1.  `NETBIOS`
2.  `SMTP`
3.  `DNS`
4.  `SMB`

### 问题 4
How can you shorten the following IPv6 address: `2001:0dC8:0000:0000:0abc:0000:def0:2311`?

1.  `2001:0dC8::0abc::def0:2311`
2.  `2001:0dC8::0000:0abc:0000:def0:2311`
3.  `2001:dC8::abc::def:2311`
4.  `2001:dC8::abc:0:def0:2311`

### 问题 5
Which of the following is a public IPv4 address?

1.  `10.10.10.1`
2.  `172.16.1.3`
3.  `126.1.1.1`
4.  `192.168.5.6`

### 问题 6
How many bits does an IPv6 address use?

1.  16
2.  32
3.  64
4.  128

## 2.6 Compare and contrast common network configuration concepts

### 问题 1
The financial director of an enterprise raised a ticket as they were unable to connect to any network resources. What should the system administrator check first?

1.  `DHCP` server
2.  `SNMP`
3.  `RADIUS` server
4.  `DNS` server

### 问题 2
What `DNS` record needs to be allocated to a host with an IP address of `2001:0436:2CA1:0057:0023:0567:5673:0005`?

1.  `A`
2.  `AAAA`
3.  `MX`
4.  `TXT`

### 问题 3
What is the default lease duration of a Microsoft `DHCP` server?

1.  12 days
2.  14 days
3.  8 days
4.  2 days

### 问题 4
Users in a small company cannot access the mail server on Monday morning. The system administrator had migrated the old mail server to a new version but did not update the `DNS` record for the mail server. What `DNS` records do they need to update?

1.  `MX`
2.  `A`
3.  `AAAA`
4.  `SRV`

### 问题 5
What type of network would be a single site with all users in close proximity, secure, and connected by one or more switches?

1.  `WAN`
2.  `MAN`
3.  `LAN`
4.  `CAN`

### 问题 6
Which of the following uses cryptography to confirm that the source server that an email came from was legitimate?

1.  `SPF`
2.  `DMARC`
3.  `Syslog`
4.  `DKIM`

### 问题 7
One of the users in a company keeps raising support tickets for very minor issues. The IT team wants to be able to track him by his IP address. The company is using a `DHCP` server to dynamically allocate IP addresses. How can the IT team set this user up so that he gets the same IP address from the `DHCP` server?

1.  Create a new scope for him
2.  Create a customized lease duration for him
3.  Create a static IP address for him
4.  Create a reservation for him

### 问题 8
Which of the following technologies allows multiple networks to be created within a switch?

1.  `VLAN`
2.  Proxy Server
3.  Firewall
4.  Router

### 问题 9
The spam gateway was fooled by spam coming from many different fake domains. Which of the following can be implemented to prevent these emails from accessing the company network?

1.  Firewall
2.  Content filter
3.  `DMARC`
4.  Proxy server

### 问题 10
A network administrator is planning to divide the company’s network into three subnets. What do they need to configure in the `DHCP` server so that each subnet gets a certain range of IP addresses that are unique to that subnet?

1.  Scope
2.  Reservation
3.  Lease
4.  Superscope

### 问题 11
What is the `DNS` record used by a host with an IP address of `131.122.14.12`?

1.  `A`
2.  `AAAA`
3.  `MX`
4.  `SPF`

### 问题 12
What is the sequence of the `DHCP` handshake?

1.  `ACK`, `OFFER`, `REQUEST`, `DISCOVER`
2.  `DISCOVER`, `OFFER`, `REQUEST`, `ACK`
3.  `DISCOVER`, `REQUEST`, `OFFER`, `ACK`
4.  `REQUEST`, `OFFER`, `DISCOVER`, `ACK`

### 问题 13
Which of the following is a method that can be used to securely connect a remote user’s laptop to a corporate server from overseas?

1.  `WAN`
2.  `SSL`
3.  `DHCP`
4.  `VPN`

### 问题 14
Which of the following tools can be used to prevent spear phishing or spam attacks? (Select all that apply)

1.  Stateless firewall
2.  `SPF`
3.  `DKIM`
4.  `DMARC`

## 2.7 Compare and contrast internet connection types, network types, and their features

### 问题 1
What type of network is the internet an example of?

1.  `MAN`
2.  `PAN`
3.  `WAN`
4.  `LAN`

### 问题 2
A mountaineering team is going to climb Mount Everest for charity. However, once they are 1,000 meters above the base camp, they find that they cannot use their cellular phones to communicate with the admin team back in the base camp. How can they climb the mountain and still communicate with the base camp?

1.  Cable modem
2.  Satellite
3.  `GPS`
4.  Wireless

### 问题 3
An organization using unshielded twisted pair cabling has been suffering from Electromagnetic Interference (`EMI`). What can the organization implement to protect from `EMI` and also provide high-speed communication at the same time?

1.  Fiber
2.  Coax
3.  `STP`
4.  10-BASE T

### 问题 4
What is the main type of connection used by an iPhone 11?

1.  `DSL`
2.  Wireless
3.  Cellular
4.  Satellite

### 问题 5
Which of the following statements are true about a Wireless Internet Service Provider (`WISP`)? (Choose two)

1.  They manufacture the most modem wireless access points
2.  They install long-range fixed-access wireless technology
3.  They provide a bridge between the customer and their service provider networks
4.  They monitor the wireless bandwidth for corporate customers

### 问题 6
Which of the following best describes a `WAN`?

1.  An unsecure network over a very large geographical area
2.  A secure fast network with all users in close proximity
3.  A boundary network that protects the `LAN`
4.  All of the above
