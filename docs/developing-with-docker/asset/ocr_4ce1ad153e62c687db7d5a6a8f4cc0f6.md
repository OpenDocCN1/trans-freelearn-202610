A fork bomb (also known as rabbit virus or wabbit) is a denial-of-service attack, wherein a process continually replicates itself to deplete the available system resources, slowing down or crashing the system due to resource usage. To limit the number of active processes within the container, execute the docker run command with --pids-limit value set:

```
docker run --pids-limit=64
```

Getting the current PID of the container is easy using the docker inspect command:

```
docker inspect --format '{{.State.Pid}}' <containerID or name>
```

On the other hand, getting the container name by PID is more tricky, but doable. To do this, you will need to use some grepping, as shown in the following example:

```
dockerps -q | xargs docker inspect --format '{{.State.Pid}}, {{.ID}}' |
grep "^${PID},"
```

Similar to how some programs write their process ID to a file (you've seen them as PID files), Docker can write the container ID to a file you choose. To write the process ID to a file, use the --cidfile switch:

```
--cidfile=""
```

Apart from the PID namespace, you can also set the UTS namespace for your containers.

# UTS settings

As you remember Docker tries to isolate the container from the host system. The UTS namespace is for setting the hostname and the domain, which is visible, to run processes in that namespace. UTS is the abbreviation for UNIX Timesharing System. By default, all containers have the UTS namespace enabled. It means the process has a separate copy of the hostname and the (now mostly unused) NIS domain name, so it can set it to something else without affecting the rest of the system.

The hostname is set via sethostname and is the nodename member of the struct returned by uname. The NIS domain name is set by setdomainname and is the domainname member of the struct returned by uname.

The syntax of setting the UTS namespace in Docker is very similar to the --pid syntax, you will just use --uts instead of --pid. To set the UTS namespace for the container, use the --uts="value" switch. You can also use the --uts=host to bind the host's UTS namespace within your container.