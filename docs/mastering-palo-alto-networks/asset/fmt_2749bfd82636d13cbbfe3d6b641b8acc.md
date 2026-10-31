
# VWire and Layer 3 Interface Configuration

As illustrated in the following screenshot, you will need to select the two interfaces with which you will form a VWire connection. If the VWire interface is placed over a trunked link (one that contains the `VLAN/802.1Q` tags), you need to indicate which ones are allowed. If you want to allow all tags, set `0-4094`. If you want to add single tags or ranges, you can add integers or ranges, separated by commas (for example, `5,15,30-70,100-110,4000`). **Multicast firewalling** needs to be checked if you want to be able to block or otherwise apply security policies to multicast traffic. If unchecked, multicast is forwarded across VWire without checking the security rules. Enabling **Link State Pass Through** brings the opposite interface down if one side loses its connection.

This ensures that both the client and server sides see the link go down and respond accordingly:

![](Images/B31589_02_49.png)

Figure 2.49: VWire configuration

## The Layer 3 interface

A Layer 3 interface is a routed interface. This means it has an IP address and can be used as a default gateway for clients on the inside connected to it or a next hop for a routing device. On the outside, it can communicate with ISP routers and forward traffic out to the internet. We’ll first look at the interface.

### Exploring the interface

In the **Config** tab of the interface, you need to assign a **Virtual Router** (`VR`) and a security zone (we’ll learn more about the `VR` later in this chapter, so don’t worry about it just yet). This zone will represent the subnet(s) connected to it when traffic needs to flow from one interface to another:

![](Images/B31589_02_50.png)

Figure 2.50: Layer 3 interface configuration

The IP configuration can be statically configured as an IP/subnet. If needed, multiple IP/subnets can be added to represent additional networks that are directly connected to the interface:

![](Images/B31589_02_51.png)

Figure 2.51: Layer 3 interface IP

A Layer 3 interface can also be set as a **Point-to-Point Protocol over Ethernet** (`PPPoE`) client if the upstream connection is provided by a broadband ISP over cable or DSL. In this case, in the **General** tab, the ISP authentication username and password can be configured:

![](Images/B31589_02_52.png)

Figure 2.52: Layer 3 PPPoE

In the `PPPoE` | **Advanced** tab, you set the authentication protocol to `PAP`, `CHAP`, `auto`, or `none`. If the ISP has provided you with a static IP, you can configure it here and you can add an access concentrator and service string if the ISP requires them to be able to connect. If required, you can disable adding the default route received by the ISP to the routing table. Some ISPs require PPPoE clients to be in a passive state as they initiate the connection. You can enable this here:

![](Images/B31589_02_53.png)

Figure 2.53: Layer 3 PPPoE Advanced options

Once you’ve configured the interface, click **OK** and commit the change. Once the commit has been completed, you can click on **Show PPPoE Client Runtime Info** from the **Network** | **Interfaces** | **Ethernet** page to receive information on the connection. From the CLI, you can issue the following command to see the same output:

```
admin@PA-440> show pppoe interface <interface>
```

The **DHCP Client** tab allows you to configure the interface as a DHCP client, receiving its IP information from an upstream DHCP server:

![](Images/B31589_02_54.png)

Figure 2.54: Layer 3 DHCP Client advanced options

You can choose to automatically accept the default route or disable this functionality and manually define the default route in the `VR`. If needed by the ISP, you can send the system or custom hostname, and you can determine the metric for the default route.

In the **Layer3** | **Advanced** tab, we gain access to several advanced features:

![](Images/B31589_02_55.png)

Figure 2.55: Layer 3 advanced options

Here we can set the link speed, duplex, and state of the interface. In the **Other Info** tab, we can add the management profile we saw earlier, change the **maximum transmission units** (`MTUs`), and enable **Adjust TCP MSS** with custom values to change the MSS header value in TCP packets flowing through the firewall.

In the other tabs available, we can also set static IPV4 ARP Entries, which can be useful in case an upstream device requires **a static ARP entry**, IPV6 **Neighbor Discovery** (`ND`) **Entries**, or **Neighbor Discovery Protocol** (`NDP`) **Proxy**. A **Link Layer Discovery Protocol** (`LLDP`) profile can also be added to interface with switches.

Lastly, if the external interface has a dynamic IP assigned to it by the ISP, a DDNS hostname can be set up. Currently, four vendors are supported: DuckDNS, afraid.org, no-IP, and Palo Alto:

![](Images/B31589_02_56.png)

Figure 2.56: DDNS configuration

For the Layer 3 subnets and IP addresses to be reachable across interfaces, they need to be added to a routing table; this is accomplished in the `VR`.

### VR

A `VR` is the routing element of the firewall, but, as the name suggests, it is not made up of a single engine but rather, a routing set that an interface is subscribed to. Each Layer 3, loopback, and VLAN interface needs to be associated with a `VR`, but multiple `VR`s can be used on a system. Not all interfaces need to be associated with the same `VR`.

You can configure the default `VR` or add new `VR`s from the **Network** | **Virtual Routers** menu. In the **Router Settings** tab of a `VR`, you can see and add interfaces associated with this `VR` and adjust the administrative distances if needed. An administrative distance associates a priority with a routing protocol. By default, static routes have a higher priority (lower administrative distance) than **Open Shortest Path First** (`OSPF`), but you can change this priority if you want `OSPF` routes to have priority and only use static routes if `OSPF` becomes unavailable. Routes within the same routing protocol can be assigned a metric to give them a higher (lower metric) or lower (higher metric) priority. Routes inside the same type (static, OSPF, etc.) are first prioritized based on the size of their subnet. A smaller subnet (for example, `/32`) will have priority over a larger subnet (for example, `/16`). Next, the metric will be used to determine priority for networks with the same subnet mask with the lower metric having priority (for example, `10.0.0.0/24 metric 10` will have priority over `10.0.0.0/24 metric 20`):

![](Images/B31589_02_57.png)

Figure 2.57: VR settings

In the **Static Routes** tab, you can add destination routes as needed. By default, the firewall loads all the connected (configured on a Layer 3, loopback, or VLAN interface) networks in the routing table; adding static routes makes remote networks available from a routing perspective.

One of the first routes you may need to configure is the “default route,” which allows clients to connect to the internet. If a Layer 3 interface is configured as a `PPPoE` or DHCP Client, an **Automatically Create Default Route** option can be enabled, which will automatically add the default route based on the upstream ISP.

The **Destination** for the default route is `0.0.0.0/0`. A regular route could have a smaller subnet, such as `172.16.0.0/24`.

The **Interface** option indicates what the egress interface will be. If the route is pointing to the internet, the interface will be the one where the ISP router is connected.

**Next Hop** has several options:

*   **IP Address**: The IP of the upstream router to forward packets to.

*   **Next VR**: Whether the packet needs to be handed over to a different `VR` on the same device.

*   `FQDN`: If the upstream router has a dynamic IP, it could be useful to use an `FQDN` that is dynamically updated by a DNS record.
