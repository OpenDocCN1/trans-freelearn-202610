Every service that Consul is aware of is defined by, well, a service definition. Now, some might say, wait, you said this stuff was automatically discovered – why do I need to configure it? The claim is that these services are automatically discovered, not automatically defined. The definition comes in the form of a file, either JSON or HCL, that Consul is able to read along with other configuration files. These files can be quite elaborate, or incredibly simple, depending on the service and its function. Let’s look at a service definition that has already been employed in our work thus far:

# httpd.json

```
{
    "service": {
        "name": "httpd",
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

You might notice that this is one file, called httpd.json, with the definition of the httpd service that is deployed in Consul. When the Consul agent starts, it attempts to read every file within the configuration folder. These files could be actual agent configurations, security configurations (such as Access Control Lists (ACLs)), or service definitions. This affords any organization the ability for the service developers to write, or at least significantly contribute to, their service definition and manage it uniquely.

If you don’t want an individual file for every service, Consul does provide the ability to combine multiple services into one configuration file. However, care should be taken to not allow those files to become overloaded. After all, can you imagine having 100 services running on a Consul agent with one configuration file?