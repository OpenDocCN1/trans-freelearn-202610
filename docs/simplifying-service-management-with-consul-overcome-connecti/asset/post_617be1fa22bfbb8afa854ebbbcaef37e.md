
If you recall, there are two methods in which Consul will run as a default for ACLs. We can either implicitly deny all requests and only specify the access that is permitted, or we can implicitly allow all requests and specify which connections we want to deny. Following along with the zero-trust model, critical to any security posturing, the implicit deny should be in place for any production environment. Consul intentions follow the same model, where we can either implicitly accept all network connections or implicitly deny all network connections. Whatever policy you have configured for your ACLs, the same policy applies to your intentions. For example, if your ACL policy is deny all, then by default any allowed service connections need to be explicitly defined. As we've already shown traffic flowing through the mesh, we can see that we don't have a deny all ACL policy in place.

So, in order to play with intentions, we'll be using that same curl to http server connection that is already established. Utilizing the CLI, let's take a look at the services that are registered within the Consul catalog:

```
$ consul catalog services
consul
curl
curl-sidecar-proxy
httpd
httpd-sidecar-proxy
```

Note that we actually have services listed for each of the sidecar proxy servers. However, our intention is to manage the actual services, curl and httpd. If we want to prevent communication between curl and httpd using the command line, it is quite simple:

```
$ consul intention create -deny curl httpd
```

The CLI responds, acknowledging the creation of the intention:

```
Created: curl => httpd (deny)
```

With this in place, let's go ahead and try issuing that curl command again:

```
$ curl http://localhost:3579
curl: (52) Empty reply from server
```

As you see, we are no longer able to connect: `Empty reply from server.`
I get the same reaction when I ask if a menu item is vegan…empty reply from server.
If you observe the logs from the `httpd` server when this happens, we get a very interesting message:

```text
[ERROR] proxy.connect: authz call denied: service=httpd
reason="Matched L4 intention: default/curl => default/httpd
(ID: a605dfa9-cdcc-b304-400d-3fe708b603c1, Precedence: 9,
Action: DENY)"
```

From the result, we can plainly see that an attempt was made from `default/curl` to `default/httpd` and it was denied. The *default* value that we see here relates to the namespace that the services, and the intention, applies to. This allows unique intentions to be configured and assigned for individual namespaces within the Consul system. Furthermore, we already know that with the proper ACL tokens, we can ensure that only those that are authorized can create intentions in their own namespaces. We also have a precedence value returned. With any rules in life, there is going to be an order of precedence. There may also be wildcards in place to generalize some of those rules and help with the management. For example, we may want to make sure that certain services on our system *only* receive connections, and never transmit. For example, our `httpd` service should never be initiating connections to any other service. For this purpose, we can wildcard an intention that instructs Consul to deny any request originating from the `httpd` service:

```shell
$ consul intention create -deny httpd '*'
```

If you aren't implicitly denying all connections within your production environment, this type of rule can be incredibly useful to manage any potential breach of an application. For example, if our `httpd` service was compromised and used to send information to any other service within our mesh, this intention would explicitly prevent such an occurrence from happening. It should be noted as well that wildcard intentions are very low in the precedence list, as shown in *Figure 6.8*. The more precise the intention, the higher the precedence that intention will have in the evaluation.

| Source Namespace | Source Name | Destination Namespace | Destination Name | Precedence |
| --- | --- | --- | --- | ---: |
| Exact | Exact | Exact | Exact | 9 |
| Exact | * | Exact | Exact | 8 |
| * | * | Exact | Exact | 7 |
| Exact | Exact | Exact | * | 6 |
| Exact | * | Exact | * | 5 |
| * | * | Exact | * | 4 |
| Exact | Exact | * | * | 3 |
| Exact | * | * | * | 2 |
| * | * | * | * | 1 |

Figure 6.8 – Consul intentions order of precedence

Within our Consul system, we have now created two intentions: one to prevent `curl` from communicating with the `httpd` service, and another to prevent any outward communication from that `httpd` service to any other service. Within the Consul UI, all of the configured intentions are visible, and can even be sorted by order of precedence! As the number of services, and therefore intentions, grows, this view can be incredibly useful.

![](img/52479dafaefba00517c48a137e037b7e_185_0.png)

Figure 6.9 – Intentions in the UI

Managing intentions within the UI is quite simple either by creating a new intention or editing an existing intention. Every service within the Consul mesh is provided as a source and destination, and we can either choose to allow, deny, or create an **Application Aware** rule:

![](img/52479dafaefba00517c48a137e037b7e_186_0.png)

*Figure 6.10 – Editing intentions within the UI*

The **Application Aware** rules add complexity through the use of explicit permissions. Recall that layer 7 intentions provide additional functionality to define intentions based on the path of the URL or even the headers in the request.

Up until now, every scenario that we've been discussing has been based on the event that every application and service exists within the service mesh. What can we do about services that reside outside of the mesh? Well, I'm glad you asked, as that question provides an excellent gateway to the next topic.

## Gateways to domination

Wow, such wonderful and entertaining information on the service mesh! But I have a problem: not all of my services are eligible for the mesh. To quote the Finnish-American architect Eero Saarinen, *have you tried gateway?*

To think that every service, and every application, will reside within a single service mesh is quite unrealistic for any enterprise deployment. For this reason, Consul offers the concept of a **gateway**. Nearly every gateway in existence, be they spiritual, philosophical, or physical, connects two realms of existence. Looking at Consul, we can view each cluster as an individual realm, within which services are discovered and can safely communicate with each other. However, we need to consider the following scenarios as well:

- **Multiple data centers** – We know we can connect multiple Consul clusters within different data centers. However, when we leave the mesh, we lose the security of identity and encryption.
- **External services** – Not every service that we need will exist within the service mesh. As we’ve seen, the mesh requires a proxy server co-resident with the relevant application. Many services, such as hosted databases, don’t offer an embedded mesh compliant proxy server (at least, not yet).
- **External clients and applications** – Especially if we are offering services to end consumers, the mesh needs to accept traffic from external entities. There must be a way for external entities to enter the mesh to access the services they are entitled to.

Each of these three scenarios requires a Consul agent dedicated to the function of gateway operation. Additionally, the Envoy proxy must be paired with that agent to provide that tunnel-like connection. As we saw in the previous section, Consul utilizes the Envoy proxy server to manage the connections among services within the mesh. This functionality wouldn’t be possible without the tight coupling between Consul and Envoy, a coupling that is required for the mesh gateways. Therefore, Envoy at this point is the only proxy server supported for this purpose. Let’s take a look at how Consul applies various gateways in these scenarios.
