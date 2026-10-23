# Clustering, High Availability, and Disaster Recovery

These three topics are related, but each represents a different aspect of managing the risks in your system. Each is a complex topic in its own right, so we can give only an overview here.

# Clustering

**Clustering** is a technique for reducing the risk of downtime and for optimizing resource utilization through load balancing. Clustering comes in a few flavors, but the common principle is that multiple equivalent servers are provisioned to handle a given load. In the CM case, we are usually talking about multiple instances of a J2EE application server at the application layer and the CE server layer.

> The version of IBM WebSphere bundled with a CM license does not include the *Network Deployment* feature. In other words, it doesn't include clustering and the accompanying administrative features. It's great for most development, functional testing, and even some types of production deployments, but it doesn't include that particular feature.

The most important thing to know about clustering in CM is that the best type of clustering to use for the CE servers depends on the API transport being used:

+   - For EJB transport, you should use software load balancing built into the application server. The reason for this is that the EJB address resolution tends to be done very infrequently and ties connections to some specific host in the cluster. This usually negates any intended load balancing from a hardware load balancer.
+   - For CEWS transport, hardware load balancers work well. You would typically configure a virtual host name for the load balancer and let it pass connections to physical servers. All hardware load balancers understand how to properly balance HTTP and HTTPS traffic.

> All API interactions with the CE are stateless. In other words, except for load balancing, it doesn't matter which CE server is used for any particular API request. Requests are treated independently, and the CE does not maintain any session state on behalf of the application. On the other hand, some CM applications do need to be configured for **sticky sessions**. A sticky session means that incoming requests (usually from a web browser) must return to the same copy of the application for subsequent requests.