# Using negative affinity

Sometimes it is important that two services never run on the same node. Generally, this is for redundancy and high availability reasons. Fleet provides the `Conflicts` option for those occasions. The `Conflicts` option can match an exact unit name or a group of services using glob-matching.

With glob-matching, an asterisk (*) is used to match zero or more characters. For example, to match any service that starts with `nginx`, the pattern would be `nginx*`. The following snippet shows a unit that is configured not to start on any node that is running a service that starts with `nginx`:

```
```
[Unit]
Description=Nginx Affinity
After=docker.service
Requires=docker.service
...
[X-Fleet]
Conflicts=nginx*
```
```

# High availability

As mentioned earlier, negative affinity is often used for high availability. Fleet uses systemd template units, which make it easy to start multiple, identical services. To start, create a file named `nginxha@.service`:

```
```
[Unit]
Description=Nginx HA
After=docker.service
Requires=docker.service

[Service]
TimeoutStartSec=infinity
ExecStartPre=-/usr/bin/docker kill nginx
ExecStartPre=-/usr/bin/docker rm nginx
ExecStartPre=-/usr/bin/docker pull nginx
ExecStart=/usr/bin/docker run --name nginx -p 80:80 nginx
ExecStop=/usr/bin/docker stop nginx

[X-Fleet]
Conflicts=nginxha@*.service
```
```

For the most part, this is identical to the `nginx.service` unit that was used before. The `Conflicts` option is set to ensure that any unit that uses this template.