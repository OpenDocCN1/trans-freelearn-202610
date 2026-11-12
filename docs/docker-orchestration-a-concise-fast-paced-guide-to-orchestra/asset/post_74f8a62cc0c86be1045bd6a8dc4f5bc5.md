
```
"maximumOverCapacity": 1
}
}
```

The HAPROXY_GROUP label tells Marathon that this service will use the external HAProxy group, which is served by the marathon-lb service.

The other important settings are in the portMappings list. This example defines a single port to make available through the load balancer. The containerPort is the port that the container is listening on. hostPort is the port on the host that the container will listen on. If it is missing or set to 0, the container is assigned a random port. A hostPort is required, even if set to 0, to make the container's service available outside the cluster.

Finally, servicePort is the port on the load balancer that will service this application. In this case, external clients will connect to the load balancer on port 10001 to access this application. In practice, an external load balancer, such as an ELB on AWS, will be used to provide mapping from a standard port to the one configured in the load balancer.

By default, Marathon-LB is configured to use service ports 10000 through 10100, 80, and 443. Any service that will use this load balancer must select an available port from these for servicePort. Make sure that the ports do not overlap.

It is possible to run multiple load balancers. However, the load balancers run in the host mode on the nodes, which means that it is only possible to run as many load balancers as there are worker nodes.

Running an additional load balancer is straightforward. First, create a file called options.json that contains the following:

```
{
"marathon-lb": {
	"name": "marathon-lb-internal",
	"haproxy-group": "internal",
	"bind-http-https": false,
	"role": ""
}
}
```

The name option is the name of the new service. The haproxy-group option sets the HAProxy group. This group is what goes in the HAPROXY_GROUP label in the service. Marathon will use whichever load balancer that matches the group. Create the new load balancer by running the following command:

```
$ dcos package install --options=lb-options.json marathon-lb
We recommend a minimum of 0.5 CPUs and 256 MB of RAM available for the
Marathon-LB DCOS Service.
Continue installing? [yes/no] yes
```

If there are resources available, the load balancer will start and be ready to serve requests.

## Providing persistent storage

Running ephemeral containers like these is all well and good, but more complicated workloads will require some form of additional storage. DC/OS and Marathon provide two options. The first is local storage, which is only available on the node that the task starts on. The second is external volumes, which are available anywhere in the cluster.

> As of this writing, volumes are considered experimental. The functionality may change.

## Using local volumes

Local persistent volumes are blocks of data allocated on a node that persists across application upgrades and restarts. Because the volume is located on a single node, the application is pinned to that node and will always start there. This means that the data will always be available for the application, but it also means that if that node goes away, the application will not be able to start. The following example shows how to add a local volume:

```
{
 "id" : "nginx-volume",
 "instances" : 1,
 "cpus" : 0.25,
 "mem" : 64.0,
 "container" : {
 "type" : "DOCKER",
 "docker" : {
 "image" : "nginx",
 "network" : "USER"
 },
 "volumes" : [
 {
 "containerPath" : "website",
 "mode" : "RW",
 "persistent" : {
 "size" : 20
 }
 }
 ],
 "ipAddress" : {
 "networkName" : "dcos"
 },
 "residency": {
 "taskLostBehavior": "WAIT_FOREVER"
 }
 }
```

This example creates an application with a local persistent volume. Volumes are created with the `volumes` block within the container definition. The path of the volume is set by `containerPath`. The path may not contain forward slashes (`/`). Within the container, the volume will be available in `/mnt/mesos/sandbox/containerPath`. In this case, the `website` volume will be `/mnt/mesos/sandbox/website`.

The `mode` option can only be `RW`, which gives the container read-write access to the volume. Other modes may be added as the volumes feature is developed. The `persistent` block only has the `size` option which defines the amount of space to be allocated in MiB.

Finally, a `residency` block must be defined with `taskLostBehavior` set to `WAIT_FOREVER`. This signals Marathon that this application is stateful.

Some applications do not allow you to change the path where the data lives. This could be a problem for applications such as MySQL. Since it is not possible to specify a specific path, Marathon has a workaround. The trick is to specify a second volume:

```
{
"id" : "nginx-volume",
"instances" : 1,
"cpus" : 0.25,
"mem" : 64.0,
"container" : {
"type" : "DOCKER",
"docker" : {
"image" : "nginx",
"network" : "USER"
},
"volumes" : [
{
    "containerPath" : "website",
    "mode" : "RW",
    "persistent" : {
        "size" : 20
    }
},
{
    "containerPath" : "/var/lib/www",
    "mode" : "RW",
    "hostPath" : "website"
}
],
"ipAddress" : {
    "networkName" : "dcos"
},
"residency" : {
    "taskLostBehavior" : "WAIT_FOREVER"
}
}
```

This example extends the previous one to mount the `website` volume in `/var/lib/www`.
The second volume uses `hostPath`, which matches `containerPath` of the persistent
volume. Since `hostPath` is being used, `containerPath` can point to any location in the
container's file system. The data stored on the volume is available in both
`/mnt/mesos/sandbox/website` and `/var/lib/www`.

### Using external volumes

The biggest failing with local persistent volumes is that the data is stuck on a single node. If
that node fails, the data is no longer accessible. External volumes allow an application to
store data off the node where the application is run. An application can be started anywhere
in the cluster and still access its data.

Mesosphere uses REX-Ray to manage access to external storage. REX-Ray supports Amazon
EC2, GCE, OpenStack, and more. A complete list of supported storage drivers is available at
`https://rexray.readthedocs.io/en/v0.3.3/user-guide/storage-providers/`.

External storage must be enabled when Mesosphere is installed. The configuration is enabled in `genconf/config.yaml` in the installation package. The specifics will depend on the driver used. Following is an example for GCE:

```
rexray_config:
  rexray:
    loglevel: info
    modules:
      default-admin:
        host: tcp://127.0.0.1:61003
    storageDrivers:
      - gce
    gce:
      keyfile: /path/to/gce-auth.json
```

Multiple storage drivers can be set in the `storageDrivers` section. Each storage driver will have its own section for configuration. In the case of GCE, `keyfile` is the JSON formatted credentials from the API portal.

The following example shows how one would use an external volume. The only option for provider is `dvdi` with the `dvdi/driver` option set to `rexray`. The value for `name` is the name of the volume from the storage provider. In the following example, the `testpd` volume will be loaded from the storage provider:

```
{
  "id" : "nginx-volume",
  "instances" : 1,
  "cpus" : 0.25,
  "mem" : 64.0,
  "container" : {
    "type" : "DOCKER",
    "docker" : {
      "image": ["nginx"],
      "network" : "USER"
    }
  },
  "volumes" : [
    {
      "containerPath" : "website",
      "mode" : "RW",
      "external" : {
        "size" : 200,
        "name" : "testpd",
        "provider" : "dvdi",
        "options" : {
          "dvdi/driver" : "rexray"
        }
      }
    }
  ]
}
],
"ipAddress" : {
"networkName" : "dcos"
},
"residency": {
"taskLostBehavior": "WAIT_FOREVER"
}
}
```

The rules for volume names with local volumes also apply to external volumes as well. If the volume is to be mounted in a specific path, a second volume must be used as demonstrated earlier.

Mesosphere only allows a single instance of an application to mount a specific external volume. A task may be scaled down to zero. The volume will persist and will be ready for use when the application is scaled back up.

Here are a few other things to keep in mind when using external persistent volumes:

- The volume will not be destroyed when the associated cluster is destroyed. You are responsible for volume clean up.

- For some providers, images that are created in a certain zone or region are only available to hosts running in that same zone.

- Ensure that your volume names are unique to avoid conflicts.

- Make sure that a compatible version of Docker is used. The REX-Ray documentations recommends Docker 1.10.2 or higher.

### Using a private registry

Mesosphere has the ability to use a private Docker Registry for images. Images can be pulled from a local registry in the same way one would with Docker; using the registry address followed by the image. For example, to use the myapp image that is on registry.example.com, you would set the image to registry.example.com/myapp.

It is also possible to host a private registry on your Mesosphere cluster. The registry can be set up using host networking or with either VIP or Marathon-LB as the load balancer. The following example creates an insecure registry which uses Marathon-LB:
