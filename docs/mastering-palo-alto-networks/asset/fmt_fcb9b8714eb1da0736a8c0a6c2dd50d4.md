
# NAT Configuration

Unless you are one of the lucky few organizations that were able to get their very own `A` (`/8`) or `B` (`/16`) class subnets, your internal network segments will most likely be made up of one or more of the well-known RFC1918 private IP address allocations: `10.0.0.0/8`, `172.16.0.0/12`, or `192.168.0.0/16`. NAT is needed in order for your hosts to be able to reach the internet and your customers and partners to reach publicly available resources hosted in your data center. NAT rules can be configured through `Policies` | `NAT`.

For this section, keep the following interface setup in mind:

![](Images/B31589_03_47.png)

Figure 3.47: Interface zone and IP configuration

Address translation comes in different flavors depending on the direction and purpose, each with its own nuances. Let’s first review inbound NAT.

## Inbound NAT

For inbound NAT, it is important to remember that the firewall is zone-based and that the source and destination zones are determined *before* the NAT policy is evaluated:

![](Images/B31589_03_48.png)

Figure 3.48: Packet flow stages

This means that for inbound NAT, the source and destination zones will be identical. The routing table will determine the source zone based on the default route and the destination zone based on the connected network, which is configured on the external interface.

For example, if the `203.0.113.1` internet IP is connecting to the `198.51.100.2` firewall IP to reach the `10.0.0.5` server, the firewall will look up `203.0.113.5` in its routing table. It will find that it only matches the default route, `0.0.0.0/0`, which points from the `ethernet1/1` interface, which is in the `Untrust-L3` zone. It will then look up `198.51.100.2` (the original destination IP in the packet header) and find it in the `198.51.100.0/24` connected network on the `ethernet1/1` interface, which is in the `Untrust-L3` zone.

The `Original Packet` tab needs to have the following:

* The same source and destination zones.
* `Source Address` can be `Any` for generic internet sources, specific IP addresses, or subnets if the source is known.
* `Destination Interface` indicates which interface the packet is headed to. This can be important in cases where there are multiple interfaces with overlapping routes.
* `Service` can be used to restrict which destination port is allowed in the received packets. This will help in cases where the IP space is restricted and `port address translation` (`PAT`) is required to host different services on the same external IP and will prevent over-exposing an internal host.
* `DESTINATION ADDRESS` needs to be a single IP for a one-to-one destination NAT (don’t add a subnet). Having a subnet-based destination NAT is possible, but only for `Session Distribution`.

The `Original Packet` tab of an incoming NAT rule will look similar to the following screenshot:

![](Images/B31589_03_49_new.png)

Figure 3.49: Original Packet NAT translation

In the `Translated Packet` tab, you can set what needs to be changed for the external client to be able to reach the internal server:

* `Source Address Translation` will usually be set to `None`, but it can be set to match an internal interface subnet or loopback interface if required. This would let the server receive a packet sourced from an internal IP, rather than the original internet IP.
* `Destination Address Translation` with `Translation Type` set to `Static IP`, also known as one-to-one NAT, changes the destination IP to a single internal server.
* `Translated Port` can be used if the internal service runs on a different port than the externally advertised one. For example, externally, a web server could be reachable on default SSL port `443`, while on the server itself, the service is enabled on `8443`.

In the `Translated Packet` tab of an inbound NAT rule, we will add the private IP of the destination server, as illustrated here:

![](Images/B31589_03_50.png)

Figure 3.50: Translated Packet NAT translation

Next, let’s take a look at address translation in the opposite direction.

## Outbound NAT

Outbound NAT rewrites the source IP addresses of internal clients to the interface associated with a different zone. This could be an internet-facing zone or one connecting to a partner, VPN, or WAN, as in the following screenshot:

* The source zone will reflect the interface that the clients are connected to.
* The destination zone and destination interface will reflect the egress interface that a routing lookup determines based on the original packet.

![](Images/B31589_03_51.png)

Figure 3.51: Outbound NAT Original Packet

When using an IP pool for source translation, the firewall will use proxy ARP to gain ownership of IP addresses. This means that you don’t need to physically configure all of the IP addresses on an interface, but it is recommended that you have at least the subnet configured on an interface so that the firewall knows which interface is used to broadcast the proxy ARP packets. If the subnet does not exist on an interface, proxy ARP will be broadcast out of all the interfaces.

### Important note

Starting from PAN-OS 10.2.8, 11.0.5 and 11.1.0 this behavior was changed; the firewall will now apply strict Proxy ARP enforcement for source NAT, which means it will only send out proxy ARP for IP addresses in the same subnet as the ingress interface.

For destination NAT, the behavior remains the same.

Let’s take a look at some of the common, and a few special, use cases – NAT rule configurations.

### Hide NAT or one-to-many NAT

The most common implementation of outbound NAT is the infamous `hide NAT`, or many-to-one, which changes the source IP addresses of all internal clients to the external IP(s) of the firewall. It is best to place this rule near the bottom of the rule base as it will catch any non-specific sessions and rewrite the source IP to that of the firewall.

The best option for this type of NAT is `Dynamic IP and Port` (`DIPP`). `DIPP` rewrites the source IP to that of a selected interface or a manually entered IP, IP-range, or subnet, and assigns a random source port to the session on egress, as you can see here:

![](Images/B31589_03_52.png)

Figure 3.52: DIPP to an interface IP or manual selection

`DIPP` supports around 64,000 concurrent sessions per available source IP, multiplied by the oversubscription factor supported by the platform you are deploying these rules on. As a rule of thumb, small platforms commonly support 2x oversubscription, large platforms support 4x, and extra-large platforms up to 8x. When multiple IPs are available, `DIPP` assigns a rewrite IP based on a hash of the source IP so that the same source always gets the same translation address. Once the concurrent allowance for a given translation address is depleted, new sessions will be blocked until existing sessions are freed up.

You can check the current oversubscription ratio by using the following command:

```
admin@PA-220> show running nat-rule-ippool rule <rule name>
VSYS 1 Rule <rule name>:
Rule: <rule name>, Pool index: 1, memory usage: 20344
-----------------------------------------
Oversubscription Ratio:                2
Number of Allocates:                   0
Last Allocated Index:                  0
```

Starting from PAN-OS 11.1.1 and later, `Persistent Dynamic IP and Port` (`PDIPP`) allows you to bind the private source IPs to a specific translated IP so all sessions from the client are translated behind the same post-NAT IP. This helps tremendously with applications like STUN that open multiple subsequent connections and require all the connections to be sourced from the same IP.
