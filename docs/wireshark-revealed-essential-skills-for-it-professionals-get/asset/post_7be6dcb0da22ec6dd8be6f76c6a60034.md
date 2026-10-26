
![](img/da40f92928403866599198344271d63e_1381_0.png)

Figure 9.22: The Find utility in the Follow TCP stream dialog

## USBPcap

USBPcap has been there for a long time with Linux and Mac users, but for Windows, this is the first time that users will be able to sniff the activity over USB interfaces. So, let's quickly walk through this latest feature and try to understand how to work with it with the help of an example. Follow the given steps to replicate the scenario:

- 1. After the successful installation of Wireshark on your Windows machine, it is highly recommended that you restart your machine because USBPcap might give you some trouble.
- 2. After your PC has restarted, open Command Prompt and change your current directory to the USBPcap installation directory that should be located at C:\Program Files\USBPcap\.
- 3. Now, perform a directory listing using the dir command to check whether USBPcapCMD.exe is present in the directory. Refer to the following screenshot that represents this step:
- 4. Type USBPcapCMD.exe in the Command Prompt to launch the sniffing application.

```
C:\Program Files\USBPcap\
```

```
dir
```

```
USBPcapCMD.exe
```

![](img/da40f92928403866599198344271d63e_1382_0.png)

Figure 9.23: The USBPcap installation directory

```
USBPcapCMD.exe
```

- 5. As soon as it has been launched successfully, you will be asked to choose a root hub over which you want to sniff the traffic and the name of the trace file where you want to redirect the output. Refer to following screenshot that illustrates this:

![](img/da40f92928403866599198344271d63e_1383_0.png)

- 6. Now, as instructed, the application will initiate the sniffing process over root hub 1 and will dump any activity captured over the USB interfaces to the `abc.pcap` file.
- 7. Now, try to copy something from your PC to the USB drive or vice versa. You probably won't be able to see any live activity over the Command Prompt, but in the background, it is actually running.
- 8. Whenever you want to stop the sniffing process, you can press Ctrl + C.
- 9. Now, it's time to open the `abc.pcap` file using Wireshark to see what we have in the trace file. Refer to the following screenshot that illustrates this:

![](img/da40f92928403866599198344271d63e_1384_0.png)

Figure 9.24: The `abc.pcap` trace file

As you can see, we have an activity, which got captured; it all looks similar to what we saw with network packets. We have all the familiar columns that list out various details such as time, source, destination, and so on. So we were able to successfully dump the activity over available USB interfaces without any technical hassle and I hope you will do some research to get a better understanding about USBPcap.

## Summary

The newer version of Wireshark has adopted a new framework that gives us a new and totally amazing GUI. The older version was built upon the GTK framework, and since now we have the QT framework, from the perspective of a normal user, the differences are mostly concerned with its look and feel.

Scrolling is definitely one of the tools that we all have seen in all major applications, but hats off to the developers who came up with such a creative idea of showing the coloring pattern of your trace file inside the scroll bar while you are trying to look for something specific. It does give an extra advantage.

The Translation feature makes Wireshark more international and close to every user in terms of personalization. As many Wireshark users might not be comfortable with the English language, now they have the facility to change the language to their native language, which would make the analytical process for a professional more effective.

Graphs are one of the features using which differences between normal and abnormal conditions can be figured out, and are used very often. Now, creating and customizing graphs is easier than ever, and the look and feel has drastically improved as well.

The Follow TCP stream dialog is introduced with some of the new features that let you find an ASCII string, and it lets you move easily between the streams available too; you don't have to close and reopen the dialog to move to a different stream.

USBPcap has been there with us for quite a long time, and most Linux and Mac users are probably aware of this fact. The way your NIC card lets you listen over the wired/wireless channel is similar to the way the USBPcap option would let you listen over the USB ports that you have. This means that now, Wireshark can also trace the activities happening over a USB interface.

## Practice questions

- Q.1 Try to find out the major differences between the GTK and QT frameworks. And which one do you think is better?
- Q.2 Try out the Translation feature by changing the system default language in Wireshark to any other language of your choice.
- Q.3 Create a Flow graph using the newer version and the legacy version, and observe how many differences you can figure out between the graphs.
- Q.4 Open any previous capture file you have, and try to figure out how many TCP streams there are in it.
- Q.5 Figure out a way to remove the display filter button for the ARP protocol that we created earlier in this chapter.
- Q.6 Try changing coloring rules for ARP packets, and check whether you can observe the difference in the intelligent scroll bar area.
- Q.7 After installing the newer version of Wireshark on a Windows machine, try to launch USBPcap. Then, copy and paste from your PC to the USB device or vice versa (dump all the activities in the test.pcap file).
- Q.8 Open the recently captured test.pcap trace file for the USB interface activity in Wireshark, and try to figure out what the packets listed in the list pane state. Specifically, try to analyze the values shown in the source and destination columns.

## Bibliography

This course is a blend of different projects and texts all packaged up keeping your journey in mind. It includes the content from the following Packt products:

- Wireshark Essentials - James H Baxter
- Network Analysis using Wireshark Cookbook - Yoram Orzach
- Mastering Wireshark - Charit Mishra

## Index

### A

- abnormalities, TCP
  - examples / [Unusual traffic](Unusual traffic)
- access_denied / [How to do it...](How to do it...)
- ACK / [How it works...](How it works...), [Regular operation of the TCP Sequence/Acknowledge mechanism](Regular operation of the TCP Sequence/Acknowledge mechanism)
- acknowledgement number field / [How it works...](How it works...)
- ACK packets / [WEP-open key](WEP-open key)
- ACK scanning / [How to do it...](How to do it...)
- Active mode (ACTV) / [Analyzing FTP problems](Analyzing FTP problems)
- Address Resolution Protocol (ARP) / [Ethernet frames and switches](Ethernet frames and switches)
  - about / [Address Resolution Protocol](Address Resolution Protocol), [The layers in the TCP/IP model](The layers in the TCP/IP model)
  - poisoning / [ARP poisoning](ARP poisoning), [ARP poisoning](ARP poisoning)
- Address Resolution Protocol (ARP) filter / [Configuring Ethernet, ARP, host, and network filters](Configuring Ethernet, ARP, host, and network filters)
- advantages, Wireshark
  - user friendly / [Why use Wireshark?](Why use Wireshark?)
  - robustness / [Why use Wireshark?](Why use Wireshark?)
  - platform independent / [Why use Wireshark?](Why use Wireshark?)
  - filters / [Why use Wireshark?](Why use Wireshark?)
  - cost / [Why use Wireshark?](Why use Wireshark?)
  - support / [Why use Wireshark?](Why use Wireshark?)
- AirPcap Adapters
  - about / [AirPcap adapters](AirPcap adapters)
- Allow sub-dissector option / [There's more...](There's more...)
- Anycast addresses
  - about / [IPv6 address types](IPv6 address types)
- application-based issues
  - troubleshooting / [Troubleshooting application-based issues](Troubleshooting application-based issues)
- application-layer attacks
  - about / [How it works...](How it works...)
- application attacks
  - discovering / [Discovering brute-force and application attacks](Discovering brute-force and application attacks), [How to do it...](How to do it...), [There's more...](There's more...)
- application layer, OSI
