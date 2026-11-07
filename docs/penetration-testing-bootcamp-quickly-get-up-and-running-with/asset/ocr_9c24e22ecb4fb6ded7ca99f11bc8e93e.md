# Denial-of-service checks

Sometimes, the intention is not to break into the system or steal any sort of information, but rather take the infrastructure down. Systems within the network may handle large amounts of bogus traffic inefficiently, and therefore, cause the CPU and memory to rise to the point that the system becomes unstable or unusable. There are ways to prevent this on the infrastructure side, so I will be testing to see how the infrastructure responds to such events.

# Various attacks with hping3

hping3 is a packet generator utility and protocol analyzer that is extremely flexible. This utility can be used for a number of different tests. First, we will be testing embryonic connection limits on our firewalls. Embryonic connections are half-open connections, which means we are going to only send SYN packets and never respond after that. The goal is to fill the connection tables on the firewall with fake connections so that it fills up and cannot accept any more connections. Using hping3, I will generate a bunch of SYN connections against the firewall to see how it handles the embryonic connections. The hping3 has a lot of options and switches available for running it. So I will not be showing all the options, but rather just explain the switches that I will use.

![](bbox=[0.13, 0.52, 0.22, 0.62])

Prior to running any command for the first time, make sure to either check out the man page for the tool or run the command with something such as -help or /?. Commands and switches sometimes change when new versions are released.

For running this test, I will be using the following syntax :

```
Hping3 -c 30000 -d 120 -S -p 22 --flood 192.168.33.1
-c - Number of packets to send
-d - Size of the packet to send
-S - SYN packets only
-p - Port to use
--flood - Send the packets as fast as possible
```