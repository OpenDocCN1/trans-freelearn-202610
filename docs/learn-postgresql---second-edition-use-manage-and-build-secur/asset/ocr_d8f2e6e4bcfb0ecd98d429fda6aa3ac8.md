For instance, the following is a PGDATA directory that has three different tablespaces:

```
$ ls -l /postgres/16/data/pg_tblspc/
lrwxrwxrwx 1 postgres postgres 22 Jan 19 13:08 16384 -> /data/tablespaces/ts_a
lrwxrwxrwx 1 postgres postgres 22 Jan 19 13:08 16385 -> /data/tablespaces/ts_b
lrwxrwxrwx 1 postgres postgres 22 Jan 19 13:08 16386 -> /data/tablespaces/ts_c
```

As you can see from the preceding example, there are three tablespaces that are attached to the /data storage. You can inspect them with oid2name and the -s flag:

```
$ oid2name -s
All tablespaces:
Oid Tablespace Name
----------------
1663 pg_default
1664 pg_global
16384 ts_a
16385 ts_b
16386 ts_c
```

As you can see, the numeric identifiers of the symbolic links are mapped to the mnemonic names of the tablespaces. From the preceding example, you can observe that there are also two particular tablespaces:

+   - pg_default is the default tablespace corresponding to “none,” the default storage to be used for every object when nothing is explicitly specified. In other words, every object stored directly under the PGDATA directory is attached to the pg_default tablespace.
+   - pg_global is the tablespace used for system-wide objects.

By default, both of the preceding tablespaces refer directly to the PGDATA directory, meaning any cluster without a custom tablespace is totally contained within the PGDATA directory.

# Exploring configuration files and parameters

The main configuration file for PostgreSQL is postgresql.conf, a text-based file that drives the cluster when it starts.