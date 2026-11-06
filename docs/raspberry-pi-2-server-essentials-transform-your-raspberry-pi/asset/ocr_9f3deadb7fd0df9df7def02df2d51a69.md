To check whether your LAN is up and running, just type `ifconfig`, and you will get text containing your current settings:

```
eth0      Link encap:Ethernet  HWaddr b8:27:eb:45:bb:fa
          inet addr:192.168.1.135  Bcast:192.168.1.255  Mask:255.255.255.0
          inet6 addr: fe80::ba27:ebff:fe45:bbfa/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:89  errors:0  dropped:0  overruns:0  frame:0
          TX packets:96  errors:0  dropped:0  overruns:0  carrier:0
          collisions:0  txqueuelen:1000
          RX bytes:7943  (7.7 KiB)  TX bytes:14104  (13.7 KiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr:  ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:16436  Metric:1
          RX packets:0  errors:0  dropped:0  overruns:0  frame:0
          TX packets:0  errors:0  dropped:0  overruns:0  carrier:0
          collisions:0  txqueuelen:0
          RX bytes:0  (0.0 B)  TX bytes:0  (0.0 B)
```

Let's try to understand the different keywords from the preceding output:

+   - **HWaddr**: This is your Pi's MAC address, which identifies a vendor and should ideally provide a globally unique address.
+   - **inet addr**: This is your current LAN IP address, which belongs to a private range of either `10.x.y.z` (Class A), `172.16.y.x` (Class B), or `192.168.x.x` (Class C). They are known as private addresses because they do not exist on the Internet.
+   - **Bcast**: This is a reserved address that's calculated by your network mask, and it transmits global messages within your private network.
+   - **Mask**: This is used in all networks but should only be concerned with advanced configurations. Masks divide a network into subnetworks and depend on the class your network belongs to.
+   - **inet6 addr**: This is your IPv6 address. It will show only if your router supports this, but it is more difficult to remember.
+   - **RX**: This shows how many packets have been received.
+   - **TX**: This shows how many packets have been sent.

# The eth0 port

The Raspberry Pi will typically use **eth0** as its on-board LAN interface. If you add USB LAN devices, they will have an incremented number after *eth*; this will increment the interface number.