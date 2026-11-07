Here is the output from my lab:

```
root@pi-kali:/var/log# shred -vfzu auth.log
shred: auth.log: pass 1/4 (random)...
shred: auth.log: pass 2/4 (random)...
shred: auth.log: pass 3/4 (random)...
shred: auth.log: pass 4/4 (000000)...
shred: auth.log: removing
shred: auth.log: renamed to 00000000
shred: 00000000: renamed to 0000000
shred: 0000000: renamed to 000000
shred: 000000: renamed to 00000
shred: 00000: renamed to 0000
shred: 0000: renamed to 000
shred: 000: renamed to 00
shred: 00: renamed to 0
shred: auth.log: removed
```

Shred is such an effective product that many use it before getting rid of their hard drives, whether for selling them to others or throwing them away, just to make sure they are unreadable.

# CLI tips for hiding your tracks

Whether you are short on time, or whether you just want to do some basic hiding of your tracks, you can use some CLI tips to get the job done as well. When logging into my Linux host, I can see that the .bash_history file keeps track of what I have done. If I look at my file now, I can see my last couple of commands:

```
root@pi-kali:~# more .bash_history bluesnarfer -r 1-100 -C 13 -b
58:40:4E:50:D1:0E
apt-get install bluepot apt-get install bluemaho
hcitool -i hci0 rssi 8C:DE:52:1F:F5:07
sudo rfcomm connect 0 8C:DE:52:1F:F5:07 10 >/dev/null&
```

I can just use the Null redirect (>) to clear out this file. This works for any file you may want to clear out:

```
root@pi-kali:~# > .bash_history
root@pi-kali:~# more .bash_history
root@pi-kali:~#
```