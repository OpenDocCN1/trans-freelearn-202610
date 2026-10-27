So far, we've only been working in our default data center, imaginatively named dc1. For this example, I'm going to be creating two data centers, Jake and Elwood. The data center for the particular Consul agent is defined within the configuration with a simple datacenter parameter. Take this, for example:

```
server = true
datacenter = "Elwood"
bootstrap_expect = 3
```

I've set up a system with three server nodes in each of the two data centers, Jake and Elwood. Joining the two data centers is pretty simple within the configuration as well as via the command line. Within the configuration, we utilize a retry_join_wan command, similar to the retry_join command we already use for the Consul nodes to discover each other. Additionally, you only require a connection between one server in each data center, as they then learn about each other. However, I would strongly recommend including multiple servers in the event that one is not available when the two data centers need to join up:

![](bbox=[0.12, 0.48, 0.87, 0.65])

Figure 4.14 – Jake and Elwood getting the band together

Once the data centers have been joined, you can get a view of all of the servers using the consul members -wan command:

| Node | Address | Status | Type | Build | Protocol | DC | Segment |
| ip-192-168-100-140.elwood | 3.133.92.242:8302 | alive | server | 1.9.4+ent | 2 | elwood | <all> |
| ip-192-168-100-180.elwood | 18.216.77.187:8302 | alive | server | 1.9.4+ent | 2 | elwood | <all> |
| ip-192-168-100-203.jake | 18.188.194.17:8302 | alive | server | 1.9.4+ent | 2 | jake | <all> |
| ip-192-168-100-60.jake | 3.140.201.212:8302 | alive | server | 1.9.4+ent | 2 | jake | <all> |
| ip-192-168-100-81.elwood | 3.140.252.128:8302 | alive | server | 1.9.4+ent | 2 | elwood | <all> |
| ip-192-168-100-97.jake | 3.135.63.108:8302 | alive | server | 1.9.4+ent | 2 | jake | <all> |

Figure 4.15 – Clusters banded together