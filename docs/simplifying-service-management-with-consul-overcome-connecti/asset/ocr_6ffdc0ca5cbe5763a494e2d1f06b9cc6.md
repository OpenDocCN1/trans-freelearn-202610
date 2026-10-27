Well, what if we don't want all of our services traversing through the gateway? No problem! When the services are connected through the mesh gateway, the Consul clusters recognize those services, and we can write intentions! This gives the Consul administrators the immense power of not only controlling the service mesh within a single cluster, but the entire world! Furthermore, as part of the service definition, we can tell Consul that the **upstream service** (the service we want to initiate the connection to) is available via the mesh gateway. Let's take a look at a modified `curl` service as an example.

# Curl.json across the mesh gateway

```
{
    "service": {
        "name": "curl",
        "tags": [
            "curl-client"
        ],
        "connect": {
            "sidecar_service": {
                "proxy": {
                    "upstreams": [{
                        "destination_name": "httpd",
                        "datacenter": "secondary",
                        "local_bind_port": 3579
                    }]
                }
            }
        }
    }
}
```

Within this slightly altered service definition, we are telling Consul that the `httpd` service that we want to reach is available in the data center named *secondary*. This is a great example of connecting to another Consul data center for access to a service, but what if the intended service isn't within the mesh?