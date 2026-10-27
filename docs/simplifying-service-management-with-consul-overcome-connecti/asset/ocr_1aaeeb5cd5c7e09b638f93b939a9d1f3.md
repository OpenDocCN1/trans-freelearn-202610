Here, we have two new configuration blocks, one for node_meta and another for autopilot. Not a lot has been entered into these blocks, but this is all that is necessary to set up the redundancy zones. First, I defined some sort of metadata for each node. I'm calling the key for that metadata stooge, and the value is either moe, larry, or curly, indicating how I'm grouping the servers. It might be good for your metadata to indicate the actual zone in accordance with your own network structure, but I wanted to use the stooge parameter for fun and to show that these keys can really be just about anything. We can see the metadata configured for each node within the UI, as shown in the following figure:

![](bbox=[0.13, 0.29, 0.86, 0.51])

Figure 4.4 – Consul agent metadata

You can also filter the output of the CLI based on the metadata:

```
# consul catalog nodes -node-meta=stooge=moe
```

| Node | ID | Address | DC |
| ip-192-168-100-20 | 8a1da666 | 3.17.154.23 | dc1 |
| ip-192-168-100-74 | d80b81de | 3.135.205.26 | dc1 |

Note that in these configurations, the read replica flag is set to false for all servers. When we designate a server as a read replica, we are essentially telling Consul that we will manage the existence of read replicas and we don't need Autopilot to manage that for us. When we utilize redundancy zones, Autopilot ensures that there is at least one voting member in each redundancy zone. Any additional servers that join the particular zone will automatically be recognized as read replicas. To see this in action, I want to provide a new diagram with the appropriate server names and the associated metadata from my test system: