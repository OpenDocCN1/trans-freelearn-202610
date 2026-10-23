As you can see, even modern automation controllers suffer from legacy vulnerabilities. The ControlLogix platform, or more precisely the FactoryTalk environment (FactoryTalk security), allows us to set up strict control over who can program or even view controller data. However, FT security is cumbersome to set up and is often not implemented.

Finally, I wish to point out that I am not singling out PLCs/protocols – most major PLC brands and automation protocols suffer from the same inherently vulnerable design flaws that we saw with Modbus and EtherNet/IP, and they are easy to manipulate/interrogate by unauthorized individuals as soon as those individuals are on the right network segment (our IDMZ should help secure things). The major manufacturers have started to implement additional controls to mitigate some of the effects of these vulnerabilities. Things are looking up, but it will take a long time, if ever, before we see those fundamental design flaws in ICS equipment be completely remediated.

To finish our industrial protocol scanning activities, we will close this section by extracting information from a Siemens PLC.

# ## Scanning Siemens S7 (iso-tsap)

Siemens S7 PLCs use the `iso-tsap` protocol to communicate to other PLCs and a programming Terminal. The protocol runs on a service that typically listens on port `102`. If you recall from our scan, we had one system with port `102` open. Let’s look at what system that was with the following command:

```
pac@KVM0101011:~$ cat ics-services.txt | grep 102 -B5

Nmap scan report for 172.25.100.20
Host is up (0.00055s latency).
Not shown: 65534 closed ports
PORT    STATE SERVICE VERSION
102/tcp open  iso-tsap Siemens S7 PLC
```

Now that we know `172.25.100.20` responds to queries on port `102`, let’s see what kind of information we can get from it. We are going to use another Nmap script, written to get information from S7 PLCs:

```
pac@KVM0101011:~$ ls /usr/share/nmap/scripts/ | grep -e s7

-rw-r--r-- 1 root 9.8K Jul 13 03:03 s7-info.nse
```