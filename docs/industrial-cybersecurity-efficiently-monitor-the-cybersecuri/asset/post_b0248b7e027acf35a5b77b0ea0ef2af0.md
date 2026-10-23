
From the CrowdStrike site, it is defined as follows:

> CrowdResponse is a modular Windows console application designed
> 
> to aid in the gathering of host information for incident response
> 
> engagements. The tool was originally written to support our CrowdStrike
> 
> Services team during their incident response engagements. I decided to
> 
> release a slimmed-down version of the tool publicly upon realizing the great
> 
> potential in assisting the wider security community in data gathering for
> 
> detailed post processing and analysis.

CrowdResponse has a variety of functionality implemented through modules and is well worth checking out. For this exercise, we will be using the YARA functionality only. The way we are going to leverage this functionality is through Python scripts that use `psexec.exe` to run CrowdResponse on all our domain computers, gather the data, and create a very convenient report detailing any findings.

So, let's get started. Follow these instructions to get the `yara_scanner` Python script installed and running:

- 1. We will be running this from the domain controller of OT-DOMAIN, OT-DC1. Install Python 3 on that server.
- 2. The Python tool in question can be downloaded from `https://github.com/tsale/yara_scanner`. Download and extract it to a temporary directory (workdir).
- 3. Download `psexec.exe` from `https://docs.microsoft.com/en-us/sysinternals/downloads/psexec` and extract in the same directory as the `yara_scanner` script.
- 4. Create the `c:\CrowdResponse` folder, download the CrowdResponse files from `https://www.crowdstrike.com/resources/community-tools/crowdresponse/`, and unpack them in the `c:\CrowdResponse` folder.
- 5. Create a `c:\CrowdResponse\results` subfolder.
- 6. We need to make some minor changes to the `yara_scanner.py` script. They are at the top of the file, highlighted in the output as follows:

```
import subprocess
import argparse
import time

crowdresponseEXE = "C:\\CrowdResponse\\CrowdResponse.exe"
# Specify the directory of the CrowdResponse executable.
```

```
**Make sure that you have rights to the folder.
crowdresponseDIR = "C:\\CrowdResponse"
targets = []
...
print("[+] Running CrowdResponse on remote hosts\n")
for x in targets:
    task = subprocess.run(f"""psexec.exe \\\\{x} -d cmd /c "c:\\temp\\crowdresponse.exe @Yara -t {directory} -v -h -s -y C:\\Temp\\ > C:\\temp\\{x}_yara.xml""",capture_output=True).stdout.decode('utf-8')    # Run psexec command and output results on xml file.
...
```

7. Create a text file with all the computer IP addresses we want the yara_scanner script to target, called workdir\targets.txt:

```
172.25.100.212
172.25.100.100
172.25.100.201
172.25.100.210
172.25.100.220
172.25.100.203
172.25.100.105
172.25.100.110
172.25.100.202
```

8. We can now run the scan:

```
pac@KVM0010110 : ~/workdir/
python.exe ./yara_scanner.py -f ./targets.txt -d c:\ -m 1 -y C:\workdir\yara-rules\
```

The arguments used to run the yara_scanner script are as follows:

- -f: Specifies the file that contains the target hosts/IPs
- -m: Specifies the pulling frequency (in minutes. Default = 5 mins)
- -d: Specifies the directory to scan on the remote host (need to specify the full path)
- -y: Specifies the directory of the YARA rules (on local host) to use against the remote machines (need to specify the full path)

Running the Python script will deploy the **CrowdResponse** tool to all the machines in `targets.txt` and send the command to run the scan. Then it will set a timer and check back in with the machines to see whether the scan is complete. While the scan is running, we get the following feedback:

```
C:\workdir> python.exe \.\yara_scanner.py -f .\targets.txt -d
C:\ -m 1 -y C:\workdir\yara-rules\
[*] Checking if all targets are alive
    - Removing 172.25.100.220 as it appears offline
    - Removing 172.25.100.203 as it appears offline
[+] Copying CrowdResponse on the remote hosts
[+] Running CrowdResponse on remote hosts

    - 172.25.100.212 is still running
    - 172.25.100.100 is still running
    - 172.25.100.201 is still running
    - 172.25.100.210 is still running
    - 172.25.100.105 is still running
    - 172.25.100.110 is still running
    - 172.25.100.202 is still running
1 minute(s) passed

    - 172.25.100.212 is still running
    - 172.25.100.100 is still running
    - 172.25.100.201 is still running
    - 172.25.100.210 is still running
    - 172.25.100.105 is still running
    - 172.25.100.110 is still running
    - 172.25.100.202 is still running
2 minute(s) passed
...
    - 172.25.100.201 is still running
15 minute(s) passed
```

```
> + Remote host 172.25.100.201 is done!
> All done!
> PS C:\yara_scanner-master>
```

Now that the scan is done, we can see the results by opening the C:\CrowdResponse\results\CrowdResponse_yara.html file:

## Produced by the free [CrowdStrike](link) tool CrowdResponse

**Module: yara**

| system | yarafIle | pid file | identifier | result |
| :------- | :----------------------- | :------------------------------- | :--------------------- | :------ |
| 172.25.100.210_yara | C:\Temp\backdoor.yar | c:\windows\winsservice.exe | backdoorExecutable | TRUE |
| 172.25.100.212_yara | C:\Temp\backdoor.yar | c:\windows\System32\backdoor.exe | backdoorExecutable | TRUE |
| 172.25.100.212_yara | C:\Temp\backdoor.yar | c:\windows\Temp\backdoor.exe | backdoorExecutable | TRUE |

Figure 12.15 – CrowdResponse report

As you can see, the scan found another copy of the backdoor.exe file on 172.25.100.210 (Workstation10)!

This covers our threat hunt for malicious or unwanted applications.

## Summary

In this chapter, we went through the process of proving or disproving the hypothesis that there are malicious or unwanted applications running on assets in the industrial environment. We saw how by combining data, tools, techniques, and activities, we not only managed to prove our hypothesis but also found compromised systems in our environment. We even discovered the use of resource- and bandwidth-hogging software (uTorrent). This chapter covered the second of three hypotheses that, when performed successfully, form an effective strategy to help clean up or verify the environment of a new or long-neglected part of the industrial network.

In the next chapter, we are going to perform threat hunting activities around the third hypothesis, There are suspicious external connections going into the industrial environment.

## 13
Threat Hunt
Scenario 3 –
Suspicious External
Connections

In this chapter, we are going to perform the third and final threat hunting exercise in the series of the three threat hunting scenarios that, as we discussed in Chapter 10, Threat Hunting, help assess the cybersecurity hygiene of a new or additional Industrial Control System (ICS) network (segment). By defining the following three threat hunting hypotheses, There are malicious actors (trying) to beacon out to external IPs/domains, There are malicious or unwanted applications running on assets in the industrial environment, and There are suspicious external connections going into the industrial environment, and performing threat hunting exercises around those hypotheses, we are trying to uncover as much malicious activity as we can when faced with adopting or integrating a new ICS network or the addition/expansion of an existing ICS network. The idea is that by performing these three threat hunting exercises beforehand on an unknown ICS network, we can be reasonably certain we’ll find any unwelcome guests before we attach the new network (segment) to our existing infrastructure.

## Forming the suspicious external connections threat hunting hypothesis

As we discussed in *Chapter 10, Threat Hunting*, threat hunting exercises are geared around hypotheses. Typically, hypotheses follow or reflect a discovered security incident or some form of an alert from an automated security monitoring system or a finding from a security analyst. The threat hunting exercise we will be performing in this chapter works a bit differently. The environment we are going to perform threat hunting activities on is unfamiliar or new to us. Our company purchased a new production plant with an existing ICS network infrastructure they are looking to tie to the existing company infrastructure, and we want to make sure that it is safe to do so. Another scenario would be when you have run your ICS network for ages but never seriously looked at the state of its security posture. In this case, these “unprovoked” types of threat hunting exercises can quickly and accurately give you an idea of malicious activity or content in your ICS environment.

In *Chapter 11, Threat Hunt Scenario 1 – Malware Beaconing*, we looked at suspicious network connections leaving the ICS environment (egress connections). We discussed that an egress connection from an industrial network should be rare because of the (intended) isolated nature of industrial systems. This holds true for ingress connections (legitimate connections into the ICS network) as well. Connections from the enterprise network should be kept at a minimum and well defined (controlled and monitored) and connections from the internet should be prevented altogether.

In general, looking at connections into or out of the ICS environment is an excellent way to pick up malicious activities. As we saw when we were hunting for beacons back in *Chapter 11, Threat Hunt Scenario 1 – Malware Beaconing*, looking at a pattern in traffic revealed an infected workstation reaching out to its **Command and Control (C&C or C2)** server.

We will be applying a similar technique to find suspicious ingress connections. By banking on the convenient fact that there should not be many connections into the industrial network from external networks, we are going to see if we can prove or disprove the hypothesis **There are suspicious external connections going into the industrial environment** by searching for some common indicators that set apart the malicious connections from legitimate ones.

Let's start with a discussion on (ingress) connections.
