# Cracking WPA2 with aircrack-ng

The aircrack-ng, in my opinion, is one of the greatest wireless penetration tools out there. It can crack both, WPA and WEP keys, by setting up a listening interface on the wireless interface, sniffing traffic and handshakes, and then trying and cracking these keys. There are many tools that come with aircrack-ng, including airbase-ng, aird ec ap-ng, airdecloak-ng, and aireplay-ng. I will use some of these tools for my wireless test.

I will use aircrack-ng in the lab to try and crack a wireless guest network that is using WPA to make sure that they are not using an easy-to-guess passphrase. There are a couple of steps involved; let me walk you through these now. I will assume that aircrack-ng is already installed:

+   1. We need to start up the wireless monitoring interface. To accomplish this, we need to use the airmon-ng command, followed by the wireless interface you plan you to use. In my case, it's wlan1.
+   
+   2. Next, we need to get a list of available WLANs that are being seen. This way, we can select the appropriate WLAN for packet and handshake capturing. To get this list, we can use the airodump-ng command:

```
root@pi-kali:~# airmon-ng start wlan1
Found 2 processes that could cause trouble.
If airodump-ng, aireplay-ng or airtun-ng stops working after a short period of time, you may want to run 'airmon-ng check kill'
PID
Name
199 NetworkManager
445 wpa_supplicant
PHY     Interface       Driver      Chipset
phy0    wlan0           ??????      Broadcom 43430
phy1    wlan1           rt2800usb   Ralink Technology, Corp. RT5372
(mac80211 monitor mode vif enabled for [phy1]wlan1 on
[phy1]wlan1mon)

(mac80211 station mode vif disabled for [phy1]wlan1)
```

![](bbox=[0.12, 0.7, 0.2, 0.78])

For any processes that may interfere, you may want to stop them before proceeding just to make sure there isn't any interference. You can use the kill PID command, where PID is the process id of that interfering process.

```
root@pi-kali:~# airodump-ng wlan1mon
```