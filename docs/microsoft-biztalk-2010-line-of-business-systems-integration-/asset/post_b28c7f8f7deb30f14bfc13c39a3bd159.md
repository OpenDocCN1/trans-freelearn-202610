
## 6 BizTalk Integration with Windows Azure AppFabric Service Bus

In today's business environment, organizations need the ability to quickly and securely integrate with an ever-changing set of business partners. Successfully exposing local services to other departments or entire organizations can require a Herculean effort of networking and security. How can you securely expose services that are buried deep behind firewalls and network routers? What measures can we take to avoid a brittle security model that falls apart when partners are added or a situation changes? It is possible to build this type of distributed application today, but it requires a significant investment in infrastructure and multiple concessions by all parties involved.

In this chapter we will cover:
- What is included in Microsoft's cloud computing integration platform named Windows Azure AppFabric
- The problems that AppFabric Access Control services and the AppFabric Service Bus are intended to solve
- How to leverage your on-premises BizTalk Server to securely consume Windows Azure AppFabric messages and publish to the Windows Azure AppFabric cloud environment

## What is Windows Azure AppFabric?

Windows Azure AppFabric is part of Microsoft's Windows Azure cloud offering, which is a set of Microsoft-hosted capabilities for building solutions. AppFabric is a "platform as a service" (PaaS) solution that acts as middleware in the cloud. This middleware includes features for secure cloud messaging (Service Bus), unified access control across federated identity stores (Access Control), distributed application cache (Caching), and BizTalk-like integration services (Integration). The Caching and Integration offerings are expected to be released in the latter half of 2011. For the purposes of this chapter, the focus will be on the Service Bus and Access Control services.

## AppFabric Access Control

AppFabric Access Control services use "claim transformation" to create a federated security strategy for connected applications. In the sections below, we look at the problems we face when attempting to unify an identity model, and how the AppFabric Access Control service can help.

### Problem scenarios

"Who are you and what can you do?" This is the fundamental question of application security. Many of our applications decide to answer this question through locally stored credentials and roles but this leads to a messy, difficult-to-manage security landscape.

While many organizations standardize on an internal identity provider such as Microsoft's Active Directory, it can prove challenging to try and maintain a single identity provider when your organization is expected to interact with a wide array of external parties. One solution is to explicitly add external accounts into the internal Active Directory, but this requires significant maintenance and may give external users more access to internal applications than was intended. What we need is a way to allow each organization to use their own identity providers while enabling a clean way to transform those identities and credentials into something that our own systems can understand. Ideally, our systems should get out of the business of managing credentials and permissions and be able to trust a unified third party instead.

### The solution

The AppFabric Access Control (AC) service provides a cloud-based engine for transforming claims from one trusted identity provider to another. The AC service has a trust relationship with specified issuers, whether that is an enterprise directory like the Active Directory of our partner, or a web identity provider like Google, Facebook, or Windows Live ID.

Those providers send a set of "claims" to the AC service. A **claim** is a set of information about a user's identity. A security token may contain claims with your name, email address, job title, and date of hire. If the AC service trusts the issuer who provided those claims, then those values can be considered trustworthy by consumers of those claims.

![](img/23d1402649b6528b085cd6b900a2c9c3_275_0.png)

For example, my application could require a user to be authenticated and also require a particular user role (for example, "Manager") to execute an operation. This application could be built to accept claims as its identity input. One can imagine that building applications to accept these issuer-specific claims from every issuer would be a mess to build and a nightmare to maintain. However, this is where the power of claims transformation becomes apparent. My application requires a specific claim and the AC service performs transformation from all the individual issuers to my single output claim type. In this way, our application code is completely decoupled from a specific identity provider and instead can authorize a whole host of parties by transforming their claims to the single format that is expected. The AppFabric AC acts as a "claim broker" as it receives claims from multiple sources and transforms them to the output format we need.

This is a powerful concept and one that will be fully demonstrated in the coming pages.

## Service Bus

The AppFabric Service Bus allows us to expose web services to the cloud in a secure fashion and without unnecessarily opening network ports or changing any infrastructure considerations.

### Problem scenarios

Connecting two distributed parties is by no means an easy feat. We've been solving this problem for years through the use of Electronic Data Exchange (EDI) and Value Added Networks (VAN), company-to-company VPNs, FTP, and a series of other mechanisms. In each case, the communication required specialized software or significant changes to each organization's infrastructure landscape. To make this happen, firewall ports must be opened, user accounts get created in the target organization's directory, certificates may get exchanged, and so forth. In addition, when an organization opens up a VPN for a partner, they may be unknowingly enabling access to a whole host of applications that the partner is not meant to see.

### The solution

The AppFabric Service Bus introduces a novel way to selectively and securely expose on-premises WCF endpoints to the cloud.

Let us look at two of the major components of the Service Bus. First, we have a global addressing service that allows us to own a namespace (for example, "Contoso") and define URIs underneath it. This global namespace can front a distributed set of services that are geographically distributed and hosted in different environments and containers. The URIs built in the cloud namespace act as a central reference point for the internal services that sit behind it.

A second, and primary, component of the Service Bus is the Relay Service, which includes the set of WCF bindings that make it possible for any WCF service to listen on the cloud namespace address. Even non-WCF services (for example, ASMX services, non-.NET clients) are capable of receiving messages through the Relay Service. Service clients call the cloud endpoint and the request is relayed down to the internally-hosted service.

The Service Bus leverages some familiar base bindings as well as introducing some new ones. For instance, many standard WCF bindings (**BasicHttpBinding**, **WebHttpBinding**, **Ws2007HttpBinding**, **NetTcpBinding**) all have equivalent Relay bindings (**BasicHttpRelayBinding**, **WebHttpRelayBinding**, **Ws2007HttpRelayBinding**, **NetTcpRelayBinding**). In addition to those Relay bindings, there are two additional bindings that are unique to the Service Bus. The **NetOneWayRelayBinding** performs a one-way communication over TCP from the cloud to the client. The **NetEventRelayBinding** is a broadcast binding that is also one way, but allows multiple services to listen on the cloud endpoint. You might use this binding if you want a client service request to be sent to multiple services.

## Publishing Service Bus messages to BizTalk Server

BizTalk Server 2010 is a straightforward recipient of Azure Service Bus messages. For the foreseeable future, organizations will continue to maintain significant on-premises software investments while looking to the cloud for expansion of their application portfolio. Architects will have to pay particular attention to integration between on-premises software and cloud platforms. Extending BizTalk Server through the Azure Service Bus opens up a host of possibilities and helps leverage the scalability and accessibility of the cloud while exploiting prior software investments.

### Scenario description

The remainder of this chapter walks through a complete solution scenario that demonstrates how to send messages through the Service Bus into BizTalk Server and back from BizTalk Server to the Service Bus.

In this scenario, we represent the fictitious company named **WatsonBSafe**. This company manages building access security for organizations that subscribe to their service. They own the technology that scans employee badges and allows entry into both buildings and rooms within a company's office buildings. WatsonBSafe has many clients who each publish their employee registries to them as well as explicit requests for access rights to be granted to client employees.
