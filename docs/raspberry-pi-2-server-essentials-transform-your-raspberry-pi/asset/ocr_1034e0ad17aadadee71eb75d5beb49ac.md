# 5

# Setting Up the Raspberry Pi as a File Server

Windows, Linux, and Mac OS X all use different file systems, and they have different ways of sharing files across a network. On top of all this, you also get network-specific protocols, for example, the ones that are used over the Internet.

If you are wondering which the best one is, you will never find the answer; instead, you should use the technologies that you find most suitable for your application and with which you are most familiar.

# Connecting the external storage

The Raspberry Pi does not have a SATA interface, and the only way to connect this storage is by using USB ports. You can further expand the amount of USB ports by attaching a powered USB hub.

Due to the 480 MBps limitation on the USB bus, we should consider the most optimal setup. We reserve 100 MBps for Ethernet, which should be more than enough to stream any kind of HD video. This leaves us with 380 Mbps (47 MBps), which is fine because we can only send a maximum of 12 MBps of data via Ethernet anyway.

If you really want to squeeze out more transfer speed, you can use a compatible 802.11n or 802.11ac wireless USB peripheral. You may achieve in excess 100 MBps with WiFi, but there is the extra process of making sure your signal is good and that your general environment is not occupied by any other wireless router. The most optimistic transfer speed you can expect is about 25-30 MBps.