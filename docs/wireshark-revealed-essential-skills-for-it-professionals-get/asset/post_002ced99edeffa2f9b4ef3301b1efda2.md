
This is a very convenient option. You can choose any field from the packet detail pane in the captured file; right-click on it and you will get a few options, as illustrated in the following screenshot:

![](img/da40f92928403866599198344271d63e_397_0.png)

A couple of options are as follows:

- **Apply as Filter**: This will set a filter according to the field you choose and apply it to the captured data.
- **Prepare a Filter**: This will prepare a filter but not apply it. It will be applied when you click on the Apply button on the right-hand side of the filter window.

In both the options, you can choose to configure a filter:

- **Selected**: This will choose the selected field and parameter
- **Not Selected**: This will choose the the field and parameter that are not selected

For example, right-clicking on the field http.request.method and choosing Selected will come with the filter string http.request.method == GET; while, choosing Not Selected will come with the string `!(http.request.method == "GET")`.

You can also choose the options **... and selected**, **... or selected**, **... and not selected**, or **... or not selected** for structured filters.

## How it works...

The display filter is a proprietary Wireshark language. There are many places where display filters can be used that will be discussed in the later chapters. Additional filters will be introduced in the upcoming recipes of this chapter.

You can always use the autocomplete feature to complete filter strings. For example, if you type in `tcp.f`, as shown in the following screenshot, the autocomplete feature lists possible display filter values that can be created beginning with `tcp.f`, that is, TCP flags (SYN, FIN, RST, and so on).

![](img/da40f92928403866599198344271d63e_399_0.png)

## There's more...

Now we will cover some additional helpful features.

## What is the parameter we filter?

Anytime you mark a specific field in the packet details pane, you will see the correlating filter string in the status bar at the bottom-left corner of the Wireshark window.

![](img/da40f92928403866599198344271d63e_400_0.png)

## Adding a parameter column

You can also right-click on a parameter in the packet pane and choose Apply as Column. This will add a column with the specific parameter. For example, you can choose the parameter tcp.window_size_value and add it as a column to the packet list pane, so you will be able to watch the TCP window size online. This influences TCP performance, as we will learn in Chapter 9, UDP/TCP Analysis.

## Saving the displayed data

To save the displayed data, you can navigate to File | Export Specified Packets... and choose which packets to save.

## Configuring Ethernet, ARP, host, and network filters

In this recipe we will discuss how to configure filters of layers 2 and 3, that is, Ethernet- and IP-based filters respectively. We will also discuss Address Resolution Protocol (ARP) filters.

## Getting ready

In layer 2 we will configure Ethernet-based filters, while in layer 3 we will configure IP-based filters. In Ethernet we have filters based on the Ethernet frame and the MAC address, while in IP we have filters based on the IP packet and address.

The common frame delta filters are as follows:

- `frame.time_delta`: This is used for the time delta between the current and previously captured frames; this will be used in statistical graphs displayed in [Chapter 5](#), Using Advanced Statistics Tools
- `frame.time_delta_displayed`: This is used for the time delta between current and previously displayed frames; this will be used in statistical graphs displayed in [Chapter 5](#), Using Advanced Statistics Tools

Since the time between frames can influence TCP performance significantly, we will use the `frame.time_delta` parameters in statistical graphs for monitoring TCP performance.

The common layer 2 (Ethernet) filters are as follows:

- `eth.addr == <MAC Address>`: This is used to display a specific MAC address
- `eth.src == <MAC Address>`: This is used to get the source MAC address
- `eth.dst == <MAC Address>`: This is used to get the destination MAC address
- `eth.type == <Protocol Type (Hexa)>`: This is used to get the Ethernet protocol types

The common ARP filters are as follows:

- `arp.opcode == <value>`: This is used for ARP requests/responses
- `arp.src.hw_mac == <MAC Address>`: This is used to capture the ARP address of the sender

The common layer 3 (IP) filters are as follows:

- `ip.addr == <IP Address>`: This is used to get the source or destination IP address
- `ip.src == <IP Address>`: This is used to get the source IP address
- `ip.dst == <IP Address>`: This is used to get the destination IP address
- `ip.ttl == <value>, ip.ttl < <value>, or ip.ttl > <value>`: This is used to get IP TTL (Time To Live) values
- `ip.len = <value>, ip.len > <value>, or ip.len < <value>`: This is used to get IP packet length values
- `ip.version == <4/6>`: This is used to get the IP protocol version (Version 4 or Version 6)

## How to do it...

Here we will see some common examples for layer 2 and 3 filters.

| Address format | Syntax | Example |
|---|---|---|
| Ethernet (MAC) address | `eth.addr == xx:xx:xx:xx:xx:xx`<br><br>Here, x = 0 to f.<br><br>`eth.addr == xx-xx-xx-xx-xx-xx`<br><br>Here, x = 0 to f.<br><br>`eth.addr == xxxx.xxxx.xxxx`<br><br>Here x = 0 to f. | `eth.addr == 00:50:7f:cd:d5:38`<br><br>`eth.addr == 00-50-7f-cd-d5-38`<br><br>`eth.addr == 0050.7fcd.d538` |
| Broadcast MAC address | `Eth.addr == ffff.ffff.ffff` | |
| IPv4 host address | `ip.addr == x.x.x.x`<br><br>Here, x = 0 to 255. | `ip.addr == 192.168.1.1` |
| IPv4 network address | `ip.addr == x.x.x.x/y`<br><br>Here x = 0 to 255, y = 0 to 32. | `ip.addr == 192.168.200.0/24`<br><br>This covers all the addresses in the network 192.168.200.0 mask 255.255.255.0. |
| IPv6 host address | `ipv6.addr == x:x:x:x:x:x:x:x`<br><br>`ipv6.addr == x:x:x:x:x:x`<br><br>Here in the format of nnnn, n = 0 to f (Hex). | `ipv6.addr == fe80::85ab:dc2e:ab12:e6c7` |
| IPv6 network address | `ipv6.addr == x::/y`<br><br>Here x = 0 to f (Hex) and y = 0 to 128. | `ipv6.addr == fe80::/16`<br><br>This covers all the addresses that start with the 16 bits `fe80`. |

The table refers to `ip.addr` and `ipv6.addr` filter strings. The value for any field that has an IP address value can be written the same way.

## Ethernet filters

These are classified into two categories:

- To display only packets sent from or to specific MAC addresses, use something like these: `eth.src == 10:0b:a9:33:64:18 and eth.dst == 10:0b:a9:33:64:18`
- To display only broadcasts, use `Eth.dst == ffff.ffff.ffff`

## ARP filters

These are classified into two categories:

- To display only ARP requests, use `arp.opcode == 1`
- To display only ARP responses, use `arp.opcode == 2`

## IP and ICMP filters

- To display only packets from a specific IP address, use something like this: `ip.src == 10.1.1.254`
- To display only packets that are not from a specific address, use something like this: `!ip.src == 64.23.1.1`
- To display only packets between two hosts, use something like these: `ip.addr == 192.168.1.1 and ip.addr == 200.1.1.1`
- To display only packets that are sent to multicast IP addresses, use something like this: `ip.dst == 224.0.0.0/4`
- To display only packets coming from the network 192.168.1.0/24 (mask 255.255.255.0), use `ip.src==192.168.1.0/24`
- To display only IPv6 packets to/from specific addresses, use something like the following:
  - `ipv6.addr == ::1`
  - `ipv6.addr == 2008:0:130F:0:0:09d0:666A:13ab`
  - `ipv6.addr == 2006:0:130f::9c2:876a:130b`
  - `ipv6.addr == ::`

## Complex filters

- To check for packets sent from the network 10.0.0.0/24 to a website that contains the word packt, use `ip.src == 10.0.0.0/24 and http.host contains "packt"`
- To check for packets sent from the network 10.0.0.0/24 to websites that end with
