+   - The immediate mode will abort every PostgreSQL process, including client connections, and shut down the cluster in a dirty way, meaning that the server will need some specific activity on the restart to clean up such dirty data (more on this in the next chapters).

In any case, once a stop command is issued, the server will not accept any new incoming connections from clients, and depending on the stop mode you have selected, existing connections will be terminated. The default stop mode, if none is specified, is fast, which forces an immediate disconnection of the clients but ensures data integrity.

If you want to change the stop mode, you can use the -m flag, specifying the mode name, as follows:

```
$ pg_ctl stop -m smart
waiting for server to shut down.................... done
server stopped
```

In the preceding example, the pg_ctl command will wait, printing a dot every second until all the clients disconnect from the server. In the meantime, if you try to connect to the same cluster from another client, you will receive an error, because the server has entered the stopping procedure:

```
$ psql
psql: error: could not connect to server: FATAL:  the database system is shutting down
```

It is possible to specify just the first letter of the stop mode instead of the whole word; so, for instance, s for smart, i for immediate, and f for fast.

# PostgreSQL processes

You have already learned how the postmaster is the root of all PostgreSQL processes, but as explained in Chapter 1, Introduction to PostgreSQL, PostgreSQL will launch multiple different processes at startup. These processes are in charge of keeping the cluster operational and in good health. This section provides a glance at the main processes you can find in a running cluster, allowing you to recognize each of them and their respective purposes.

If you inspect a running cluster from the operating system point of view, you will see a bunch of processes tied to PostgreSQL:

```
$ pstree -p postgres
postgres(1)─postgres(34)
           ├─postgres(35)
           └─postgres(37)
```