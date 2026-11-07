Using a utility called macof, we can attempt to check if the appropriate switch port controls are in place by flooding the switch port we are connected to with as many MAC addresses as possible.

The syntax of macof is as follows. Check out the man page or --help to get a full description of the available options:

```
macof [-i interface] [-s src] [-d dst] [-e tha] [-x sport] [-y dport] [-n times]
```

In my lab, to test this out, I will run the following command to check if I can use up all the TCAM space:

```
macof -i eth0 -d 192.168.33.1 -n 1000000
```

To protect against MAC address flooding, the key is to limit, as best you can, the number of MAC addresses allowed on a switch port. On Cisco devices, this is done with the switchport port-security set of commands. Here is a basic example of a Cisco switch:

```
Switch(config)# interface gi 0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 5
Switch(config-if)# switchport port-security mac-address sticky
```

# Wireless-based attacks

With the proliferation of wireless networks, the increase in security of these networks has never been more important. As with the wired, wireless has its fair share of utilities to both, audit and attack the various hosts and networks. As a penetration tester, it is our job to make sure that the wireless networks are set up correctly and that there are no other networks that are in range and shouldn't be there, as well as making sure that they are not interfering with the networks in-scope.

In the following section, I will show examples of some of the wireless utilities that I use during penetration testing. There are a lot of great tools out there, and I could probably devote a whole book just going over them, but I wanted to select a sample that shows a variety of testing strategies.