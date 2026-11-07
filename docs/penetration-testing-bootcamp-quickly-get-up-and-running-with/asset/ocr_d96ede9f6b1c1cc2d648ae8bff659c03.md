You can see that I have my hciX interface (hci0 in my case). You can also see that it has a MAC address. Now that we have verified that our Bluetooth interface is working, we can start to scan our area.

Here is the command and output that I ran in my lab. Since I have multiple Bluetooth interfaces on my device, I will be specifying the device I want to use. I will also specify the file to write my findings in as well as the logging options to write timestamps, device name, manufacturer, and class.

```
bluelog -i hci0 -o /root/bt-log.txt -vnmct
```

Here is the output from my lab:

```
root@pi-kali:~# bluelog -i hci0 -o /root/bt-log.txt -vnmct
Bluelog (v1.1.2) by MS3FGX-----------------------------------Initializing
device...OK
Opening output file: /root/bt-log.txt...OKWriting PID file:
/tmp/bluelog.pid...OK
Scan started at [02/22/17 01:53:30] on 00:1A:7D:DA:71:10.
```

Hit on Ctrl + C to end scan:

```
[02/22/17 01:56:02] 58:40:4E:50:D1:0E, JB's iPhone, 0x7a020c
[02/22/17 01:56:17] 8C:DE:52:1F:F5:07, SRS-BTM8, 0x240414
[02/22/17 01:58:05] C0:CE:CD:0F:D4:BA, Ryan's iPhone, 0x7a020c
```

Based on my findings, you can see that my area is a Bluetooth rich area. It's important that I go through this list, and if anything seems strange, to investigate more. But, as a penetration tester, your job is to run, observe, make note, and report on it.

I haven't seen this defined as a policy or requirement, but I would definitely recommend performing these checks on a regular basis. Similar checks via WiFi are required by organizations such as PCI. It doesn't hurt to include these checks as well for a stronger security profile.