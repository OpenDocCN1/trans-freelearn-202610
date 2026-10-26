# Troubleshooting connectivity issues

Single user or small group connectivity issues can be resolved by confirming that the networking functions required for a user workstation to access local and remote network resources are functioning properly. The basic requirements or items to confirm include:

+   - Enabling the correct network interface(s) (workstation configuration)
+   - Confirming layer 1 (physical) connectivity
+   - Obtaining an IP address, subnet mask, and default gateway for each interface (DHCP)
+   - Obtaining the MAC address of the default gateway or other local network services (ARP)
+   - Obtaining the IP address of a network service (DNS)
+   - Connecting to a network service (TCP handshake or UDP response)

We'll briefly discuss each of these in order; while the first two steps will not involve using Wireshark, they are a necessary part in a troubleshooting approach. If the connectivity issue is affecting a group of users or a whole office, the first step is probably not applicable.