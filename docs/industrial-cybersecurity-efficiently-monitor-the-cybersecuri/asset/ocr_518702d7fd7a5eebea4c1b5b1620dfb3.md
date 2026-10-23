Some forms of active security monitoring include the following:

+   - Network scanning to interrogate and examine network-connected devices
+   - Host-based agents that can scan the host for security-related issues and malicious content
+   - Manually examining endpoints for signs of malicious activity and content

Let's look at each of them in detail.

# ## Network scanning

In this section, we will discuss the various methods around network scanning. We will look at common tools and techniques and discuss the applicability of those tools and techniques to uncover certain types of network and host information. We will start by looking at IP and port scanning methodologies.

# ## IP and port scanning

Any system that wants to communicate over the network will at least need an **Internet Protocol (IP)** address to identify itself. Applications on a system use this IP address, along with a port number (TCP or UDP), to create a socket so that the application can communicate with other applications on other systems. The IP address of a system is like the street address of an apartment complex, while the port number represents the apartment number within the building. So, when you want to send a letter to a person who lives in the apartment complex, you would use the street address and the apartment number in combination to reach your targeted receiver.

Knowing what IP addresses are **live** (present and running) on your network is valuable information for figuring out all your assets or to see whether any new assets have appeared on your network since the last time you scanned it. Additionally, figuring out what port numbers are **open** (respond to network requests) can give us information about the applications or services running on that live system.

Some well-known port scanning tools include the following:

+   - Nmap (https://nmap.org/)
+   - Angry port scanner (https://angrjip.org/)
+   - Netcat (http://netcat.sourceforge.net/)