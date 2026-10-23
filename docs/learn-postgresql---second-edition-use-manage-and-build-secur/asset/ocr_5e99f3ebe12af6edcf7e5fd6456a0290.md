This can be demonstrated by a couple of simple instructions:

```
$ psql -U enrico forumdb

forumdb=> SELECT * FROM forum.users;
 pk | username | gecos | email
----+----------+---------------+---------------------
  1 | fluca1978 | Luca Ferrari | fluca1978@gmail.com
  2 | sscotty71 | Enrico Pirozzi | sscotty71@gmail.com
(2 rows)

forumdb=> UPDATE forum.users SET gecos = upper( gecos );
UPDATE 2

forumdb=> SELECT * FROM forum.users;
 pk | username | gecos | email
----+----------+---------------+---------------------
  1 | fluca1978 | LUCA FERRARI | fluca1978@gmail.com
  2 | sscotty71 | ENRICO PIROZZI | sscotty71@gmail.com
(2 rows)
```

As you can see, the user enrico has actually changed the name and surname of the existing users to a full uppercase string. Let’s now see what the other user can do:

```
$ psql -U luca forumdb

forumdb=> SELECT * FROM forum.users;
ERROR: permission denied for table forum.users
forumdb=> SELECT username, gecos FROM forum.users;
 username | gecos
----------+---------------
 fluca1978 | LUCA FERRARI
 sscotty71 | ENRICO PIROZZI
(2 rows)

forumdb=> UPDATE forum.users SET gecos = lower( gecos );
ERROR: permission denied for table forum.users
```