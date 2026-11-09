25. Ensure that the cloud endpoint is online, and if so, click the **Get Discount** button to have Salesforce.com call our on-premise service.

![](bbox=[0.35, 0.19, 0.65, 0.36])

In this exercise, we saw that the Azure AppFabric Service Bus is for more than just WCF clients. We can securely and quickly leverage on-premises resources from a SaaS environment without unnecessarily duplicating data or changing network infrastructure.

# Summary

In our first example, we saw how to consume the strongly-typed Salesforce.com WSDL within a BizTalk solution. In our second set of examples, we looked at strategies for sending requests from Salesforce.com to on-premises services. Both demonstrations leveraged Windows Azure AppFabric to relay messages from the cloud back to an internal service. The first example, which used Salesforce.com Outbound Messaging, used a message-embedded token to authenticate the caller. In the second example, we used Salesforce.com APEX code to acquire an AppFabric Access Control token and authenticate our client to the Service Bus. The request message was then relayed to a proxy service that then sent a message into BizTalk Server.

To be sure, we could have communicated between Salesforce.com and our on-premises systems through publicly exposed services or proxies. If your organization has a mature extranet strategy with reverse-proxy communication to internal services, then you do not need to leverage Microsoft cloud integration services. However, Windows Azure AppFabric gave us a powerful way to expose our internal services in a simple and secure way and without the need for additional hardware or software.