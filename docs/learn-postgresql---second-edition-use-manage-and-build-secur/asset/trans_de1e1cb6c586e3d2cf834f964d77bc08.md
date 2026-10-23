

The replica server will be in a state of continuous recovery, and it continuously executes the WAL that is passed by the primary machine; this way, the replica machine binarily replicates the data of the primary machine through the WAL.

As we’ve seen in *Chapter 15, Backup and Restore*, in a classic PITR situation, WAL segments are saved somewhere by the primary, and then they are taken by the recovery machine using manual scripts:

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_645_0.png)

In a streaming replication context, a communication channel will be open between the replica and primary, and the primary will send the WAL segments through it:

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_645_1.png)

The replica server will receive the WAL segments and rerun them, remaining in a permanent recovery state.

We will now look at how to perform asynchronous physical replication. The technique is very similar to PITR.

## Asynchronous replication environment

Let’s prepare our environment. We need two servers: the first one will be called pg1, and its IP will be 192.168.122.10; the second one will be called pg2, and its IP will be 192.168.122.11. Let’s take a look at the preparatory steps for physical replication.

On the primary server, we need to do the following:

- 1. The first thing we have to do is modify listen_addresses so that it listens to the network. If we set listen_addresses = '*', PostgreSQL will listen to any IP; otherwise, we can specify a list of IP addresses separated by commas. This change requires a restart of the PostgreSQL service.
- 2. We need to create a new user that is able to perform the replication:
- 3. We have to modify the pg_hba.conf file so that from the replica machine with the user replicarole, it is possible to reach the primary machine:
- 4. To make this configuration active, we need to run a reload of the PostgreSQL server. For example, we can run the following:
- 5. On the replica server, we have to turn off the PostgreSQL service, destroy the PGDATA directory, and remake it – this time, empty and with the right permissions. To do this, we can use these statements:

```
postgres=# CREATE role replicarole WITH REPLICATION ENCRYPTED PASSWORD 'SuperSecret' LOGIN;
CREATE ROLE
```

```
host    replication    replicarole    192.168.122.11/32
scram-sha-256
```

```
postgres=# select pg_reload_conf();
 pg_reload_conf
-----------------
 t
(1 row)
```

```
root@pg1:/# systemctl stop postgresql
root@pg1:/# cd /var/lib/postgresql/16/
root@pg1:/# rm -rf main
root@pg1:/# mkdir main
```

## The slot way

In PostgreSQL, there is another approach that can be used to solve the problem of storing WAL segments: the slot technique. Through the slot technique, we can tell PostgreSQL to keep all the WAL segments on the primary until they have been transferred to the replica servers. In this way, we have dynamic, variable, and fully automated management of the number of WAL segments that the primary server must keep as a buffer. This is a very easy way to manage our physical replicas, and it is the method we will focus on in this book.

The instruction we need to perform on PostgreSQL to create a new slot is as follows:

```
postgres=# SELECT * FROM pg_create_physical_replication_slot('master');
slot_name | lsn
-----------+-----
master     |
(1 row)
```

The instruction we need to perform on PostgreSQL to drop a slot is this:

```
postgres=# select pg_drop_replication_slot('master');
pg_drop_replication_slot
------------------------

(1 row)
```

Later on in this chapter, we will look at these instructions in more detail.

## The pg_basebackup command

In Chapter 15, Backup and Restore, in the section Basic concepts behind Point In Time Recovery, we talked about the base backup; this is a hot backup that acts as a starting base on which we can then perform all the WAL segments. There is a command called pg_basebackup that implements this procedure almost automatically.

It is necessary that the max_wal_senders value is at least 2. It is a very useful command for the DBA because it allows us to do everything we need to do with a single instruction. We will use and better explain this command in the next section, where we will implement our first asynchronous physical replication.

For further information about the pg_basebackup command, please refer to https://www.PostgreSQL.org/docs/current/app-pgbasebackup.html.

## Asynchronous replication

We now have all the building blocks necessary to easily and quickly make our first asynchronous physical replication. By default, in PostgreSQL, physical replication is asynchronous. Let’s now start with the replication technique. By following the steps from the previous sections of this chapter, we already have a primary server ready to be connected to the replica server, and we have the replica ready to receive information from the primary. The replica server will now have the PostgreSQL service turned off and the PGDATA data folder created, empty, and with the right permissions granted:

- 1. Let’s go inside the PGDATA directory as the system postgres user:

```
root@pg2:# su - postgres
postgres@pg2:~$ cd /var/lib/PostgreSQL/16/main
```

- 2. Now let’s run the pg_basebackup command with the right options. This command will execute the base_backup command from the primary machine to the replica machine and prepare the replica machine to receive and execute the received WAL segments, causing the replica server to remain in a state of permanent recovery:

```
postgres@pg2:~/16/main$ pg_basebackup -h 192.168.122.10 -U replicarole -p5432 -D /var/lib/PostgreSQL/16/main -Fp -Xs -P -R -S master
Password:
22483/22483 kB (100%), 1/1 tablespace
```

The password that we have to insert is the password of the replicarole user; in our case, this is SuperSecret. If the pg_basebackup doesn’t start quickly, that means that it is waiting for a checkpoint from the primary, so to improve the performance of this operation, we can go on the primary server and execute:

```
postgres=# checkpoint ;
CHECKPOINT
```

Let’s analyze the pg_basebackup command in more detail:

- -h: With this option, we see the host that we want the replica to connect to.
- -U: This is the user created on the primary server used for replication.
- -p: This is the port where the primary server listens.
- -D: This is the PGDATA value on the replica server.
- -Fp: This performs a backup on the replica, maintaining the same data structure present on the primary.
- -Xs: This opens a second connection to the primary server and starts the transfer of the WAL segments at the same time as the backup is performed.
- -P: This shows the progress of the backup.
- -S: This is the slotname created on the primary server.
- -R: This creates the standby.signal file and adds the connection settings to the PostgreSQL.auto.conf file:
- postgres@pg2:~/16/main$ cat postgresql.auto.conf
- # Do not edit this file manually!
- # It will be overwritten by the ALTER SYSTEM command.
- primary_conninfo = 'user=replicarole password=SuperSecret channel_binding=disable host=192.168.122.10 port=5432 sslmode=disable sslcompression=0 sslcertmode=disable sslsni=1 ssl_min_protocol_version=TLSv1.2 gssencmode=disable krbsrvname=postgres target_session_attrs=any load_balance_hosts=disable'
- primary_slot_name = 'master

3. Now let’s start the PostgreSQL service on the replica machine, and physical replication should work. As the root user, let’s execute the following:

```
root@pg2:/var/lib/postgresql/16# systemctl start postgresql
```

As we can see from the PostgreSQL log file (/var/log/postgresql/postgresql-16-main.log), the replica machine started in standby mode and read-only mode:

```
2023-05-22 13:27:29.823 UTC [1244] LOG:  entering standby mode
2023-05-22 13:27:29.832 UTC [1244] LOG:  redo starts at 0/2000028
2023-05-22 13:27:29.835 UTC [1244] LOG:  consistent recovery state reached at 0/2000100
2023-05-22 13:27:29.835 UTC [1241] LOG:  database system is ready to accept read-only connections
```

4. Let’s connect to the replica server and try to see whether everything has been replicated:

```
postgres=# \l
databases
List of
```

