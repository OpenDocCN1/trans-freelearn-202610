```
80/tcp open http
```

```
Nmap done: 256 IP addresses (256 hosts up) scanned in 1024.26 seconds
```

Furthermore, looking at the single open port reveals a simple, static web page—nothing special, no dynamic content, nor any type of web application to attack, as we can see here:

![](bbox=[0.13, 0.246, 0.868, 0.622])

Figure 16.1 – Company Z impenetrable website

We run Nikto, Nessus, and Nmap scans for the server running on 10.0.0.120 but cannot find any vulnerabilities for the server, the web service, or the web page running on the web server. Time to start thinking about a visit to the plant. While we prepare a trip to site, we should set up a periodic scan to see if we can detect a change in the public presence of Company Z.