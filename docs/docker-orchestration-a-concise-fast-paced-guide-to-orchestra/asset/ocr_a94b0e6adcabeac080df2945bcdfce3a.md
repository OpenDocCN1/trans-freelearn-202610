```
member d263b74b628b0458 is healthy: got healthy result from
http://172.17.8.101:2379
cluster is healthy
```

This example is from a three node cluster. The output of etcdctl will list each host in the cluster and their state. The important part is the last line that shows that the cluster is healthy.

The other command is fleetctl list-machines. This command will list every machine in the cluster and any metadata that has been set on them:

```
$ fleetctl list-machines
MACHINE		IP		METADATA
86bceaea...	172.17.8.103	-
9c07271b...	172.17.8.102	-
abc0cfa4...	172.17.8.101	-
```

The fleetctl tool uses SSH to communicate with the other nodes in the cluster. Make sure that your public key is in ~/.core/.ssh/authorized_keys on every server. It also requires that you have an SSH-agent running. You can use an existing agent from your own desktop by passing -A to your ssh command. For example, ssh -A core@core-01 will connect to the core-01 server as the core user and forward your existing agent to the server. This will allow you to use your own SSH key without the need to copy the private key to all the Fleet nodes. Be aware that this is a potential security hole.

# Starting a container

Services in Fleet are defined by systemd unit files. These files work like any other unit file in systemd, but there are a few Fleet-specific extensions to allow for scheduling services across the cluster. Let's start with a simple nginx service.

Create the nginx.service file in the current working directory with the following contents:

```
[Unit]
Description=Nginx
After=docker.service
Requires=docker.service

[Service]
TimeoutStartSec=infinity
ExecStartPre=-/usr/bin/docker kill nginx
ExecStartPre=-/usr/bin/docker rm nginx
ExecStartPre=-/usr/bin/docker pull nginx
```