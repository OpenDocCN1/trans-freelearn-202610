If you are using the Docker image, PostgreSQL is already running as the main service. This means that issuing a stop or a restart command will force you to exit from the container due to its shutdown.

Moreover, in the Docker container, the PostgreSQL service will be already running without any need for manual intervention.

The status command just queries the cluster to get information, so it is pretty safe as a starting point to understand what is happening:

```
$ pg_ctl status
pg_ctl: server is running (PID: 1)
/usr/lib/postgresql/16/bin/postgres
```

The command reports back that the server is running, with a Process Identifier (PID) equal to one (this number will be different on your machine). Moreover, the command reports the executable file used to launch the server, in the above example, /usr/lib/postgresql/16/bin/postgres.

If the server is not running for any reason, the pg_ctl command will report an appropriate message to indicate that is unable to find an instance of PostgreSQL started:

```
$ pg_ctl status
pg_ctl: no server running
```

In order to report the status of the cluster, pg_ctl needs to know where the database is storing its own data—that is, where the PGDATA is on disk. There are two ways to make pg_ctl aware of where the PGDATA is:

+   - Setting an environment variable named PGDATA, containing the path of the data directory
+   - Using the -D command-line flag to specify the path to the data directory

Almost every PostgreSQL cluster-related command searches for the value of PGDATA as an environmental variable or as a -D command-line option.

In the previous examples, no PGDATA has been specified, and this is because it has been assumed the value of the PGDATA was specified by an environment variable.