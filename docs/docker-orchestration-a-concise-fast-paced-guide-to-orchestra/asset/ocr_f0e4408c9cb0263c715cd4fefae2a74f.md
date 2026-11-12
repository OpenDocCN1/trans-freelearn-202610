The containers for the tasks are stopped, but remain on the nodes. The `docker service ps` command will show that the tasks for the service are all in the `Shutdown` state. If needed, one can inspect the containers on the nodes:

```
$ docker service ps web
ID                         NAME      IMAGE       NODE      DESIRED STATE       CURRENT STATE               ERROR
68v6trav6cf2qgj8pg8wgcbmhu web.1     nginx       worker1   Shutdown            Shutdown 3 seconds ago
060wax426mtqdx79g0ulwu25r  web.2     nginx       manager   Shutdown            Shutdown 2 seconds ago
79uidx4t5rz4o7an9wtya1456  web.3     nginx       worker2   Shutdown            Shutdown 3 seconds ago
```

When it is time to bring the service back, use `docker swarm update` to set the number of replicas back to what is needed. Swarm will start the containers across in the swarm just as if you had used `docker swarm create`.

# Upgrading a service with rolling updates

It is likely that services running in a swarm will need to be upgraded. Traditionally, upgrades involved stopping a service, performing the upgrade, then restarting the service. If everything goes well, the service starts and works as expected and the downtime is minimized. If not, there can be an extended outage as the administrator and developers debug what went wrong and restore the service.

Docker makes it easy to test new images before they are deployed and one can be confident that the service will work in production just as it did during testing. The question is, how does one deploy the upgraded service without a noticeable downtime for the users? For busy services, even a few seconds of downtime can be problematic. Docker Swarm provides a way to update services in the background with zero downtime.

There are three options which are passed to `docker service create` that control how rolling updates are applied. These options can also be changed after a service has been created with `docker service update`. The options are as follows:

+   • `--update-delay`: This option sets the delay between each container upgrade.
+   The delay is defined by a number of hours, minutes, and seconds indicated by a
+   number followed by `h`, `m`, or `s`, respectively. For example, a 30 second delay will
+   be written as `30s`. A delay of 1 hour, 30 minutes, and 12 seconds will be written
+   as `1h30m12s`.