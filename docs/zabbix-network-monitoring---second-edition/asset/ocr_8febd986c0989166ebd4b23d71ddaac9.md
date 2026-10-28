# Managing users

We saw how managing users was an easy task using the frontend. But what if you have forgotten the password? What if some remote installation of Zabbix is administered by local staff, and the only Zabbix super admin has left for a month-long trip without a phone and nobody else knows the password? If you have access to the database, you can try to solve such problems. Let's find out what exactly Zabbix stores about users and how. In the MySQL console, execute this:

```
mysql> select * from users limit 2;
```

This way, we are listing all data for two users at the most:

| MariaDB [zabbix]> select * from users limit 2; |
|---|---|---|---|---|
| userid | alias | name | surname | passwd |
| 1 | Admin | Zabbix | Administrator | 5fce1b3e34b520afeffb37ce08c7cd66 |
| 2 | guest |  |  | d41d8cd98f00b204e9800998ecf8427e |
| 2 rows in set (0.00 sec) |

> The example output is trimmed on the right-hand side and fewer than half of the original columns are shown here. You can also replace the trailing semicolon in the SQL query with \G to obtain vertical output, like this: select * from users limit 2 \G.

That's a lot of fields. We'd better find out what each of them means:

| Field | Usage |
|---|---|
| userid | Quite simple, it's a unique, numeric ID. |
| alias | More commonly known as a username or login name. |
| name | User's name, usually their given name. |
| surname | This surely can't be anything else but the surname. |
| passwd | The password hash is stored here. Zabbix stores MD5 hashes for authentication. |
| url | The after-login URL is stored in this field. |
| autologout | Whether auto-logout for this user is enabled. Non-zero values indicate timeout. |
| lang | The language for the frontend. |
| refresh | The page refresh in seconds. If zero, page refresh is disabled. |
| theme | The frontend theme to use. |
| attempt_failed | How many consecutive failed login attempts there have been. |