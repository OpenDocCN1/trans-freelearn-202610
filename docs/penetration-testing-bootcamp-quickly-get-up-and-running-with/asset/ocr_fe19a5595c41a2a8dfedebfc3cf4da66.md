# Summary

The challenge presented to me at the start of this chapter was to be able to create a secure tunnel to transfer our evidence offsite, while making sure we could get around any potential security devices. I also needed a way to get back into the environment from my C&C server without having the company create associated firewall access control rules and NAT rules.

Utilizing stunnel and reverse ssh, I was able to accomplish this. stunnel allowed me to have a SSL tunnel from the internal server to my external C&C server. The SSL tunnel is key since most firewalls will allow outbound SSL and most IPS setups will not decrypt the payload, and therefore it will go through without being inspected. The reverse ssh tunnel allowed the C&C server to piggyback off the already established ssh connection that originated from the inside server. Automating this allowed the entire setup to come back in the event of a reboot of the Raspberry Pi.

Using the script that I created, I was able to transfer files into the correct directory as well as log each transfer to a log file for auditing purposes. To make sure I made the best use of this tunnel, I talked about how to compress and split the files, as well as how to put them all back together on the other end.

In the next chapter I will jump right into the penetration test by performing vulnerability scans against some of our targets and then try to exploit these vulnerabilities with tools such as Metasploit. I will also jump into the user side of exploitation by looking into BeEF for browser-based exploitation and SET.