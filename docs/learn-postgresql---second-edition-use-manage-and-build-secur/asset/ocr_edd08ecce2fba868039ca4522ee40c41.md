```
```text
[....]

completed successfully (2868ms)
```
```

If everything is OK, we will receive a completed successfully message (as seen above); now we are ready to manage continuous backup.

# Managing base backups

As we previously mentioned, pgbackrest is able to handle full backups, differential backups, and incremental backups with a simple command-line statement.

To create a full base backup, we can do this:

```
```shell
postgres@pgbackrest:~$ pgbackrest --stanza=pg1 --type=full backup
```
```

When we press the *Enter* key on the keyboard, if everything is OK, we get this message:

```
```text
2023-07-11 08:42:58.125 P00   INFO: expire command end: completed
successfully (46ms)
```
```

Now, if we want information about our repository, we can use the info command as follows:

```
```text
postgres@pgbackrest:~$ pgbackrest --stanza=pg1 info
stanza: pg1
    status: ok
    cipher: aes-256-cbc

    db (current)
        wal archive min/max (15): 000000010000000000000001/00000001000000
00000004

        full backup: 20230711-084245F
                timestamp start/stop: 2023-07-11 08:42:45 / 2023-07-11
08:42:57
                wal start/stop: 000000010000000000000004 /
000000010000000000000004
                database size: 22.0MB, database backup size: 22.0MB
                repo1: backup set size: 2.9MB, backup size: 2.9MB
```
```

The info command tells us about WAL segments, the full backup start time, the original database size, and the repository backup size.