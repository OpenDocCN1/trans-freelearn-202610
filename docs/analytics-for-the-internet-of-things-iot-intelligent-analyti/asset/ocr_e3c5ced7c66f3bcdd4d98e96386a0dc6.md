These devices form a **Personal Area Network (PAN)**. Network interaction times can be very fast. A device can join a network in 30 ms and change from sleeping to active in 15 ms.

There are three types of ZigBee devices in a network:

+   - A **coordinator device** that initiates network formation. There is one and only of these for each network. It may also act as a router once the network is formed. It serves as the trust center and is the repository for security keys.
+   - A **router device** is optional. It can join with a coordinator or another router device and acts as a multi-hop intermediary in routing of messages.
+   - An **end device** has just enough functionality to communicate with its coordinator. It is optional and cannot participate in the routing of messages.

![](bbox=[0.201, 0.352, 0.801, 0.836])