![](bbox=[0.07, 0.06, 0.93, 0.4])

Figure 3.8: Endpoints window

Now, if you want to analyze other protocols, then simply click on any tab of your choice. I clicked on the IPv4 tab and sorted the main pane using the Packets column, which looks like the one shown in the following screenshot:

By just looking at the Endpoints dialog, I can now easily figure out that maximum data was transferred from IP 172.20.10.7. This could be a one single IP talking to some server or probably a server talking to multiple machines on our network at a moderate rate.

| Address | Packets | Bytes | Tx Packets | Tx Bytes | Rx Packets | Rx Bytes |
|---|---|---|---|---|---|---|
| 4a:74:6e:ba:d0:64 | 1 717 | 771 601 | 850 | 638 274 | 867 | 133 327 |
| Apple_b9:53:ec | 1 690 | 770 617 | 870 | 133 603 | 820 | 637 014 |
| Broadcast | 33 | 1 536 | 0 | 0 | 33 | 1 536 |