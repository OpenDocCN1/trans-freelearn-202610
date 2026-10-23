The general approach to network scanning is to minimize the amount of traffic we send out on the network. As you will see in the next section, we will follow a pattern for this:

+   * Find out what is live on the network using a limited scan, just enough to detect whether an endpoint is reachable.
+   * After determining what systems are live, use the output from that scan as the source for a more intense scan that reveals services, OS details, and other specifics about the discovered endpoint.
+   * Record and log everything you do. This includes logging output scan results to files, as well as adding comments and storing them in an organized way so that we don't have to repeat a scan because we accidentally lost the scan output in our terminal session.

The setup for the lab environment we will be using throughout this book is explained in Appendix 1.

# ## Nmap

Network mapper, o Nmap, is arguably the most popular network scanning tool available. It can scan for live systems, open ports, vulnerabilities, OS details, and much more. Nmap is an open source tool that was created by Gordon Lyon in 1997 and is freely available from https://nmap.org/:

![](bbox=[0.34, 0.54, 0.62, 0.86])