As you can see, there are a lot of areas where we can apply ACLs to manage what humans, machines, or cyborgs can do to access our services. This also provides a plethora of areas to control the individual nodes.

When we are talking about security, we often use the term *least privilege*. Plainly speaking, or writing, this means that instead of giving a child a pile of candy and telling them what they can’t eat, we start with the child having no candy and providing only what they can eat. This is why, earlier in this section, we talked about instituting a default deny policy within Consul. If we utilize this as our standard security rule, especially with the certificates we discussed in the previous section, we can further tighten our control of the individual nodes. This drastically reduces the possibility of any cowans or eavesdroppers gaining access to, or listening in on, our cluster operations – for if you don’t have the secret handshake, you will not pass.

Now that we’ve addressed all of that, let’s take a look at what a rule looks like. We’re going to look at two rules here, both of which we’ll actually use when we start getting our hands on the system.

This first rule applies a minimal policy for a Consul client to perform its operations on itself and alert the Consul server of any changes to the infrastructure:

```
node "ip-192-168-100-11" {
  policy = "write"
}
service "consul" {
  policy = "write"
}
agent_prefix "ip" {
  policy = "write"
}
```

Within this policy, we have three main rules that appear to be quite simple, but within a least privileged model, their role in society is critically important.

The first rule within the policy utilizes the node rule type. Without this rule, the node can’t update the server cluster on what services it sees and, more importantly, the health and location of the node. If a client attempts to join the cluster, without the token associated with this rule, the server cluster won’t even recognize it as a client. Note that because the node rule only applies to ip-192-168-100-11, this rule only applies to the actions for this particular node, as that is its hostname. Any other node using the token associated with this policy would not be recognized by the Consul server cluster.