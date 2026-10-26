# Self-service and management

It is not cost-effective for the Cluster Operations team to hold every tenant user's hand for day-to-day operations. From a compliance perspective, it is not usually permitted for such cluster admins to be tightly embedded into a tenant's business. Besides, with cloud-based systems, there is every expectation that tenants can self-serve themselves for most requirements, without needing IT support. However, overall cluster security is the responsibility of the Cluster Operations team, and they need to be able to have assurances themselves that the tenants cannot breach beyond their instance scope.

Hence, it becomes a fine balance between maintaining rigid security constraints and the flexibility needed for tenants to manage their instances themselves every day. In this section, we will look at the capabilities that OpenShift and Cloud Pak for Data offer to help enterprises balance this need.

# Provisioning and upgrades – enabling a choice of services and versions

Different tenants need different sets of Cloud Pak for Data services to be deployed. They may even need to "pin" them to specific versions from a stability perspective – typically, production environments are maintained at well-tested levels, while there is more flexibility for development environments.

With the judicious use of operator custom resources, individual tenants can decide to install and upgrade each service by themselves. With services that support the concept of *service instances*, such as Db2, appropriately authorized tenant users can even spin up a database instance on their own, without the need for Kubernetes access.

Hence, cluster admins could grant tenant admins privileges in their namespace to empower them to provision Cloud Pak for Data services on their own. In turn, tenant admins can authorize a subset of tenant users to be able to provision service instances on their own within the confines of that Cloud Pak for Data instance.

# Monitoring

With tenant instance scoped monitoring and management, cluster admins can delegate day-to-day monitoring to individual tenant admins. Cluster admins still retain the rights to control the namespace as a whole, especially by establishing resource quotas, and tenant admins operate within those boundaries to decide how to monitor and throttle usage inside their tenancy.