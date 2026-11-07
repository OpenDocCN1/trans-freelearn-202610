In the preceding code, the `-c` specifies the channel, `-w` is the WLAN Name, and `-bssid` is the BSSID of the WLAN that we are going to capture traffic on. And finally, we finish the command off with the monitoring interface we set up. While this runs, it will let you know which clients are connected and traffic that it has captured. You can also hope to catch a handshake if you are lucky.

![](bbox=[0.13, 0.2, 0.2, 0.27])

If you don't capture any handshakes, don't worry. You can use the `aireplay-ng` command (`aireplay-ng -0 0 -a <BSSID>`) monitoring interface to force a client to de-authenticate and then re-authenticate, which means you will see a handshake.

Here is a screenshot from my lab capture using airodump-ng. Please note that it does say that I obtained a handshake. I can also see that I have two clients connected:

![](bbox=[0.18, 0.35, 0.8, 0.48])

+   4. Once we are happy with our wireless capture, we can hit on Ctrl + C to stop it. Now, it is time to take that capture and mix it with a good word list using the `aircrack-ng` command to try and crack that passphrase. To accomplish this in my lab, I ran the following command:

```
root@pi-kali:~#aircrack-ng -w /usr/share/wordlists/wiilist.txt SP-Guest-01.cap
```