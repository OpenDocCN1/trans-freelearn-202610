After the interfaces, snmp-check will go through and show the current L3 interfaces, the IP/Netmask, and active TCP ports and UDP ports that are being listened on. You can refer to the following screenshot. This is great information that allows you to run further tests against that device and gain a better understanding of the topology:

```
[*] Network IP:

Id          IP Address      Netmask        Broadcast
121         192.168.1.5     255.255.255.0  1

[*] TCP connections and listening ports:

Local address   Local port   Remote address   Remote port   State
192.168.1.5     22           192.168.33.3     64906         established

[*] Listening UDP ports:

Local address   Local port
192.168.1.5     161
192.168.1.5     162
192.168.1.5     2228
192.168.1.5     10002
192.168.1.5     63408
```

# Rogue DHCP server

Most organizations run DHCP servers within their environment; yet, very few protect against starvation attacks. The configuration required to protect the DHCP server is very minimal, but powerful. In my penetration test, I will try to exhaust the current DHCP server, spin up my own DHCP server to have clients obtain addresses from me, and then send their traffic through me. Here are the step by step configurations needed to accomplish this task:

+   1. First, we need to set up the networking for our new interface. I will need to add a subinterface on my main Ethernet interface. Try to make the IP look as close to the current default gateway as possible. So, if the organization is using 192.168.22.1, try using 192.168.22.11, or if they are using 192.168.45.254, try using 192.168.45.154. Again the goal here is to try and trick the eyes of the users so they don't notice a change with just a quick glance:

```
ifconfig eth0:1
ifconfig eth0:1 192.168.33.111 netmask 255.255.255.0
route add default gw 192.168.1.1 eth0:1
```