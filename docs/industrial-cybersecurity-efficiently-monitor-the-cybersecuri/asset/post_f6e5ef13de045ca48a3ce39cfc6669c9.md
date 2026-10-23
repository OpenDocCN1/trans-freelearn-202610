
[-] Ensuring yum-utils is installed... SUCCESS
[-] This installer will:
[-] Install MongoDB
[-] Install RITA to /usr/local/bin/rita
[-] Create a runtime directory for RITA in /var/lib/rita
[-] Create a configuration directory for RITA in /etc/rita
[-] Zeek IDS is already installed
Note: It is now time to select capture interface(s). Keep the following in mind when making selections:
1. The interfaces you most likely want to use for capturing start with "eth" or "en" (e.g. eth0, eno1, enp1s0, enx78e7d1ea46da).
You will generally NOT want to use loopback, bridged, or virtual interfaces (e.g. lo, br-c446eb08dde, veth582437d).
If you choose to select interfaces belonging to the latter category, proceed at your own risk.
2. Ensure that your capture interfaces are up before continuing.

3. At this point in the installation process, RITA will ask whether we want to create a new Zeek config file; answer with yes – y:

```
Would you like to continue running the zeek configuration script and generate a new node.cfg file? (y/n) ? y
Continuing, all requirements met
This system has 8 cores.
The potentially sniffable interfaces are: br-c87c409d9f51 ...
Here are the stats for br-c87c409d9f51
5: br-c87c409d9f51: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default
link/ether 02:42:6e:4a:16:53 brd ff:ff:ff:ff:ff:ff
RX: bytes packets errors dropped overrun mcast
84 3 0 0 0 0
TX: bytes packets errors dropped carrier collsns
8722 69 0 0 0 0
```

4. RITA can be installed as a service, continuously monitoring on our sniffing interfaces. However, we are not going to use that functionality, so answer *no* to the following question (depending on the number of interfaces on the VM, the question might repeat several times):

```
Would you like to include it as a sniff interface (y/n)?
n
...
[-] Installing MongoDB... SUCCESS
[!] Starting MongoDB and enabling on startup.
Created symlink from /etc/systemd/system/multi-user.target.wants/mongod.service to /lib/systemd/system/mongod.service.
[!] Starting MongoDB process completed.
[!] You can access the MongoDB shell with 'mongo'.
[!] If you need to stop MongoDB,
[!] run 'sudo systemctl stop mongod'.
[-] Installing RITA... SUCCESS
[!] To finish the installation, reload the system profile with
[!] 'source /etc/profile'.

Brought to you by Active CounterMeasures

Thank you for installing RITA! **Happy hunting!**
```

5. As the installer indicates after completion, we are now ready to start hunting. The first thing we need to do now is to import our Zeek logs into a RITA database. The following command will instruct RITA to import all zeek notices from the `/nsm/zeek/logs/2020-09-07/` location (your location will be different as it is tied to the date) and store the details in a new **rolling** database (a rolling database allows data to be added later) called `zeek-1`:

```
pac@secOnion-002:~/tools/rita$ rita import /nsm/zeek/
logs/2020-09-07 zeek-1 -rolling
[+] Importing [/nsm/zeek/logs/2020-09-07]:
[-] Verifying log files have not been previously
parsed into the target dataset ...
[-] Processing batch 1 of 1
```

```
[-] Parsing logs to: zeek-1 ...
[-] Parsing /nsm/zeek/logs/2020-09-07/dns.20:16:49-21:00:00.log.gz -> zeek-1
...
[-] Parsing /nsm/zeek/logs/2020-09-07/dns.23:00:00-00:00:00.log.gz -> zeek-1
[-] Host Analysis:          113 / 113
[==================] 100 %
[-] Uconn Analysis:         123 / 123
[==================] 100 %
[-] Exploded DNS Analysis:  180 / 180
[==================] 100 %
[-] Hostname Analysis:      180 / 180
[==================] 100 %
[-] Beacon Analysis:        123 / 123
[==================] 100 %
[-] UserAgent Analysis:     5 / 5
[==================] 100 %
[!] No certificate data to analyze
[-] Updating blacklisted peers ...
[-] Indexing log entries ...
[-] Updating metadatabase ...
[-] Done!
```

6. We need to repeat this process for every subfolder under /nsm/zeek/logs/ to make sure we import all logs. This can be scripted though and can be automated with a cron script.

7. After all relevant Zeek logs are imported, we can now instruct RITA to show some interesting stats. Let's look at the help for RITA:

```
pac@secOnion-002:~/tools/rita$ rita -h
NAME:
    rita - Look for evil needles in big haystacks.
USAGE:
    rita [global options] command [command options]
    [arguments...]
VERSION:
    v3.3.1
COMMANDS:
```

| Command | Description |
|---|---|
| `delete, delete-database` | Delete imported database(s) |
| `import` `target database` | Import zeek logs into a |
| `html-report` `an analyzed database` | Create an html report for |
| `show-beacons` `signs of C2 software` | **Print hosts which show** |
| `show-bl-hostnames` `which received connections` | Print blacklisted hostnames |
| `show-bl-source-ips` `initiated connections` | Print blacklisted IPs which |
| `show-bl-dest-ips` `received connections` | Print blacklisted IPs which |
| `list, show-databases` `currently stored` | Print the databases |
| `show-exploded-dns` `covert dns channels` | Print dns analysis. Exposes |
| `show-long-connections` `relevant information` | Print long connections and |
| `show-strobes` | Print strobe information |
| `show-useragents` `information` | Print user agent |
| `test-config` `file for validity` | Check the configuration |
| `help, h` `help for one command` | Shows a list of commands or |

GLOBAL OPTIONS:
`--help, -h` show help
`--version, -v` print the version

8. We can see RITA displays beaconing stats with the show-beacons flag. Let's see what we can find in our environment:

```
pac@secOnion-002:~/tools/rita$ rita show-beacons zeek-1
Score,Source IP,Destination IP,Connections,Avg Bytes,Intvl Range,Size Range,Top Intvl,Top Size,Top Intvl Count,Top Size Count,Intvl Skew,Size Skew,Intvl Dispersion,Size Dispersion
0.996,172.25.100.220,222.222.222.222,594,862,265108,432,600,318,534,569,0,0,0,0
0.834,172.25.100.100,204.79.197.200,98,152,315934,0,7,152,30,98,0,0,0,0
0.762,172.25.100.211,52.241.128.114,1595,534,4551,0,224,534,42,1595,0,0,14,0
0.729,172.25.100.105,34.232.229.251,2517,76,267660,56,9,104,1254,1259,0.235294,0,13,0
...
```

9. As you can see from the output of RITA, a system on the ICS network – 172.25.100.220 – shows clear signs of beaconing to a system on the internet – 222.222.222.222. RITA discovered 594 connections, with an average of 862 bytes transferred per connection and an interval of 600 seconds (10 minutes). The confidence scoring RITA placed on this is 0.996 out of a perfect 1.

To conclude, RITA verified our earlier findings that there is beaconing traffic occurring between the internal machine with IP address 172.25.100.220 and an external IP address of 222.222.222.222. Time to go see what is up with this system.

To explore additional functionality or get more information on RITA, head on over to https://www.activecountermeasures.com/free-tools/rita/.

## Investigating/forensics of suspicious endpoints

At this point, we take our threat hunt investigation to the suspected victim machine. Let's quickly see what we can find out about 172.25.100.220 from Security Onion. Clicking on the IP address 172.25.100.220 in the Connections dashboard has Kibana open a new tab with the indicator dashboard loaded and the IP address as a search term. This dashboard allows us to see a variety of interesting facts about our suspected victim:

![](img/c8ffa9959721097701584fcc95101362_455_0.png)

Figure 11.18 – Security Onion – suspicious host – indicator dashboard

At a glance, we can see the types of logs (Dataset) that are present in the database for the suspect system, as well as summary widgets for Source IP, Destination IP, and Destination Port.

## Threat Hunt Scenario 1 – Malware Beaconing

In this case, the default indicator screen isn't showing too much useful information, but we can make things fit our needs (starting to see a pattern?). Simply add the **Security Onion – DNS - Queries** data table widget (like we added the network transport protocol widget earlier). This gives us a summary of all the DNS queries made by the suspect system. Now if we filter on `222.222.222.222` (the suspicious IP address we found), we get the following view:

![](img/c8ffa9959721097701584fcc95101362_456_0.png)

Figure 11.19 – Security Onion – suspicious host – custom indicator view

Notice how this ties the suspicious target IP address of `222.222.222.222` to the website `very-malicious-website.com`. We now have two solid indicators of compromise:

- **IOC-1 – IP address:** `222.222.222.222`
- **IOC-2 – URL:** `very-malicious-website.com`

We can use the discovered **Indicators Of Compromise (IOCs)** to find other potential victims and use them to create firewall and/or IDS rules to find suspicious network traffic.

Because the indicator dashboard clearly indicates HTTP usage, a final Kibana dashboard I would like to cover is the **HTTP** dashboard, found under **Home – Network – HTTP**:
