
The preceding command sets the hard memory limit to one gig, and then sets the memory reservation to half a gig. With those constraints set, when the container consumes memory more than 500M and less than 1G, Docker will attempt to shrink container memory below 500M.

In the next example, we are going to set the memory reservation without setting the hard memory limit:

```
docker run -it --memory-reservation 1G ubuntu /bin/bash
```

In the previous example, when the container starts, it can use as much memory as its processes need. The --memory-reservation switch setting will prevent the container from consuming too much memory for a long time, because every memory reclaim will shrink the container's memory usage to the size specified in the reservation.

The kernel memory is something entirely different from the user memory – the main difference is that kernel memory can't be swapped out to disk. It includes stack pages, slab pages, sockets memory pressure, and TCP memory pressure. You use the --kernel-memory switch to set up kernel memory limit to constrain these kinds of memory. The same as in setting the user memory limit, just provide a number with a suffix like k, m, g for kilobyte, megabyte or gigabyte respectively – although setting it in kilobytes may be a really rare case.

For example, every process eats some stack pages. By restricting kernel memory, you can prevent new processes from being started when the kernel memory usage is too high and because the host cannot swap the kernel memory to disk, the container can block the whole host services by consuming too much kernel memory.

Setting the kernel memory limit is straightforward. We can set the --kernel-memory alone, without limiting the total memory with -m, as seen in the following example:

```
docker run -it --kernel-memory 100M ubuntu /bin/bash
```

In the previous example, the process in the container can take memory as it needs, but it can only consume 100M kernel memory. We can also set up the hard memory limit, as in the following command:

```
docker run -it -m 1G --kernel-memory 100M ubuntu /bin/bash
```

In the previous example, we set memory and kernel memory altogether, so the processes in the container can use 1G memory in total, and this 1G will include 100M of the kernel memory.

One more constraint related to the memory which can be useful when running containers, is the swappiness constraint. We apply the constraint by using the `--memory-swappiness` switch to the `docker run` command. It can be helpful when you want to avoid performance drops related to memory swapping. The parameter for the `--memory-swappiness` switch is the percentage of anonymous memory pages that can be swapped out, so it takes values from 0 to 100. A value of 0 turns off anonymous page swapping completely. On the contrary, a value of 100 sets all anonymous pages as candidates for swapping out. For example:

```
docker run -it --memory-swappiness=0 ubuntu /bin/bash
```

In the previous command we turn off swapping completely for our `ubuntu` container.

Apart from setting the memory usage constraint, you can also instruct Docker how the processor power should be assigned to containers it's going to run.

## Processors

Using the `-c` (or `--cpu-shares` as an equivalent) for the `docker run` command switch it's possible to specify a value of shares of the CPU that container can allocate. By default, every new container has 1024 shares of CPU and all containers get the same part of CPU cycles. This percentage can be altered by shifting the container's CPU share weighting relative to the weighting of all other running containers. But take note, that you cannot set the precise processor speed that container can use. This is a relative weight and has nothing to do with the real processor speed. In fact, there is no way to say precisely that a container should have the right to use only 2GHz of host's processor.

![](img/0c87321919fa245e22f056c67ec70003_193_0.png)

> CPU share is just a number – it's not related at all to the CPU speed.

If we start two containers and both will use 100% CPU, the processor time will be divided equally between the two containers. The reason for that is two containers will have the same number of processor shares. But if you constrain one container's processor shares to 512 it will receive just a half of the CPU time. This does not mean that it can use only half of the CPU – the proportion will only apply when CPU-intensive processes are running. If the other container (with 1024 shares) is idle – our container will be allowed to use 100% of the processor time. The real amount of CPU time will differ depending on the number of containers running on the system. It's easier to understand on a tangible example.

Consider three containers, one (let's call it Container1) has --cpu-shares set for 1024 and two others (Container2 and Container3) have a --cpu-shares setting of 512. When processes in all three containers attempt to use all of the CPU power, the Container1 would receive 50% of the total CPU time, because it has half of the CPU usage allowed in comparison to the sum of other running containers (Container2 and Container3). If we add a fourth container (Container4) with a --cpu-shares of 1024, our first Container1 will only get 33% of the CPU, because it now has one third of the total CPU power assigned, relatively. Container2 will receive 16.5%, Container3 also 16.5% and the last one, Container4, again, will be allowed to use 33% of the CPU.

While the -c or --cpu-shares flag for the docker run command modifies the container's CPU share weighting relative to the weighting of all other running containers, it does not restrict the container's use of CPU from the host machine. But there's another flag to limit the CPU usage for the container: --cpu-quota. Its default value is 100000 which means allowance for 100% of the CPU usage. We can use the --cpu-quota to limit CPU usage, for example:

```
docker run -it --cpu-quota=50000 ubuntu /bin/bash
```

In the preceding command, the limit for the container will be 50% of a CPU resource. The --cpu-quota is usually used in conjunction with --cpu-period flag for the docker run. This is the setting for CPU CFS (Completely Fair Scheduler) period. The default period value is 100000 which is 100 milliseconds. Take a look at the example:

```
docker run -it --cpu-quota=25000 --cpu-period=50000 ubuntu /bin/bash
```

It means that the container can get 50% of the CPU usage every 50ms.

Limiting CPU shares and usage is not the only processor-related constraint we can set on the container. We can also assign the container's processes to a particular processor or processor core. The --cpuset switch of the docker run command comes in handy when we want to do this. Consider the following example:

```
docker run -it --cpuset 4 ubuntu
```

The preceding command will run the ubuntu image and allow container to use all of four processor cores. To start the container and only allow usage on one processor core, you can change the --cpuset value to 1:

```
docker run -it --cpuset 1 ubuntu
```

![](img/0c87321919fa245e22f056c67ec70003_195_0.png)

You can of course mix the option --cpuset with --cpu-shares to tweak your container's CPU constraints.

## Updating constraints on a running container

As with the restart policies, the constraints can be updated also when the container is already running. This may be helpful, if you observe your containers eating too much of the Docker host system resources and would like to limit this usage. Again, we use the docker update command to do this.

The same as with restart policies, the syntax for the docker update command will be the same as when starting the container – you specify the desired constraints as an argument for the docker update command and then give the container ID (taken from the docker ps command output for example) or its name. Again, if you would like to change the constraints on more than one container at once, just provide their IDs or names separated with a space. Let's look at some examples how to update constraints at the runtime:

```
docker update --cpu-shares 512 abbdef1231677
```

The previous command will limit the CPU shares to the value of 512. Of course, you can apply CPU and memory constraints at the same time, to more than one container:

```
docker update --cpu-shares 512 -m 500M abbdef1231677 dabdff1231678
```
