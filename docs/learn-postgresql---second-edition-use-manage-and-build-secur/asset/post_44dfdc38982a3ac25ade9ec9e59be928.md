
In this chapter, when we talk about how to install a replication system, we will not use Docker containers; this is because to install a replication service, we need to shut down the postgresql service on the replica server, and shutting down a service in a Docker environment shuts down the whole container. However, on Docker, shutting down postgresql and restarting it at the same time can be done when the Docker container starts for the first time, so to better understand how to install a replica on a production server, it is better not to use a Docker container (even if on the GitLab repo you can find some containers that you can use). In this chapter, Docker is only used to help the reader who doesn’t want to install a replica server to learn and instead wants to see how physical replication works; for everything concerning the installation part, we will refer not to the paths of the Docker images but to the paths of a Debian server.

## Technical requirements

In the Learn PostgreSQL GitHub repository, you can find three Docker images:

- **chapter17_streaming**: Primary/replica asynchronous replication; if you want to use the replica container after starting the container with:
  
  ```
  chapter_17$ bash run-pg-docker.sh chapter17_streaming
  ```
  
  you have to run:
  
  ```
  chapter_17$ bash run-pg-docker_replica.sh chapter17_streaming
  ```
- **chapter17_synchronous**: Primary/replica synchronous replication; if you want to use the replica container after starting the container with:
  
  ```
  chapter_17$ bash run-pg-docker.sh chapter17_synchronous
  ```
  
  you have to run:
  
  ```
  chapter_17$ bash run-pg-docker_replica.sh chapter17_synchronous
  ```
- **chapter17_delayed**: Primary/replica with delayed replication; if you want to use the replica container after starting the container with:
  
  ```
  chapter_17$ bash run-pg-docker.sh chapter17_delayed
  ```
  
  you have to run:
  
  ```
  chapter_17$ bash run-pg-docker_replica.sh chapter17_delayed
  ```

All the replica containers are stopped when we exit from the primary container.

In this chapter, we will cover the following topics:

- Exploring basic replication concepts
- Managing streaming replication

## Exploring basic replication concepts

In PostgreSQL, there are two kinds of physical replication techniques:

- **Asynchronous replication:** In asynchronous replication, the primary device (source) sends a continuous flow of data to the secondary one (target), without receiving any return code from the target. This type of copying has the advantage of speed, but it brings with it greater risks of data loss because the received data is not acknowledged.
- **Synchronous replication:** In synchronous replication, a source sends the data to a target, that is, the second server; at this point, the server acknowledges that the changes are correctly written. If the check is successful, the transfer is completed.

Both methods have advantages and disadvantages, and in the *Managing streaming replication* section of this chapter, we will analyze them.

## Physical replication and WALs

Let’s briefly summarize what we have already covered about MVCC and WAL segments: we have seen how PostgreSQL stores data on disk using WAL segments, and as we saw in *Chapter 11, Transactions, MVCC, WAL, and Checkpoints*, WAL segments are mainly used in the event of a crash. After a crash, PostgreSQL retraces WAL segments and reapplies them to data starting from the last checkpoint; during the recovery time after a crash, the server puts itself in a recovery state.

Here is a summary of the key information about WAL segments:

- The WAL size is fixed at 16 MB.
- By default, WAL files are deleted as soon as they are older than the latest checkpoint.
- We can maintain extra WAL segments using wal_keep_segments.
- WAL segments are stored in the pg_wal directory as shown here:

```
postgres@pg2:~/16/main/pg_wal$ ls -alh
total 17M
drwx------  3 postgres postgres 4.0K May 22 09:52 .
drwx------ 19 postgres postgres 4.0K May 22 10:18 ..
-rw-------  1 postgres postgres 16M May 22 10:18
000000010000000000000001
drwx------  2 postgres postgres 4.0K May 22 09:52 archive_status
```

## The wal_level directive

The wal_level directive sets what kind of information should be stored in WAL segments. The default value is minimal. With this value, all information that is stored in a WAL segment can support archiving and physical replication.

> For further information, see https://www.postgresql.org/docs/current/runtime-config-wal.html#GUC-WAL-LEVEL.

So, in this chapter, we will use the wal_level=replica value, which is the default value, and in the next chapter, we will use wal_level=logical. We have to remember that we need to restart the PostgreSQL server every time we change the wal_level parameter.

## Preparing the environment setup for streaming replication

In this section, we will prepare the two servers that we need to proceed: the first one is the primary server machine, and the second one is the replica server. So, let’s proceed with the installation of two virtual machines. In the following examples, we will use two Debian Linux virtual machines, with 192.168.122.10 as the IP for the primary server and 192.168.122.11 as the IP for the replication server. In this chapter, all the paths refer to a PostgreSQL 16 instance installed on Debian, for example, /var/lib/postgresql/16/main.

1. For the primary server, we will see the following output:

```
root@pg1# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel
state UP group default qlen 1000
    link/ether 52:54:00:5c:df:f4 brd ff:ff:ff:ff:ff:ff
    inet 192.168.122.10/24 brd 192.168.122.255 scope global enp1s0
        valid_lft forever preferred_lft forever
    inet6 fe80::5054:ff:fe5c:dff4/64 scope link
        valid_lft forever preferred_lft forever

root@pg1:# su - postgres
postgres@pg1:~$ psql
psql (16)
Type "help" for help.

postgres=#
```

2. Similarly, for the replica server, we will have the following:

```
root@pg2:~# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
        valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
        valid_lft forever preferred_lft forever
2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel
state UP group default qlen 1000
    link/ether 52:54:00:93:47:18 brd ff:ff:ff:ff:ff:ff
    inet 192.168.122.11/24 brd 192.168.122.255 scope global enp1s0
        valid_lft forever preferred_lft forever
    inet6 fe80::5054:ff:fe93:4718/64 scope link
        valid_lft forever preferred_lft forever
root@pg2:~# su - postgres
postgres@pg2:~$ psql
psql (16)
Type "help" for help.

postgres=#
```

3. Let’s check to see whether there is a connection between the two servers.

Using the ping command, we will do a simple test to check if the node pg1 can connect to the node pg2 and if the node pg2 can connect to the node pg1:

```
postgres@pg1:~$ ping 192.168.122.11
PING 192.168.122.11 (192.168.122.11) 56(84) bytes of data.
64 bytes from 192.168.122.11: icmp_seq=1 ttl=64 time=0.292 ms
64 bytes from 192.168.122.11: icmp_seq=2 ttl=64 time=0.406 ms

postgres@pg2:~$ ping 192.168.122.10
PING 192.168.122.10 (192.168.122.10) 56(84) bytes of data.
64 bytes from 192.168.122.10: icmp_seq=1 ttl=64 time=0.536 ms
64 bytes from 192.168.122.10: icmp_seq=2 ttl=64 time=0.359 ms
```

Now that everything is ready, let’s start exploring the details of physical replication.

## Managing streaming replication

In this section, we will talk about why we have to have replicas.

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_644_0.png)

Figure 17.1: Primary/Replica Schema

In a production environment, you often need to be able to restore it as quickly as possible after a system crash. In order to do this, we have to use the streaming replication technique. To make this possible, we need at least two servers, one primary server and one secondary server. The primary server performs all the operations that will be requested by the application programs; the replica server will be available only for read operations and will have the data copied in real time.

## Basic concepts of streaming replication

The idea behind streaming replication is to copy the WAL files from the primary server to another (replica) server.
