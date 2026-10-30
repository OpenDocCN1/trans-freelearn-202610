The first thing we do is specify that the IP address of the DNS server and DHCP server is the same. To do this, add the following line at the end of the file:

```
dhcp-option=6,192.168.1.1
```

Now that the clients know where the DNS server is located, we need to create a lookup database that contains the IP addresses with their corresponding names. Although in actual DNS servers a more sophisticated database platform is used, for example SQL, here we will use a simple text file as a demonstration. Add the following two lines to the configuration file:

```
no-hosts
```

```
addn-hosts=/etc/hosts.dnsmasq
```

Here, the no-hosts line tells dnsmasq to ignore the default /etc/hosts file for DNS queries. And the next line specifies that our own file, /etc/hosts.dnsmasq, should be used for DNS queries.

The next step in this process is to create our hosts file. To do that, enter the following command:

```
sudo nano /etc/hosts.dnsmasq
```

Since this file that didn't exist before, we should now be editing a blank file. New entries can be added in a specific format, which is the IP Address, followed by a tab, followed by the name. For example:

```
192.168.1.1    server
```

Here, the first part is the IP address, the second part is the tab spacing, and the third part is the name associated with that IP address.

> We have to add a tab instead of multiple spaces for it to work.

Now, save and exit this file. Make sure that the server is the only device connected to the networking switch. Unplug all other devices before restarting the dnsmasq service. Restart the service with the following command:

```
sudo service dnsmasq restart
```

But there is one more thing. Since our server is set up with a static IP address, we also need to tell the DHCP server to use its own DNS server. We do that through a file named /etc/resolv.conf. Open it for editing by:

```
sudo nano /etc/resolve.conf
```