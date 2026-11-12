Each instance of the template is named nginxha@id.service, where id is whatever makes sense for the service. Usually, id is a number, but it does not have to be:

```
$ fleetctl start nginxha@1.service
Unit nginxha@1.service inactive
Unit nginxha@1.service launched on 86bceaea.../172.17.8.103
$ fleetctl start nginxha@2.service
Unit nginxha@2.service inactive
Unit nginxha@2.service launched on 9c07271b.../172.17.8.102
$ fleetctl start nginxha@3.service
Unit nginxha@3.service inactive
Unit nginxha@3.service launched on acbccfa4.../172.17.8.101
```

```
$ fleetctl list-units
UNIT                      MACHINE                         ACTIVE      SUB
nginxha@1.service         86bceaea.../172.17.8.103        active      running
nginxha@2.service         9c07271b.../172.17.8.102        active      running
nginxha@3.service         acbccfa4.../172.17.8.101        active      running
```

In this example, three instances of the nginxha@.service template were started. They were named nginxha@1, nginxha@2, and nginxha@3. Notice; all of them were started on different machines. A load balancer could then be used to spread the load across all the instances. If any of the units fail, the overall service will remain up.

# Sidekick services

Most services are not aware of what else is happening in the cluster, by design. How does a service let other things know that it is up and ready to answer requests? To do that, a separate service called sidekick is started. The sidekick will notify the other services that the container has started or stopped. The specifics will vary depending on the environment. For example, it might issue API calls to an external load balancer.

Fleet does not have a native way to inform other services that a new unit has started. What it does have is etcd. The etcd key value store is designed for exactly this sort of message passing. The following example is a simple sidekick that updates etcd when nginx.service starts:

```
[Unit]
Description=Nginx Sidekick
BindsTo=nginx.service
After=nginx.service
```