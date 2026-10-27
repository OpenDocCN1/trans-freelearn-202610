OK—now that we've gone through the full Terraform file, let's take a closer look at that server template file. Via the template, we're creating a `consul-server.hcl` server configuration file and populating it with all of the configuration necessary, as follows:

```
```bash
#!/usr/bin/env bash

cat << EOF > /etc/consul/consul.d/consul-server.hcl
server = true
bootstrap_expect = 3
retry_join = ["provider=aws tag_key=Name tag_value=${server_name_tag}"]
bind_addr = "$(curl -s http://169.254.169.254/latest/meta-data/local-ipv4)"
advertise_addr = "$(curl -s http://169.254.169.254/latest/meta-data/public-ipv4)"
client_addr = "$(curl -s http://169.254.169.254/latest/meta-data/local-ipv4)"
ui = true
EOF

# Starting consul services
sudo systemctl start consul
```
```

Following along with that template file, let's take a look at exactly what that file is doing:

+   1. Indicating that the Consul agent should operate as a server.
+   2. Next, we're identifying the expected number of servers that we want to see in our cluster. Remember that we mentioned this before when discussing consensus and quorums. We're creating three Consul servers, which means our quorum is… (I'll give you a hint—it's more than one, and less than three).
+   3. OK—this part is pretty cool. If you remember, in the Terraform script, we had that `instance` tag that identifies the individual servers. This `retry_join` configuration identifies a list of servers that compose that expected peer set of three servers. AWS fills in this value with the servers that match that `instance` tag.