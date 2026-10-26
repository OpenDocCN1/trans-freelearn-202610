# Architecting for multi-tenancy

In this section, we will explore what it means to implement multi-tenancy with Cloud Pak for Data, starting with how to plan out the topology on an OpenShift cluster and how to separate the tenants within the same cluster.

# Achieving tenancy with namespace scoping

As we established earlier, assigning a unique OpenShift project (a Kubernetes namespace) to each unique tenant is the recommended approach for Cloud Pak for Data multi-tenant deployments.

The following diagram shows how multiple tenants can be sandboxed into individual OpenShift projects and the logical separation between the Operations zone (which would be managed by the cluster administrators) and the Tenants zone (somewhere the tenant users are potentially given access to):

![](bbox=[0.12, 0.45, 0.88, 0.78])

Figure 12.1 – Individual tenant namespaces in a shared Kubernetes cluster