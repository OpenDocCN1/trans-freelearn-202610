However, by running the traceroute or ping commands, you can give them a jump start. So, let’s take a slightly deeper look into these commands and what information they reveal.

# Ping

This is a widely used command to check whether there is connectivity with the host, which in our case is the Salesforce server. The command sends four packets to the server and if the server responds with four packets, then that means connectivity exists and there is no packet loss. If the server doesn’t respond, then that could mean that there is an issue with the server. Let’s look at an example to understand this better. If we ping login.salesforce.com, we can see in Figure 6.18 that the server at this location responded and there was no packet loss:

![](bbox=[0.17, 0.35, 0.82, 0.62])

Figure 6.18 – Server response of a ping operation

As in Figure 6.18, the server at login.salesforce.com responded, and therefore, there is connectivity between our devices and the server.

If the same number of packets were not received as were sent, or the packet loss amounts to more than 10% of the total packets that should have been received, we would know that packet loss has occurred and that further investigation is required. The **Round-Trip Time (RTT)** is the cumulative time it takes for a data packet to travel to the destination and then return to the sender.