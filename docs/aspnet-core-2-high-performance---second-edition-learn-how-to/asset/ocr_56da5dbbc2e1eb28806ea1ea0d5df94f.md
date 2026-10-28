To programmatically perform a DNS lookup, follow these steps:

+   1. Add the System.Net namespace.
+   
+       using System.Net;
+   
+   2. To resolve a hostname to an IP address, use the following static method. This will return an array of IP addresses, although there will usually only be one.
+   
+       var results = await Dns.GetHostAddressesAsync(host);
+   
+   3. To resolve an IP address to a hostname, use the following method instead:
+   
+       var revDns = await Dns.GetHostEntryAsync(result);
+   
+   4. If successful, then the hostname will be available as revDns.HostName.

To programmatically ping a host, follow these steps:

+   1. Add the System.Net.NetworkInformation namespace.
+   
+       using System.Net.NetworkInformation;
+   
+   2. You can then instantiate a new Ping object.
+   
+       var ping = new Ping();
+   
+   3. With this object, you can now ping an IP address or hostname (that will perform a DNS lookup internally) by using the following method:
+   
+       var result = await ping.SendPingAsync(host);
+   
+   4. You can get the status of the ping with result.Status and, if successful, then you can get the RTT in milliseconds with result.RoundtripTime.