It is also usually the case for a tenant admin to decide on matters of access management (and compute resource quotas) on their own, without needing IT support.

It is preferable for monitoring and diagnostics or other maintenance tasks to be delegated to the tenant admins as well.

Approaches to tenancy should thus be able to support granting some level of management privileges to designated "tenant administrators" within their scope.

In this section, we looked at some common requirements that are placed on multi-tenancy approaches. The next section will introduce how the Cloud Pak for Data technology stack helps address these aspects of tenancy.

# Isolation using namespaces

While it may be simpler to use different Kubernetes clusters or even dedicated host nodes for each tenant, it will not be cost-effective. Luckily, Kubernetes includes the concept of a namespace [34] that represents a virtual cluster to help sandbox different tenant workloads. OpenShift Container Platform extends namespaces with the concept of a project [35].

The most common approach is to have different installations of Cloud Pak for Data in different tenant instance namespaces. Each tenant is then assigned one such installation and works in relative privacy and isolation from one another. Cloud Pak for Data installations in each tenant namespace use their own OpenShift router, which allows for the customization per tenant of this access point, including external hostnames and URLs, as well as the ability to introduce different TLS certificates for each tenant.

Since persistent volume claims and network policies can be scoped to namespaces as well, isolation from storage and network access can also be guaranteed. Should a tenant user need access to Kubernetes (for example, to install or upgrade their services), that could be scoped to their namespace, too, with Kubernetes RBAC. OpenShift projects could also be mapped to dedicated nodes and this can provide for more physical separation, at an increased cost, to isolate Pods from various tenants from one another.

The chapter on multi-tenancy describes the various options of using namespaces to achieve isolation as well as options where two tenants may want to even share the same namespace.