> Remember: The postmaster process is the first process ever started in the cluster. Both the backend processes and the postmaster are run starting from the postgres executable, and the postmaster is just **the root of all PostgreSQL processes**, with the main aim of keeping all the other processes under control.

Now that the cluster is running, let’s stop it. As you can imagine, stop is the command used to instruct pg_ctl about which action to perform:

```
$ pg_ctl stop
waiting for server to shut down....
[27765] LOG:  received fast shutdown request
[27765] LOG:  aborting any active transactions
[27765] LOG:  background worker "logical replication launcher" (PID 27771) exited with exit code 1
[27766] LOG:  shutting down
[27766] LOG:  checkpoint starting: shutdown immediate
[27766] LOG:  checkpoint complete: wrote 0 buffers (0.0%); 0 WAL file(s) added, 0 removed, 0 recycled; write=0.001 s, sync=0.001 s, total=0.035 s; sync files=0, longest=0.000 s, average=0.000 s; distance=0 kB, estimate=237 kB; lsn=0/1529DC8, redo lsn=0/1529DC8
[27765] LOG:  database system is shut down
done
server stopped
```

During a shutdown, the system prints a few messages to inform the administrator about what is happening, and as soon as the server stops, the message server stopped confirms that the cluster is no longer running.

Shutting down a cluster can be much more problematic than starting it, and for that reason, it is possible to pass extra arguments to the stop command in order to let pg_ctl act accordingly. There are three ways of stopping a cluster:

+   - The smart mode means that the PostgreSQL cluster will gently wait for all the connected clients to disconnect and only then will it shut the cluster down.
+   - The fast mode will immediately disconnect every client and will shut down the server without having to wait.