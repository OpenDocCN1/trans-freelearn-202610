

```
ExecStart=/usr/bin/docker run --name nginx -p 80:80 nginx
ExecStop=/usr/bin/docker stop nginx
```

The [Unit] section sets the description and some meta information about the service.
The Description option is a short, freeform description of the service. The After option
tells Fleet that this service must start after Docker. Since it does not make sense to run a
Docker container without Docker, the Requires option is set. For both After and
Requires, the value is the name of a systemd unit. Additional units can be added to both
by adding additional After or Requires lines or by adding additional units to the existing
lines, separated by spaces.

The steps needed to start a service that is set in the [Service] section. The
TimeoutStartSec option is the number of seconds to wait for a service to start. If the
service does not complete start up within the timeout window, Fleet assumes the service
failed and kills it.

Each ExecStartPre line combines to form a script that runs prior to starting the actual
service. Prefixing a command with a minus (-) tells systemd that it is okay for this step to
fail. Otherwise, any command that exits with a non-zero error code will prevent the service
from starting.

The first command kills the existing nginx container. If it is not running, this command
will fail, but that is just fine. Killing the service might seem a little extreme. If the service
stops cleanly, then the running container will be in a stopped state and kill will do nothing.
If the service did not stop cleanly, docker kill ensures that the service is stopped so that
it can be restarted.

The second command removes the container from Docker. A container cannot start if a
container with the same name already exists. Removing the container ensures that the
container will be recreated with all the right options later.

The third and final ExecStartPre line pulls a new copy of the image. This ensures that the
service is not running a stale image. One might think that this would be a command that
should prevent the service from starting if it fails. Letting it fail means that the service can
still start if the registry is down, assuming that the image has already been downloaded.

The `ExecStart` command is the command that actually starts the service. Multiple lines can be specified, but that does not make as much sense when using Docker containers. It is important that the `-d` flag is not passed to `docker run`. If it is, Fleet loses track of the container and stops the service.

Finally, `ExecStop` is the command to run to stop the service. Remember that `docker stop` leaves the container around. This is why `docker rm` is needed in `ExecStartPre`. You could remove the container here. Keeping it around makes it easier to troubleshoot if there are problems:

```
$ fleetctl start nginx.service
Unit nginx.service inactive
Unit nginx.service launched on 86bceaea.../172.17.8.103
```

The preceding command loads the given unit file and starts it. In this case, the `nginx.service` file is in the current directory.

A unit file can be added to the Fleet registry without starting with the `fleetctl submit` command. The service can be started at a later time with `fleetctl start`:

```
$ fleetctl submit nginx.service
Unit nginx.service inactive
```

## Monitoring services

There are two ways to check the status of a service. The first is the same `fleetctl list-units` command, which will also show the unit name, the machine where it is assigned, and the current status of each unit:

```
$ fleetctl list-units
UNIT MACHINE ACTIVE SUB
nginx.service 86bceaea.../172.17.8.103 active running
```

The other option is to use `fleetctl status`. This will not only show the status of the container but also the most recent log entries for the service and the status codes of the `ExecStartPre` commands:

```
$ fleetctl status nginx.service
● nginx.service - Nginx
Loaded: loaded (/run/fleet/units/nginx.service;
linked-runtime; vendor preset
Active: active (running) since Fri 2016-10-21 02:59:31
UTC; 1min 24s ago
Process: 3947 ExecStartPre=/usr/bin/docker pull nginx
(code=exited, status=0/SUCCESS
Process: 3942 ExecStartPre=/usr/bin/docker rm nginx
(code=exited, status=0/SUCCESS
Process: 3933 ExecStartPre=/usr/bin/docker kill nginx
(code=exited, status=1/FAILURE
Main PID: 3953 (docker)
    Tasks: 7
    Memory: 3.7M
    CPU: 40ms
CGroup: /system.slice/nginx.service
    └─3953 /usr/bin/docker run --name nginx -p
        80:80 nginx

Oct 21 02:59:27 core-03 systemd[1]: Starting Nginx...
Oct 21 02:59:27 core-03 docker[3933]: Error response
from daemon: Cannot kill container
Oct 21 02:59:27 core-03 docker[3942]: nginx
Oct 21 02:59:28 core-03 docker[3947]: Using default
tag: latest
Oct 21 02:59:31 core-03 docker[3947]: latest: Pulling
from library/nginx
Oct 21 02:59:31 core-03 docker[3947]: Digest:
sha256:e40499ca855c9edfb212e1c3ee1
Oct 21 02:59:31 core-03 docker[3947]: Status: Image is
up to date for nginx:latest
Oct 21 02:59:31 core-03 systemd[1]: Started Nginx.
```

## Viewing service logs

It is possible to view the logs from a service by viewing the journal for the unit with fleetctl journal. The command takes the name of the unit of interest. The lines marked as coming from Docker are both the output from the docker command as well as the output of docker logs. The -f flag can be added to follow the logs as new entries are added:

```
$ fleetctl journal nginx.service
-- Logs begin at Thu 2016-10-20 00:35:44 UTC, end at Fri 2016-10-21
03:12:36 UTC. --
Oct 21 02:56:45 core-03 systemd[1]: Stopped Nginx.
Oct 21 02:59:27 core-03 systemd[1]: Starting Nginx...
Oct 21 02:59:27 core-03 docker[3933]: Error response
from daemon: Cannot kill container nginx: Container
816e993ec3de47cf48f5ab19900db4a162905a133078f4fbc3bd91b27b5c24b is not
running
Oct 21 02:59:27 core-03 docker[3942]: nginx
Oct 21 02:59:28 core-03 docker[3947]: Using default tag: latest
Oct 21 02:59:31 core-03 docker[3947]: latest: Pulling from library/nginx
Oct 21 02:59:31 core-03 docker[3947]: Digest:
sha256:e40499ca855c9edfb212e1c3ee1a6ba8b2d873a294d897b4840d49f94d20487c
Oct 21 02:59:31 core-03 docker[3947]: Status: Image is up to date for
nginx:latest
Oct 21 02:59:31 core-03 systemd[1]: Started Nginx.
Oct 21 03:12:32 core-03 docker[3953]: 172.16.69.1 - - [21/Oct/2016:03:12:32
+0000] "GET / HTTP/1.1" 200 612 "-" "Wget/1.18 (linux-gnu)" "-"
```

## Stopping a service

A running service is stopped with `fleetctl stop`. It takes the name of the service as the only argument:

```
$ fleetctl stop nginx.service
Unit nginx.service loaded on 86bceaea.../172.17.8.103
```

The `stop` command stops the running container, but it leaves it scheduled on a node in the cluster. When the service is restarted, it will start on the same node that it was running on before.

To unschedule a service, use the `fleetctl unload` command. This will remove the service from the list of active units. It will remain in the Fleet registry and can be started again with `fleetctl start`:

```
$ fleetctl unload nginx.service
Unit nginx.service inactive
```

A service can be permanently removed from the cluster with the command `fleetctl destroy`:

```
$ fleetctl destroy nginx.service
Destroyed nginx.service
```

The `destroy` command is also used when changing a unit. The service must be removed with `fleetctl destroy` then it can be recreated with the required changes.

## Using environment variables

Many Docker images use environment variables to configure them and control how they run. Fleet can define those variables in the unit file for the service, using the `Environment` option:

```
[Unit]
Description=MySQL
After=docker.service
Requires=docker.service

[Service]
Environment=MYSQL_ROOT_PASSWORD=password
Environment=MYSQL_DATABASE=myapp
TimeoutStartSec=infinity
ExecStartPre=-/usr/bin/docker kill mysql
ExecStartPre=-/usr/bin/docker rm mysql
ExecStartPre=-/usr/bin/docker pull mysql
ExecStart=/usr/bin/docker run --name mysql -e MYSQL_ROOT_PASSWORD -e MYSQL_DATABASE -p 3306:3306 mysql
ExecStop=/usr/bin/docker stop mysql
```

This example starts a MySQL image and defines the MySQL root password and an initial database. The values of the variables do not need to be defined in the `docker run` command. Docker will use the values set in the `Environment` lines.

Rather than specifying all of the environment variables in the unit as was done earlier, they can all be placed into an `EnvironmentFile`. Variables and their values are defined, one per line:

```
MYSQL_ROOT_PASSWORD=password
MYSQL_DATABASE=myapp
```

The file is then specified in the `EnvironmentFile` option in the `[Service]` section of the unit:

```
...
[Service]
EnvironmentFile=/etc/mysql.env
...
```

The file must exist on every node in the cluster where the service may run. If it does not exist, the service will be unable to start.

## Scheduling services with affinity

It is often desirable to ensure that a service runs on the same host as another service. Contrarily, it may be better to force services to run on separate hosts. This process, known as affinity, is configured in a special `[X-Fleet]` section of the unit file.

