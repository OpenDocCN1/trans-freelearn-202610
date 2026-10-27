
In the output, you'll see that the agent is running, but you'll also see some errors:

```
consul.service - Consul server agent
    Loaded: loaded (/etc/systemd/system/consul.service; disabled; vendor preset: enabled)
    Active: active (running) since Sat 2021-04-03 23:37:57 UTC; 3min 57s ago
    Process: 2990 ExecStartPre=/bin/chown -R consul:consul /var/run/consul (code=exited, status=0/SUCCESS)
    Process: 2979 ExecStartPre=/bin/mkdir -p /var/run/consul (code=exited, status=0/SUCCESS)
    Main PID: 2991 (consul)
    Tasks: 7 (limit: 2347)
    CGroup: /system.slice/consul.service
            └─2991 /usr/local/bin/consul agent -config-dir=/etc/consul/consul.d/ -data-dir=/var/lib/consul/ -pid-file=/var/run/consul/consu

Apr 03 23:39:04 ip-192-168-100-77 consul[2991]:
2021-04-03T23:39:04.935Z [DEBUG] agent.auto_config: making AutoEncrypt.Sign RPC: addr=
Apr 03 23:39:04 ip-192-168-100-77 consul[2991]:
2021-04-03T23:39:04.943Z [ERROR] agent.auto_config: AutoEncrypt.Sign RPC failed: addr=
Apr 03 23:39:04 ip-192-168-100-77 consul[2991]:
2021-04-03T23:39:04.944Z [ERROR] agent.auto_config: No servers successfully responded
```

Oh! The RPC failed, and no servers successfully responded! Why do you think this happened? If you guessed ACL, then you're really starting to learn from this book. I guess I'm not that horrible at English after all!

Now that we've ostracized our client node, we need to help it get back into its family of servers. You can do this with all five clients if you like, but at this point, working with one client will be sufficient. As you may recall, part of that provisioning process included moving a client_acl.hcl file to the clients, similar to what we did for the servers. Let's move that client file into the proper directory:

```
$ sudo mv /tmp/client_acl.hcl /etc/consul/consul.d/
```

If we look at that file, we'll see that it is nearly identical to the server file that we put in place. There is only one thing missing – a suitable token:

```
acl = {
    enabled = true
    default_policy = "deny"
    enable_token_persistence = true
    tokens {
        "default" = ""
    }
}
```

So, before we restart that client node again, why don't we create a new policy and token for that node? This one is going to be a bit more specific. To create these items, we need to go back to the Consul server that we were using previously. If your bootstrap token is no longer in the environment, you'll need that again. You can check this by looking at the environment variables:

```
$ env | grep CONSUL
```

The output should contain the Consul address of the server that you are working on, as well as its HTTP token:

```
CONSUL_HTTP_TOKEN=34e87e9d-2207-f2eb-7efc-d41674eca6f0
CONSUL_HTTP_ADDR=http://192.168.100.146:8500
```

If you have both these values, and they are correct, then we can continue.

We're going to create a policy specifically for this node. The hostname for the client node I'm working with is ip-192-168-100-77. The policy rule I'm going to create is going to enable node- and agent-level write operations, but only for that node. Let's start by creating that policy file, similar to what we did previously for the servers:

```
$ cat > client-policy.hcl << EOF
node "ip-192-168-100-77" {
    policy = "write"
}
agent "ip-192-168-100-77" {
    policy = "write"
}
EOF
```

The file produced should look similar to this, but where I have my node, `ip-192-168-100-77`, you should have the hostname of your client node. Now, we have to create the policy with this file:

```
$ consul acl policy create -name "client-token" -description "Client ip-192-168-100-77 Token Policy" -rules @client-policy.hcl
```

The output of this command should include all the policy information:

```
ID:                a1fb3a7c-2a42-613d-4586-5dadfb422022
Name:              client-token
Description:       Client ip-192-168-100-77 Token Policy
Datacenters:
Rules:
node "ip-192-168-100-77" {
    policy = "write"
}
agent "ip-192-168-100-77" {
    policy = "write"
}
```

Just as we did previously, we must now create a new token for that policy. To create this token, we're going to execute the following command:

```
$ consul acl token create -description "ip-192-168-100-77 Token" -policy-name "client-token"
```

The output of that command is as follows:

```
AccessorID:          b0b8cee4-8efd-62d2-e22c-fc84a0dc3c92
SecretID:            776f0c07-6352-8d2f-8fc7-23f94cbafd74
Description:         ip-192-168-100-77 Token
Local:               false
Create Time:         2021-04-04 00:02:08.863705384 +0000 UTC
Policies:
    a1fb3a7c-2a42-613d-4586-5dadfb422022 - client-token
```

We now have a new Secret ID value that we can place within client_acl.hcl. Wouldn't this be so much easier if we used Terraform and Vault?

Go back to your tab that's logged in to the client that you are working with and edit the client_acl.hcl file to insert your new token:

```
$ sudo vim /etc/consul/consul.d/client_acl.hcl
```

This is what the file should look like – with your token of course, not mine!

```
acl = {
    enabled = true
    default_policy = "deny"
    enable_token_persistence = true
    tokens {
        "default" = "776f0c07-6352-8d2f-8fc7-23f94cbafd74"
    }
}
```

OK; we have our configuration and our token. Are you ready to restart that Consul service now? I know I am!

```
$ sudo systemctl restart consul
```

You can either check that status again on the client node or if you are more excited, go back to your server node and run another consul members. Do you see the node you've been working on? I certainly hope so!

As you may recall, all those nodes are running both the Consul service as well as an HTTPD service that was deployed when we set up the system. Let's check to make sure those services are registered on that node. We can do this via the command line, on the server node, using the following command:

```
$ consul catalog services -node=ip-192-168-100-77
```

> Note
> If you run a command like this and end up with an ACL error, you might not be on the server node that has your bootstrap token in the environment variable.

This is very specific to our node, as well as to the two services that are running on our client node – just as things should be.

Let's update our client-token policy with the new rule:

```
$ consul acl policy update -name "client-token" -rules=@client-policy.hcl
```

The output should confirm that your new service rules have been written to the policy:

```
ID:           a1fb3a7c-2a42-613d-4586-5dadfb422022
Name:         client-token
Description:  Client ip-192-168-100-77 Token Policy
Datacenters:
Rules:
node "ip-192-168-100-77" {
    policy = "write"
}
agent "ip-192-168-100-77" {
    policy = "write"
}
service "httpd" {
    policy = "write"
}
```

Great! We've updated our policy on our server node, and we never had to return to the client. Here's the final test; does your httpd service now show up in your catalog for that node? Run the following command to find out:

```
$ consul catalog services -node=ip-192-168-100-77
```

You should see the httpd service listed in the output. If you do, stand up and scream to the world, I have mastered Consul Access Control Lists! Well, maybe mastered is a bit of an exaggeration, but you should be very proud of what you've been able to accomplish here!

## Summary

Wow – another long chapter! I'm not sure about you, but I am seriously wiped out! Although this is a *getting started* book, the concept of secure communication using TLS certificates is not a light topic for the family dinner. As Consul is such a critical service to the network, though, with the application and service information it maintains, the necessity to authenticate your nodes and encrypt their communication paths can't be understated. Furthermore, Consul ACLs provide such a level of flexibility that they can be very overwhelming. However, their power and control should be a pinnacle component in any production environment. I would strongly encourage you to continue playing with the other client nodes, create some new policies and tokens for them, and see what you can manage. If you're worried about killing something, don't be. You can always use Terraform to destroy and rebuild the infrastructure. On that topic, don't forget to destroy what you've built today... we don't want to end up with an AWS bill at the end of the month to struggle with! Once you've destroyed your infrastructure, kick your feet up and have a beverage. The next chapter will be a bit lighter (I promise). There, we'll be discussing how to extend Consul across multiple data centers and connecting clusters. We're almost halfway there!
