As an example, suppose we want to give access to the luca and enrico roles (from the same network that the cluster is running into) to the forumdb and learnpgdb databases so that pg_hba.conf looks like the following:

```
host forumdb luca samenet scram-sha-256
host forumdb enrico samenet scram-sha-256
host learnpgdb luca samenet scram-sha-256
host learnpgdb enrico samenet scram-sha-256
```

Since the database and the role fields can list more than one item, the preceding code can be compressed into the following one:

```
host forumdb,learnpgdb luca samenet scram-sha-256
host forumdb,learnpgdb enrico samenet scram-sha-256
```

We can shrink the rules one step further since the machine from which the database connection can be established is literally the same for both rules, and therefore the final code is as follows:

```
host forumdb,learnpgdb luca, enrico samenet scram-sha-256
```

It should now be clear to you that if more rules have the same authentication method and connection protocol, then it is possible to collapse them into an aggregation. This can help you manage the host-based access configuration.

# Using groups instead of single roles

The role field in every pg_hba.conf rule can be substituted by the name of a group (remember that a group is itself a role); however, in order to make the rule valid for every member of the group, you have to prefix the group name with a + (plus) sign.

To better understand this, consider the example of the book_authors group, which includes the luca member. The following rule will not allow the luca role to access the database:

```
host forumdb book_authors all scram-sha-256
```

Even if the user is a member of the book_authors role, it will be denied the ability to log in to the database; the cluster host-based access policy requires the book_authors role to be exactly matched by a rule, and in the following command, the luca role does not match any rule:

```
$ psql -U luca forumdb
psql: error: could not connect to server:
FATAL:  no pg_hba.conf entry for host "192.168.222.1", user "luca", database "forumdb", SSL off
```