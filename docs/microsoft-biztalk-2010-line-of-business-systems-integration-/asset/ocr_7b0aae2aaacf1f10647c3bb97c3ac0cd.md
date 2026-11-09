+   6. With the messaging ports complete, we can complete our orchestration binding. Go to our orchestration in the Chapter6 application and match the Outbound Logical Ports to the appropriate Send Ports / Send Port Groups.

![](bbox=[0.295, 0.213, 0.705, 0.345])

+   7. Start the send ports and orchestration.

We can test the entire end-to-end solution by starting our client application and submitting a new building access request. Select building 10 for the first request and notice that the output message has no training requirements for the user. If you send another request and use building 41, you should see a message with two courses assigned to the user.

# Summary

As more and more organizations begin to rely on the cloud for mission critical applications, the need for straightforward integration solutions will increase. In this chapter, we first saw how we can send messages from the cloud to our on-premises BizTalk Server with minimal effort. The majority of work we performed was around building a reusable WCF behavior that can validate tokens and overlay an authorization layer across our services. Prior to the availability of Windows Azure AppFabric, this sort of integration would have only been possible by either exposing BizTalk Server to the Internet through a reverse proxy, or, by using hardware/software to allow partners to directly access an internal service bus. Now, with minimal effort, we selectively exposed individual endpoints to any authorized, Internet-accessible client.

Besides just sending to the cloud, we also need the ability to consume cloud services. We saw that it was very easy to consume a cloud endpoint in the same way that BizTalk consumes on-premises services. One can imagine the exciting possibilities of orchestrating messaging solutions that combine internal and external data in a seamless manner.

In the following chapter, we'll see how to build integrated SharePoint solutions using the Windows SharePoint Services Adapter and InfoPath.