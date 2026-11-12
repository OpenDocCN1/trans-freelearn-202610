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

# Monitoring services

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
```