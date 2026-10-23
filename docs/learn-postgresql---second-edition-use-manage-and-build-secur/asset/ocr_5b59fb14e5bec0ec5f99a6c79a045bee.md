```
drwx------  3 postgres postgres 4.0K May 22 09:52 .
drwx------ 19 postgres postgres 4.0K May 22 10:18 ..
-rw-------  1 postgres postgres 16M May 22 10:18
000000010000000000000001
drwx------  2 postgres postgres 4.0K May 22 09:52 archive_status
```

# The wal_level directive

The wal_level directive sets what kind of information should be stored in WAL segments. The default value is minimal. With this value, all information that is stored in a WAL segment can support archiving and physical replication.

> For further information, see https://www.postgresql.org/docs/current/runtime-config-wal.html#GUC-WAL-LEVEL.

So, in this chapter, we will use the wal_level=replica value, which is the default value, and in the next chapter, we will use wal_level=logical. We have to remember that we need to restart the PostgreSQL server every time we change the wal_level parameter.

# Preparing the environment setup for streaming replication

In this section, we will prepare the two servers that we need to proceed: the first one is the primary server machine, and the second one is the replica server. So, let’s proceed with the installation of two virtual machines. In the following examples, we will use two Debian Linux virtual machines, with 192.168.122.10 as the IP for the primary server and 192.168.122.11 as the IP for the replication server. In this chapter, all the paths refer to a PostgreSQL 16 instance installed on Debian, for example, /var/lib/postgresql/16/main.

+   1. For the primary server, we will see the following output:

```
root@pg1# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
```