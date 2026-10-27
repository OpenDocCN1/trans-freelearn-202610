
If we want to register a service with a specific namespace (not the default namespace), all we need to do is specify the desired namespace within the configuration of the service itself. In previous sections, we observed an httpd service that Consul was managing. I've modified that JSON file slightly, just to indicate that this service will be running within the gus namespace.

## httpd.json service definition

```
{
    "service": {
        "name": "httpd",
        "namespace": "gus",
        "tags": [
            "python-server"
        ],
        "port": 8080,
        "check": {
            "args": [
                "curl",
                "localhost:8080"
            ],
            "interval": "10s"
        }
    }
}
```

When we have our services configured and designated to a particular namespace, as we've done here with the httpd service, then only members of that namespace can interact and obtain the information associated with that service. To follow on from my example, if I query the services that are associated with my Consul system, I only end up with one service:

```
$ consul catalog services
consul
```

Oh no! Isn't my `httpd` service running? I can assure you that the service is running, only in a separate and distinct namespace:

```bash
$ consul catalog services -namespace gus
httpd
```

Now, being able to separate these services can help greatly; however, if we aren't securing those namespaces uniquely, then we really aren't getting the true benefit of these subdivisions. If those nerves are acting up from the memories of our ACL work, you need to trust your intuition. When we start creating namespaces within our Consul system, it's kind of like developing a management hierarchy.

If you recall, when we were playing with ACLs in order to bootstrap the system, we needed to create a bootstrap ACL token that gave us full administrative privileges over the entire Consul system. Using a grossly oversimplified analogy, we can view this administrator, or administrative group, as the governor of the Consul system. We'll call him **Governor Lepetomane**. Based on his credentials, represented by a unique token, he is able to set the rules for his jurisdiction that all of the towns and communities within must abide by. However, each town still maintains some level of autonomy. **Mayor Johnson**, with his own credentials (token), is able to create his own rules and regulations for the town of Rock Ridge, so long as those rules comply with those set forth by Governor Lepetomane. The management of the Consul namespace structure is nearly identical.

When we bootstrap the ACL functionality within Consul, we create a token that abides by the default global management policy. Using this token, we are able to create new policies, and corresponding tokens, to enable very fine-grained management of everything Consul can do. When we create our namespaces, we need to give those operators their own management policies, but we can't have them exercising their authority over neighboring namespaces. Therefore, each namespace gets its own *operator*-level token that the administrators of those namespaces can use to create their own policies, rules, services, and so on. So how are these two default policies different?

> **Important Note**
>
> Namespaces allow us to subdivide services and policy; however, Consul cluster nodes are visible to all namespaces.

Let's start by creating a *namespace-management-level* policy for Shawn's namespace:

```
consul acl token create -namespace shawn -description "Shawn's Management Token" -policy-name "namespace-management"
```

Now we have a default operational-level token assigned to the shawn namespace. Utilizing the Consul CLI, we can observe the existence of both policies. Note that in order to see the *namespace-management* policy, I need to include the flag for the appropriate namespace:

```
$ consul acl policy list
global-management:
  ID:             00000000-0000-0000-0000-000000000001
  Namespace:      default
  Description:    Builtin Policy that grants unlimited access
  Datacenters:

$ consul acl policy list -namespace=shawn
namespace-management:
  ID:             2226f072-68ae-9082-6d8d-7c9bdb1a987c
  Namespace:      shawn
  Description:    Default policy that grants all available privileges within a single namespace
  Datacenters:
```

Utilizing the consul acl policy read command, I'm able to look at the details of those two policies. Again, for the shawn namespace, I need to include the namespace flag in the command. Figure 4.12 provides a side-by-side view of the policy differences:

| global-management | namespace-management |
|---|---|
| <pre>ID: 00000000-0000-0000-0000-000000000001&#10;Name: global-management&#10;Namespace: default&#10;Description: Builtin Policy that grants unlimited access&#10;Datacenters:&#10;Rules:</pre> | <pre>ID: 2226f072-68ae-9082-6d8d-7c9bdb1a987c&#10;Name: namespace-management&#10;Namespace: shawn&#10;Description: Default policy that grants all available privileges within a single namespace&#10;Datacenters:</pre> |
| acl = "write" | acl = "write" |
| <pre>agent_prefix "" {&#10;  policy = "write"&#10;}&#10;event_prefix "" {&#10;  policy = "write"&#10;}</pre> | |
| <pre>key_prefix "" {&#10;  policy = "write"&#10;}</pre> | <pre>key_prefix "" {&#10;  policy = "write"&#10;}</pre> |
| keyring = "write" | |
| <pre>node_prefix "" {&#10;  policy = "write"&#10;}</pre> | <pre>node_prefix "" {&#10;  # node policy is restricted to read within a namespace&#10;  policy = "read"&#10;}</pre> |
| <pre>operator = "write"&#10;query_prefix "" {&#10;  policy = "write"&#10;}</pre> | |
| <pre>service_prefix "" {&#10;  policy = "write"&#10;  intentions = "write"&#10;}&#10;session_prefix "" {&#10;  policy = "write"&#10;}</pre> | <pre>service_prefix "" {&#10;  policy = "write"&#10;  intentions = "write"&#10;}&#10;session_prefix "" {&#10;  policy = "write"&#10;}</pre> |
| <pre>namespace_prefix "" {&#10;  acl = "write"&#10;  key_prefix "" {&#10;    policy = "write"&#10;  }&#10;  node_prefix "" {&#10;    policy = "read"&#10;  }&#10;  session_prefix "" {&#10;    policy = "write"&#10;  }&#10;  service_prefix "" {&#10;    policy = "write"&#10;    intentions = "write"&#10;  }&#10;}</pre> | |

Figure 4.12 – Bootstrap and namespace policies

Notice that there is an entire section of the bootstrap policy that is directly related to namespace management. Therefore, those with this very privileged policy are able to manage ACLs, keys, nodes, sessions, and services within each namespace. In other words, our governor has the ability to manage and control items at the town level, but mayors within the town have no authority over other towns. Additionally, although at the bootstrap level we have the ability to write to nodes within the cluster, at a namespace level, as the namespace has access to all nodes, it can only read.

As we've seen, namespaces are able to segment our entire Consul system into individual mini-systems, providing autonomy to the operators of those systems. However, in some highly secure environments, it may be required to actually divide Consul at the network layer, not just the application layer.

Our entire Consul cluster, every node in every namespace, communicates utilizing that gossip protocol we talked about so very long ago. We're not going to delve too deep into this topic, as it is quite advanced. However, consider the situation where we might have thousands of Consul nodes distributed throughout our data center network, a situation not uncommon in production environments. If you remember, when we started the discussion about namespaces with the tree example (Figure 4.10), the separation occurred one layer above the Consul nodes. When we segment the Consul network, we are actually dividing up those gossip pools into smaller pools, each communicating on their own network ports.

![](img/52479dafaefba00517c48a137e037b7e_138_0.png)

I've tried to illustrate this segmentation in Figure 4.13, using different types of pools. In this example, all network segments, or pools, are able to communicate with the core centralized server cluster. However, due to firewall or other network-based divisions, the pools are not able to communicate with each other. By utilizing Consul network segmentation, we can still manage the overall Consul cluster, services, and namespaces within the divided network.

Now that we've successfully conquered an entire data center with Consul services, it's time to continue with the Brain's goal of taking over the world. So come along Pinky, let's start discussing how to connect our Consul clusters.

## Connecting clusters

To think that all of our services are going to reside within a single data center might have worked 20 years ago when I started writing this book (so it seems). The world has truly evolved to a continuously connected mobile environment, although some might argue that this was actually devolution. Regardless, the only way to really maintain the communications among our services in this world is to ensure that their availability and location can be discovered from multiple geographic areas. Whether I'm in Boston, Berlin, or Melbourne, the applications and services I've grown to rely on must be available! All that we have done within this chapter has led us to this point!

Throughout the book, we've been talking a lot about gossiping and pools, and we're going to continue that discussion. However, as you can imagine, the amount of data that we have communicated within a single Consul cluster can be quite extensive, especially as we onboard all of our services. If we wanted, we could simply start deploying Consul nodes into different data centers and regions and be done with that. Unfortunately, this would cause a great deal of traffic to be broadcast across the entire network. Furthermore, spreading Consul server nodes across data centers can impact the availability of your server cluster, even with the brilliant Autopilot.

When you join Consul clusters across a Wide Area Network (WAN), the clusters don't share catalog information. Each server cluster only becomes aware of the other, so when a request for a service is made, that request can be transmitted to another data center to check for service availability.
