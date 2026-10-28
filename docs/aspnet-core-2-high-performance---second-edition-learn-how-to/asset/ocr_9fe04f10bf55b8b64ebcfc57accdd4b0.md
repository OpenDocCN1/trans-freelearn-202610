# Network diagnostics tools

As we discovered earlier, practically all I/O operations in a virtualized or cloud-hosting infrastructure are now network operations. Disks and databases are rarely local, as this would prevent scaling out horizontally. There are various command-line tools that can help you discover where the API, DB, or any other server you're using is located, and how much latency is present on the connection.

While all of these commands can be run from your workstation, they are most useful when run from a server via a Secure Shell (SSH) or Remote Desktop Protocol (RDP) connection. This way, you can check where your databases, APIs, and storage servers are, in relation to your web servers. Unfortunately, it is common for hosting providers to geographically separate your servers, and put them in different data centers.

For example, if using AWS, then you would want to configure your servers to be in at least the same region, and preferably in the same Availability Zone (AZ), which usually means the same data center. You can replicate (cluster) your DB or file server across AZs (or even across regions) so that your web servers are always talking to a server on their local network. This also adds redundancy, so in addition to increasing performance, it will make your application more resilient to hardware faults or power supply failures.

# Ping

Ping is a simple networking diagnostics tool, available on almost all operating systems. It operates at the IP level and sends an Internet Control Message Protocol (ICMP) echo message to the host specified.

Not all machines will respond to pings, or requests may be blocked by firewalls. However, it's good netiquette to allow servers to respond for debugging purposes, and most will oblige. For example, open a Command Prompt or terminal, and type the following:

```
ping ec2.eu-central-1.amazonaws.com
```

This will ping an Amazon Web Services (AWS) data center in Germany. In the response, you will see the time in milliseconds. From the UK, this round-trip time (RTT) may be something like 33ms, but your results will vary.

On Windows, by default, ping performs four attempts, then exits. On a Unix-like OS (such as macOS, BSD, or Linux), by default it continues indefinitely. Press Ctrl + C to stop and quit.

![](bbox=[0.12, 0.87, 0.18, 0.95])