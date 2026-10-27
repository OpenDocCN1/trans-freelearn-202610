| Source Namespace | Source Name | Destination Namespace | Destination Name | Precedence |
|---|---|---|---|---:|
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

![](bbox=[0.116, 0.56, 0.858, 0.797])

Figure 6.9 – Intentions in the UI