# Flow graphs

This is one of the nicest features in Wireshark, where we are assisted with troubleshooting capabilities in scenarios like facing a lot of dropped connections, lost frames, retransmission traffic, and more. Flow graphs let us create a column-based graph, which summarizes the flow of traffic between two endpoints, and it even lets us export the results in a simple text-based format. This is the easiest way of verifying the connection between client and server.

For instance, I have a web server running at 172.16.136.1 and a client running at 172.16.136.129. The client will request the web server for a certain resource. Let's see what the flow graph looks like for such kind of requests. There will be hundreds of packets generated, but we will look only at HTTP packets, just to make the results more confined and understandable. Click on Flow Graph under Statistics, and then from the pop-up dialog, choose Displayed Packet. Click on OK. Refer to the following screenshot that illustrates the same:

![](bbox=[0.073, 0.45, 0.926, 0.82])

Figure 3.13: Flowgraph