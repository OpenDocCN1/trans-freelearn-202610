
Utilizing Wireshark, you can verify what firewalk is actually doing. Here, we can drill into the Layer 3 and 4 information in the packet, and see the TTL being set to 2:

![](img/095df2cd1930baecbcecaa51ee3417aa_54_0.png)

## Detecting a web application firewall

Network-based firewalls are not the only type of firewall you may discover along the way. Web Application Firewalls, or WAFs, are very commonly used to protect web-based applications. If you are unfamiliar with an environment, detecting a WAF can help lay out the web application infrastructure. To help us figure out this bit of information, we are going to utilize a tool called WAFW00F. WAFW00F can help you determine whether there is that extra layer of security prior to the web servers.

WAFW00F can detect the presence of a lot of different WAF types. By running the wafw00f command with the -l flag, you can see list of currently defined WAFs. Here is the current list from my lab. If one of these is not detected, don't fret; the wafw00f command will still inform you that a generic WAF has been detected:

- Profense
- NetContinuum
- Incapsula WAF
- CloudFlare

## Information Gathering

- USP Secure Entry Server
- Cisco ACE XML Gateway
- Barracuda Application Firewall
- Art of Defence HyperGuard
- BinarySec
- Teros WAF
- F5 BIG-IP LTM
- F5 BIG-IP APM
- F5 BIG-IP ASM
- F5 FirePass
- F5 Trafficshield
- InfoGuard Airlock
- Citrix NetScaler
- Trustwave ModSecurity
- IBM Web Application Security
- IBM DataPower
- DenyALL WAF
- Applicure dotDefender
- Juniper WebApp Secure
- Microsoft URLScan
- Aqtronix WebKnight
- eEye Digital Security SecureIIS
- Imperva SecureSphere
- Microsoft ISA Server

The wafw00f command is extremely straightforward to run. You just need to specify the URL that you would like to check. I have run two examples, to show you a site that matches one of the preceding WAF types, as well as one that just lets you know that one exists.

In this example, you can see that a WAF was detected, and the actual type was determined:

![](img/095df2cd1930baecbcecaa51ee3417aa_55_0.png)

In the next example, `wafw00f` was unable to figure out the exact WAF being used but still lets you know one exists:

![](img/095df2cd1930baecbcecaa51ee3417aa_56_0.png)

## Protocol fuzzing with DotDotPwn

DotDotPwn is a slick multi-protocol fuzzer to discover traversal directory vulnerabilities within web servers. Fuzzing is the testing technique of looking for poor coding or security loopholes in software applications such as web servers or even operating systems. Because of this, DotDotPwn makes a good reconnaissance tool for finding various issues within the web server stack that you can later exploit.

Getting the most information about the environment now makes the exploitation phase much easier. We will note everything we can find and then exploit it at the appropriate time. Do not rush through the reconnaissance phase, as it will just lessen the overall quality of the penetration test. The more we can find now, the more we can exploit later.

First thing to know about `dotdotpwn` is that it supports many different protocols or modules. We will focus on the `http` module during the lab but there are other modules available such as `tftp` and `ftp`. Here are the available switches when running `dotdotpwn` from the CLI:

```
Usage: ./dotdotpwn.pl -m <module> -h <host> [OPTIONS]
Available options:
-m Module [http | http-url | ftp | tftp | payload | stdout]
-h Hostname
-O Operating System detection for intelligent fuzzing (nmap)
-o Operating System type if known ("windows", "unix" or "generic")
-s Service version detection (banner grabber)
-d Depth of traversals (e.g. deepness 3 equals to ../../..; default: 6)
-f Specific filename (e.g. /etc/motd; default: according to OS detected, defaults in TraversalEngine.pm)
-E Add @Extra_files in TraversalEngine.pm (e.g. web.config, httpd.conf, etc.)
-S Use SSL - for HTTP and Payload module (use https:// for in url for http-uri)
-u URL with the part to be fuzzed marked as TRAVERSAL (e.g. http://foo:8080/id.php?x=TRAVERSAL&y=31337)
-k Text pattern to match in the response (http-url & payload modules - e.g. "root:" if trying /etc/passwd)
-p Filename with the payload to be sent and the part to be fuzzed marked with the TRAVERSAL keyword
-x Port to connect (default: HTTP=80; FTP=21; TFTP=69)
-t Time in milliseconds between each test (default: 300 (.3 second))
-X Use the Bisection Algorithm to detect the exact deepness once a vulnerability has been found
-e File extension appended at the end of each fuzz string (e.g. ".php", ".jpg", ".inc")
-U Username (default: 'anonymous')
-P Password (default: 'dot@dot.pwn')
-M HTTP Method to use when using the 'http' module [GET | POST | HEAD | COPY | MOVE] (default: GET)
-r Report filename (default: 'HOST_MM-DD-YYYY_HOUR-MIN.txt')
-b Break after the first vulnerability is found
-q Quiet mode (doesn't print each attempt)
-C Continue if no data was received from host
```

Now that we have all the options, we will test them against our host in our lab 192.168.1.134. In our test, we will be using the method http with the -m switch as well as limiting the detection of our traversal to 3 due to time limits. We will also be specifying our host with the -h switch. Here is the command we will be running versus our lab web server:

```
dotdotpwn -m http -d 3 -h 192.168.1.134
```

Since the output of dotdotpwn stops while conducting the traversal tests, you can use tcpdump to verify that it is checking and actively doing something. While this is running, you will see the output of all the directory traversal tests when something is found. Be patient though; this can take a long time to complete:

```
[*] HTTP Status: 404 | Testing Path: http://192.168.1.134:80/..%5C..%5Cwindows%5Csystem32%5Cdrivers%5Cetc%5Chosts%00
[*] HTTP Status: 404 | Testing Path: http://192.168.1.134:80/..%5C..%5Cwindows%5Csystem32%5Cdrivers%5Cetc%5Chosts%00index.html
[*] HTTP Status: 404 | Testing Path: http://192.168.1.134:80/..%5C..%5Cwindows%5Csystem32%5Cdrivers%5Cetc%5Chosts%00index.htm
[*] HTTP Status: 404 | Testing Path: http://192.168.1.134:80/..%5C..%5Cwindows%5Csystem32%5Cdrivers%5Cetc%5Chosts%00index.htm
[*] HTTP Status: 404 | Testing Path: http://192.168.1.134:80/..%5C..%5Cwindows%5Csystem32%5Cdrivers%5Cetc%5Chosts%00
[*] HTTP Status: 404 | Testing Path: http://192.168.1.134:80/..%5C..%5Cwindows%5Csystem32%5Cdrivers%5Cetc%5Chosts%00index.html
[*] HTTP Status: 404 | Testing Path: http://192.168.1.134:80/..%5C..%5Cwindows%5Csystem32%5Cdrivers%5Cetc%5Chosts%00index.html
[*] HTTP Status: 404 | Testing Path: http://192.168.1.134:80/..%5C..%5Cwindows%5Csystem32%5Cdrivers%5Cetc%5Chosts%00index.htm
[*] HTTP Status: 404 | Testing Path: http://192.168.1.134:80/..%5C..%5Cwindows%5Csystem32%5Cdrivers%5Cetc%5Chosts%00index.htm
[+] Fuzz testing finished after 51.25 minutes (3075 seconds)
[+] Total Traversals found: 1080
[+] Report saved: Reports/192.168.1.134_09-30-2016_00-44.txt
```

One option is to use the -b, which will stop the testing as soon as it finds a vulnerable host. This is a good way to find just one traversal issue and then stop, but not typically a good move in terms of a penetration test, as you typically want finding and documentation always in a system.

## Using Netdiscover to find undocumented IPs

Netdiscover is a great tool for finding potential IP addresses on the network for further examination. It accomplishes this by sending out ARP messages for the given network you specify. By running this tool, you can discover any live hosts on any type of network, wired or wireless. It also attempts to discover the vendor by the MAC address, which can be very helpful in finding vulnerabilities to exploit.

Next is a screenshot with `netdiscover` tool running in my lab:

![](img/095df2cd1930baecbcecaa51ee3417aa_58_0.png)

This is a `tcpdump` command of ARP requests that are being sent out by the `netdiscover` tool to discover these live IP addresses:
