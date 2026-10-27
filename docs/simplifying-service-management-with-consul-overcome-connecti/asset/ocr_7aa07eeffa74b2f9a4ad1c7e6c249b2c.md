Recall that as we were discussing service discovery in *Chapter 5, Little Bo Peep Lost Her Service*, Consul is able to contain services within the registry that are not being managed by Consul. These are **external** services, which kind of makes sense, as they are **external** to Consul. Any services that are to be accessible via the terminating gateway must be registered in Consul; after all, if they aren't, how can we discover them? When we configure a terminating gateway, we need to list the services that are available through that gateway. This is the only way that Consul can manage the discovery and connection from within the mesh to those external services. We can still use ACLs and security up to the gateway node; however, once the traffic leaves the gateway, we need to resort to old-fashioned networking, the way our grandmothers did.

# Ingress gateways

Next, we have to think about all of those entities that are outside in the great big scary world that want to access the wonderful services that we offer! This requires a method to open the service mesh and invite others to join us. However, we just don't want to open the door and let anything into our mesh. We've worked very hard and have put up with a plethora of horrible jokes to secure our beautiful mesh. The last thing we want is to put the mesh at risk by allowing nefarious traffic through the door.

![](bbox=[0.118, 0.492, 0.854, 0.765])

Figure 6.13 – Ingress gateways