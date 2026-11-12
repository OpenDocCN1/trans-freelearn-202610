
Once the file is changed, it can be passed to `dcos`:

```
$ dcos marathon app update nginx < nginx-1.11.json
Created deployment 2c28f5db-0fcd-4605-9d5b-b1abd5986579
```

Once again, Marathon will proceed to replace the old tasks with ones running the updated image. When the deployment finishes, you can verify that the application is running the new image:

```
$ dcos marathon app show nginx | grep image
"image": "nginx:1.11",
```

## Setting an upgrade strategy

An application can define an upgrade strategy to control how application tasks are replaced. By default, Marathon will start all of the new tasks before stopping the old ones. This can be problematic for two reasons:

- First, the update could corrupt data if the application cannot handle multiple tasks writing to the same volume
- Second, you need to have enough capacity to run all of those extra tasks

To solve this problem, Marathon allows one to set an `upgradeStrategy` for an application:

```
"upgradeStrategy": {
"minimumHealthCapacity": 1
}
```

The `minimumHealthCapacity` option is a value from zero to one. When set to 1, the new instances are started before the old ones have been stopped. When set to 0, all of the old instances are stopped before the new ones start. This solves the first problem. It also, technically, solves the second, but there could be noticeable downtime when the number instances gets too low to serve the clients trying to access it.

To smoothly update the application while minimizing client impact, `minimumHealthCapacity` can be set to a fraction between 0 and 1. This value represents a percentage of running instances to scale the application down to. For example, if there are ten instances and `minimumHealthCapacity` is set to `0.4` (40 percent capacity), the application will be scaled down to four instances and six new instances will be started. Anything over `0.5` means that additional capacity will be needed for the upgrade. If the new instances have no errors, the rest of the old instances will be shut down and the new instances will be started to take their place.

## Testing updates with blue-green deployments

The built-in update mechanism is great for automatically deploying a new version of the application. In some cases, however, it may be desirable to roll out an update more slowly to verify that there is no impact on end users. This is especially useful if the updates make significant changes.

A blue-green deployment slowly replaces the old application, called blue, with a new version, called green. In Marathon, this manual process is done by creating a new application which uses the updated image. Give the new application a title that makes it obvious that it is the new one.

The green application is started with a small number of tasks; often just one. Once the health checks show that everything is ready, the green application is added to the load balancer for the service. Assuming there are a larger number of the old, blue tasks running, most of the requests will go there. A small number, however, will be sent to the green application.

Once requests start going to the green application, one can watch for errors or other issues that might require a rollback. As tests pass, the green application can be scaled up and the blue application can be scaled down. This is a manual process that can take as much or as little time as needed. It is not unusual to wait hours or days before choosing to scale up the green application. If anything goes wrong, the green application can be scaled back down or removed from the load balancer completely.

When you are satisfied that the green application is fully ready for production, the green application can be scaled up to the desired number of tasks and the blue application can be scaled to zero and removed from the load balancer. At that point, the application is fully upgraded. The blue application can be removed from the system once it is clear that it will no longer be needed for rollback.

More information on blue-green deployments and how they may be scripted is available at https://mesosphere.github.io/marathon/docs/blue-green-deploy.htm 1.

## Introducing pods

With the 1.9 release, DC/OS introduces pods. Pods in DC/OS and Marathon are very similar to those found in Kubernetes. They allow multiple containers to be grouped together into a single object. All of the containers can share volumes and communicate with each other over localhost.

## Using network services and load balancing

Mesosphere provides a number of networking services that make orchestration easier.
These services provide service discovery and load balancing for running applications.

## Discovering services with DNS

Every service that is created is assigned a hostname in Mesos DNS. The entries are
service-name.marathon.mesos, where service-name is the name of the service in the
id field. The following snippet is from a service with id set to simple-nginx. Mesos DNS
would create a record named simple-nginx.marathon.mesos:

```
{
  "id" : "simple-nginx",
  ...
}
```

Because the services are translated directly into DNS hostnames, there are a few restrictions,
as follows:

- First, names must only contain letters, numbers, and dash (-).
- Second, the names must be shorter than 24 characters. Longer names will be shortened. This can lead to problems if two services end up with the same hostname. Tasks running in one service may be returned for the other service.

The DNS service can also be queried over HTTP using REST API. Entries are returned in
JSON format. The following example gets the IP information for the simple-nginx service:

```
$ curl http://master.mesos:8123/v1/hosts/simple-nginx.marathon.mesos
[
  {
    "host": "simple-nginx.marathon.mesos.",
    "ip": "10.128.0.4"
  }
]
```

## Connecting to the overlay network

So far, the examples have used host networking. DC/OS creates a Docker overlay network when it is installed. Connecting applications to the overlay network enables the use of load balancing as well as the ability to connect to other services across the network:

```
{
"id" : "nginx-overlay",
"instances" : 1,
"cpus" : 0.25,
"mem" : 64.0,
"container" : {
"type" : "DOCKER",
"docker" : {
"image" : "nginx",
"network" : "USER"
}
},
"ipAddress":{
"networkName":"dcos"
}
}
```

The preceding example creates a service named nginx-overlay that connects to an overlay network. The network tag is set to USER and the networkName tag is set to the name of the overlay network, in this case, dcos.

## Using virtual IPs

Marathon can assign a name-based virtual IP to a service. Virtual IPs provide layer four load balancing for the services they are attached to. Tasks across the cluster can then use the virtual IP to access the service.

To add a virtual IP, click on the service in the web interface then click on the **Edit** button. Go to the **Network** section. Select **Virtual Network**. This is the same as connecting the service to an overlay network. Enter **Container Port**, **Name**, and **Protocol**, then click on the checkbox next to **Load Balanced**. If additional ports are needed, click on the **Add an endpoint** link. When you are done, click on **Deploy Changes**:

![](img/02bead9c8ff4515f78dac0eb33163961_162_0.png)

The service will be assigned a name that looks like `service-name.marathon.14lb.thisdcos.discovery` where `service-name` is the name of the service. For a service named `nginx`, the name is `nginx.marathon.14lb.thisdcos.discovery`. The service can then be accessed with the virtual IP anywhere in the cluster:

```
$ curl nginx.marathon.14lb.thisdcos.discovery:80
<!DOCTYPE html>
<html>
...
```

## Using the Marathon load balancer

Marathon provides a second load balancer by the name of Marathon-LB. It is a HAProxy-based service that can handle multiple services as well as proxy based on the hostname for a web service. Marathon-LB is an add-on installed with the DC/OS package manager:

```
$ dcos package install marathon-lb
We recommend a minimum of 0.5 CPUs and 256 MB of RAM available for the
Marathon-LB DC/OS Service.
Continue installing? [yes/no] yes
Installing Marathon app for package [marathon-lb] version [1.4.1]
Marathon-lb DC/OS Service has been successfully installed!
See https://github.com/mesosphere/marathon-lb for documentation.
```

The `dcos` command will install Marathon-LB. By default, the installation is configured to serve external requests. The load balancer is visible in DC/OS as a service named marathon-lb.

Adding a new service to the load balancer is pretty easy. Following is a service that runs nginx:

```
{
  "id" : "nginx-mlb",
  "instances" : 1,
  "cpus" : 0.25,
  "mem" : 64.0,
  "labels" : {
    "HAPROXY_GROUP" : "external"
  },
  "container" : {
    "type" : "DOCKER",
    "docker" : {
      "image" : "nginx",
      "network" : "BRIDGE",
      "portMappings" : [
        {
          "hostPort" : 0,
          "containerPort" : 80,
          "servicePort" : 10001,
          "name" : "web"
        }
      ]
    }
  },
  "upgradeStrategy": {
    "minimumHealthCapacity": 1,
```
