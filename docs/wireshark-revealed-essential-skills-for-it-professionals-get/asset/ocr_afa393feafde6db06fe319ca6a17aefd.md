screenshot for the normal traffic pattern shown in terms of an IO graph:

![](bbox=[0.073, 0.096, 0.927, 0.486])

Figure 8.22: Normal traffic in an IO graph

In the preceding graph, no major deviation can be observed; hence, we can include such a traffic pattern while creating a baseline for our network. Just the ICMP packets are sent from the client to the server without much trouble.

Next, I want you to see and observe the difference between the traffic pattern that we saw and the one below the IO graph, which was captured for the same network infrastructure. However, there was one more application that was involved in the replication of the event, which generated unnecessary traffic. This resulted in network clogging, which is popularly known as a bottleneck.

The application I used is the network traffic generator that can be used to deviate a normal traffic pattern. This results in a network bottleneck scenario and can even result in a denial of service. Refer to the following screenshot for reference: