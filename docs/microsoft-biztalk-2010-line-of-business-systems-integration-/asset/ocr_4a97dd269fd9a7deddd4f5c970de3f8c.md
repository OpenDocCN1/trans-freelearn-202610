While not a fool-proof security mechanism, this last step showed us that if we sent the wrong value, or omitted the node entirely, that the service call would fail and BizTalk would never process the message. Overall, Outbound Messaging is a powerful feature that makes Salesforce.com very event-driven and reduces the need to do inefficient polling operations.

# Querying BizTalk Server from Salesforce.com

Outbound Messaging is great, but there can certainly be cases where we want to send more than just after-the-fact, asynchronous requests to on-premises systems. What if we have sensitive or complex data and queries embedded in local systems but need access to them in the cloud? We could choose to replicate that data and logic, but there could be a both logistical and legal reason why that is suboptimal.

Instead, we can expose our on-premises systems to Salesforce.com through the Azure AppFabric Service Bus. To do this, we need to do low-level Apex programming that exposes access to the raw HTTP request and response payloads. In our previous exercise, we injected a long token into outbound messages and verified that token in BizTalk Server. We did this because we couldn't directly authenticate to the Service Bus. However, this isn't a problem when we do Apex programming. However, a new challenge arises: REST-based services that listen on the Service Bus expect the Access Control token in the HTTP header. This makes it easy to build and attach the token to Service Bus calls. Unfortunately, SOAP-based services that listen on the Service Bus expect a complex token in the SOAP headers and it is much more difficult to create the proper token. For this chapter's solution, we will build a REST service proxy in front of a BizTalk-generated SOAP service. This way, we can officially authenticate to the Service Bus before sending a request/ reply message to BizTalk Server.

The last part of this exercise requires us to build a Visualforce page that calls the Service Bus. Our completed solution reflects an end-to-end scenario where the Salesforce.com user needs on-premises data and BizTalk Server fulfils that request.

# Building the cloud-enabled REST service proxy

For this scenario, we already have a running BizTalk process that calculates discounts that we give our customers. This is proprietary knowledge that we do not want to replicate in the cloud. In this first step, we expose this BizTalk orchestration as a web service and put a cloud-ready REST web service in front of it:

+   1. Deploy the existing BizTalk project named `Chapter10-SFDC.AccountProcessing.ContractCalc.`