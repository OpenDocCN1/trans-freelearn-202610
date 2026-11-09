# What is Windows Azure AppFabric?

Windows Azure AppFabric is part of Microsoft's Windows Azure cloud offering, which is a set of Microsoft-hosted capabilities for building solutions. AppFabric is a "platform as a service" (PaaS) solution that acts as middleware in the cloud. This middleware includes features for secure cloud messaging (Service Bus), unified access control across federated identity stores (Access Control), distributed application cache (Caching), and BizTalk-like integration services (Integration). The Caching and Integration offerings are expected to be released in the latter half of 2011. For the purposes of this chapter, the focus will be on the Service Bus and Access Control services.

# AppFabric Access Control

AppFabric Access Control services use "claim transformation" to create a federated security strategy for connected applications. In the sections below, we look at the problems we face when attempting to unify an identity model, and how the AppFabric Access Control service can help.

# Problem scenarios

"Who are you and what can you do?" This is the fundamental question of application security. Many of our applications decide to answer this question through locally stored credentials and roles but this leads to a messy, difficult-to-manage security landscape.

While many organizations standardize on an internal identity provider such as Microsoft's Active Directory, it can prove challenging to try and maintain a single identity provider when your organization is expected to interact with a wide array of external parties. One solution is to explicitly add external accounts into the internal Active Directory, but this requires significant maintenance and may give external users more access to internal applications than was intended. What we need is a way to allow each organization to use their own identity providers while enabling a clean way to transform those identities and credentials into something that our own systems can understand. Ideally, our systems should get out of the business of managing credentials and permissions and be able to trust a unified third party instead.