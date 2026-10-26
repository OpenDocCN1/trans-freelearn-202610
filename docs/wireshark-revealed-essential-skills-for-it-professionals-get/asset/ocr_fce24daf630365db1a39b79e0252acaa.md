two clients are connected to each other. The packets sent and received by the wireless clients are not relayed to the access point. The clients taking part in this communication now handle the process of sending beacons and processing authentication that a WAP handles in normal scenarios.

![](bbox=[0.12, 0.145, 0.925, 0.285])

+   - **Master mode**: When the NIC card in your machine lets you become an AP, this is what the master mode is all about. Higher-end devices have a capability to act like access points, and this is possible when NIC cards start working together with a special driver.

![](bbox=[0.12, 0.42, 0.925, 0.545])

+   - **Monitor mode**: For the purpose of this chapter, this mode is very important. This mode is used to listen to the packets that are flying around; when the monitor mode is activated, your device will stop transmitting and receiving any packets and it will just sit silently and sniff live traffic. If you want to capture packets from the wireless network concerning 802.11 protocols, then your NIC and the driver that is being used must support the monitor mode. It is quite easy to activate the monitor mode on an OS, such as Linux and MAC; however, with Windows, it becomes quite troublesome to activate the monitor mode. This mode is often termed as the **Radio Frequency Monitor Mode (RFMON)**.