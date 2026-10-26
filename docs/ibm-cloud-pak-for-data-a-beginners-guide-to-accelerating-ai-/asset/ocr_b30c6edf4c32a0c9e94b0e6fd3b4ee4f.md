# Securing network access

In the previous sections, we established how the front door and unique routes help guarantee isolation for accessing individual tenant Cloud Pak for Data instances from *outside* the cluster. Kubernetes includes the concept of network policies, a concept very similar to firewall rules, that further strengthen the security of a tenant's instance from intrusive access from *within* the cluster.

Network policies can be authored to permit access between microservices within the same namespace and to deny, by default, access from other namespaces. Additional *allow* policies are defined, as an exception to the *deny all* rule, to permit HTTPS access to each tenant's front door and between the operators' namespace and each tenant's namespace. The *deny* policy blocks traffic between tenants, except for their front doors. Interactions between tenants, *if at all* desired, would need to be done via the authorization checks in their respective front doors, much like an external client accessing from outside the cluster.

Hence, while some access still needs to be permitted between the Operator and tenant namespaces, by carefully crafting network policies, tenants can ensure they are secure from intrusions either from outside or within the Kubernetes cluster.

# Auditing

There always needs to be checks and balances with regards to how software services are operated in such multi-tenant architectures. From a regulatory compliance perspective, auditing is one powerful tool that enterprises use to provide assurances of safe and monitored operations. In *Chapter 10, Security and Compliance*, we discussed the many mechanisms that the cluster operations team can employ to ensure security and compliance.

Tenants typically need to adhere to compliance or monitor the usage of their instance of Cloud Pak for Data from a security perspective. Such tenants should not have access to auditing or monitoring records across the whole cluster, just those scoped to their instance.

Cloud Pak for Data includes an instance scoped auditing service, as described in *Chapter 10, Security and Compliance*. Each tenant admin or security focal, if properly authorized, can configure just their instance of Cloud Pak for Data to forward all relevant audit events to **security information and event management (SIEM)** systems. Even if the same SIEM is used by all tenants and by the cluster operations team, the ability to identify each tenant uniquely can be maintained, and tenant-specific auditing reports can be published.