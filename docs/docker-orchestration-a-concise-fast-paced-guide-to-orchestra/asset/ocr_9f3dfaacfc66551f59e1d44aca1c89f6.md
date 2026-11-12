```
},
"container": {
"docker": {
"image": "mysql:5.7",
"forcePullImage": true,
"network": "HOST"
}
}
}
```

# Scaling applications

Marathon makes it very easy to scale up or down the number of tasks for an application. Not only does this allow one to scale out to meet increased load, it also allows for blue-green deployments, which will be covered a little later in the chapter.

Assume that there is an application named nginx running with one instance. All that needs to be done to increase that to two instances is to run dcos marathon app update:

```
$ dcos marathon app update nginx instances=2
Created deployment 8dbf3d16-44d3-4478-9366-9743658dcfcd
```

This example sets the number of instances to two. Applications can similarly be scaled up or down. Marathon will attempt to start or stop as many instances as needed to meet the new requirement.

Make sure that your application can support running multiple instances. Databases, in particular, do not react well to other processes changing their files.

# Checking application health

Marathon, very similar to Kubernetes, supports the option of setting one or more health checks on a service to ensure it is running properly before making it available to handle requests. There are options to run a custom check command, check for TCP connectivity, or check the HTTP status for a specific path.