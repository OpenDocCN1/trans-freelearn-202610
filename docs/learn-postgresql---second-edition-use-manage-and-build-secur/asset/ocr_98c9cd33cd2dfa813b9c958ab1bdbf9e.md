```
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
Type "help" for help.

forumdb=>
```

Similarly, you can specify that you don’t want an SSL connection at all by setting sslmode=disable. This time, if pg_hba.conf has a hostssl mode, the connection will be rejected, while it will be served as a non-encrypted one if the pg_hba.conf file has a host line:

```
$ psql "postgresql://forum@localhost:5432/forumdb?sslmode=require"
psql: error: could not connect to server: FATAL:  no pg_hba.conf entry for host "127.0.0.1", user "forum", database "forumdb", SSL off
```

From the error, you can clearly see that there is no line that accepts a plain (host mode) connection in the pg_hba.conf file, or, on the other hand, that there are only hostssl lines.

Lastly, if you require the connection to use SSL but the PostgreSQL server is not configured to use SSL, an error message about the mismatch will be reported:

```
$ psql "postgresql://forum@localhost:5432/forumdb?sslmode=disable"
psql: error: could not connect to server: server does not support SSL, but SSL was required
```

# Summary

In this chapter, we learned that PostgreSQL provides a very rich infrastructure for managing permissions associated with roles. Internally, PostgreSQL handles permissions for different database objects by means of ACLs, and every ACL contains information about the set of permissions, the users to whom permissions are granted, and the user who granted such permissions. In terms of tabular data, it is even possible to define column-based permissions and row-level permissions to exclude users from having access to particular subsets of data.

Permissions are granted by nested roles in a dynamically inherited way or on demand, leaving you the option to fine-tune how a role should exploit privileges.

Lastly, when opportunely configured, a server can handle network connections via SSL, thereby encrypting all network traffic and data.

In the next chapter, you will learn all about transactions and how PostgreSQL manages them in a concurrent scenario, providing rock-solid stability to your data.