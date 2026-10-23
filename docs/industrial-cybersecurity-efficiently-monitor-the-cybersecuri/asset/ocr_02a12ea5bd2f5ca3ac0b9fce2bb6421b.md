+   - **Artifact 5 – Workstation12:**
+     - `fun-service`
+     - `c:\windows\system32\backdoor-v2.exe`

That covers comparing snapshots of startup applications. Next, we will look at how application errors can reveal malicious applications.

# Looking for application errors to find artifacts

One final location I want to point out that might indicate malicious executables running on a system is the **Windows Error Reporting** section. Malware code can be sloppy, buggy, and easy to crash, so looking through the log of application errors can reveal some interesting findings:

```
```text
[Windows Error Reporting]

Time        Type              Details
12/27/2020 11:23 PM  Application Error  Faulting application name: backdoor.exe, version: 0.0.0.0, time stamp: 0x4bc63c7d&x000d;
12/23/2020 11:45 PM  Application Error  Faulting application name: backdoor.exe, version: 0.0.0.0, time stamp: 0x4bc63c7d&x000d;
12/18/2020 8:24 PM   Application Error  Faulting application name: backdoor.exe, version: 0.0.0.0, time stamp: 0x4bc63c7d&x000d;
9/3/2020 7:04 PM       Application Error  Faulting application name: backdoor.exe, version: 0.0.0.0, time stamp: 0x4bc63c7d&x000d;
9/3/2020 6:59 PM       Application Error  Faulting application name: backdoor.exe, version: 0.0.0.0, time stamp: 0x4bc63c7d&x000d;
8/29/2020 9:45 PM      Application Error  Faulting application name: backdoor.exe, version: 0.0.0.0, time stamp: 0x4bc63c7d&x000d;
8/29/2020 8:04 PM      Application Error  Faulting application name: backdoor.exe, version: 0.0.0.0, time stamp: 0x4bc63c7d&x000d;
8/29/2020 7:15 PM      Application Error  Faulting application name: backdoor.exe, version: 0.0.0.0, time stamp: 0x4bc63c7d&x000d;
8/28/2020 10:19 PM     Application Error  Faulting application name: backdoor.exe, version: 0.0.0.0, time stamp: 0x4bc63c7d&x000d;
8/28/2020 8:48 PM      Application Error  Faulting application name: backdoor.exe, version: 0.0.0.0, time stamp: 0x4bc63c7d&x000d;
```
```

![](bbox=[0.117, 0.373, 0.856, 0.496])

That `backdoor.exe` executable is getting more and more suspicious the more we look into things.

I started this section with the sentence *if we had a known-good baseline copy of these snapshots…* This was stated on purpose; remember we are basing our activities around a scenario where we are adding a new plant to our ICS environment. We therefore do not have baseline snapshots and will need to look for other methods to discover malicious or unwanted code running on our assets. One such method involves looking at network artifacts, which is what we will discuss next.

# Looking for malicious network traffic to find artifacts

Unless we are lucky and catch the malicious or unwanted application's executable code being downloaded (or an update to the malicious executable), spotting malicious executables that might be present on endpoints will be limited to interaction traversing the network to and from those hosts. In the case of malicious executables, this could be a beacon out to a C&C server, a connection into the host from the attacker that controls the malware, or the malware scanning the network for other hosts to infect.