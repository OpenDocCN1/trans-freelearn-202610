# # Introducing multitenancy using namespaces

In this section, we will introduce the concept of multitenancy in Amazon QuickSight. Based on what we have learned so far, a user that owns a QuickSight asset can share the resource with any other registered user in the account. However, there are use cases where a single QuickSight account needs to support multiple **tenants**.

# ## Understanding multitenancy

QuickSight offers features that allow developers to build multi-tenant **business intelligence (BI)** platforms. Due to its cloud-native architecture, QuickSight scales automatically depending on the number of users. This feature, coupled with QuickSight’s embedding capabilities and its API actions that can be accessed programmatically, helps developers to implement multi-tenant BI platforms.

A key characteristic of a multi-tenant platform is that tenants don’t interact with each other, which effectively prevents a tenant from granting (or revoking) access to users belonging to another tenant. It is also common that different tenants can have different permission requirements for their users and different _look and feel_ requirements. Multi- tenant platforms can serve large corporations, where a tenant can be each subsidiary or a separate department within the company.

Another use case can be software vendors who offer BI capabilities to their customers. In this example, each tenant of the platform would represent a customer of the software vendor. Each tenant would be able to manage their own BI assets and share them with their own users (but not with other tenants). For this scenario, the tenants would typically access QuickSight through a web portal. To separate and organize users from different tenants, QuickSight introduced the concept of _namespaces_.

In the next section, we will introduce QuickSight namespaces.