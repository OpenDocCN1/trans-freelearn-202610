+   - UpgradeVersionTag – In the event that you want to manually override a version of a Consul agent, perhaps when shifting configuration parameters across a cluster, this parameter defines what metadata would be used to indicate the version of the server. Without this value, Autopilot simply looks at the version being reported by the Consul software itself.

We aren't really going to be playing with any of these parameters, with the exception of RedundancyZoneTag. This will also help us understand the use of the metadata within the Consul agent configuration. So, let's look at a new configuration for our server nodes with the metadata and redundancy zone configured:

# Consul server configuration with redundancy zones

```
server = true
bootstrap_expect = 3
retry_join = ["provider=aws tag_key=Name tag_value=rjackson-consul-server-instance"]
bind_addr = "192.168.100.20"
advertise_addr = "3.17.154.23"
client_addr = "192.168.100.20"
ui = true
read_replica = false
node_meta {
    stooge = "moe"
}
autopilot {
    redundancy_zone_tag = "stooge"
}
```