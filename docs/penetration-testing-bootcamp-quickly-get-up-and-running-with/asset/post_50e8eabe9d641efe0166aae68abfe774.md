
## John the Ripper

John the Ripper is a fast and feature-rich password cracking utility that is designed to work on many different hash types and systems. This is why it is such a widely used utility.

Another cool little feature of John the Ripper is that it also contains some notification utilities that are designed to notify users of either their weak password or the fact that John the Ripper has actually cracked it. This information is great for a penetration tester to include in the final penetration report to the stakeholders. But enough with the features; let's see how we can use this utility.

The first task you should perform is to look at the main page for John the Ripper. There are lots of command-line switches that allow you to change the functionality of the utility depending on the type of system you are looking to crack. Here is the syntax of John the Ripper:

```
john [OPTIONS] [PASSWORD-FILES]
```

Here is a list of the various options:

```
--single=[SECTION]          "single crack" mode
--wordlist=[FILE] --stdin   wordlist mode, read words from FILE or stdin
--pipe                      like --stdin, but bulk reads, and allows rules
--loopback=[FILE]           like --wordlist, but fetch words from a .pot file
--dupe-suppression          suppress all dupes in wordlist (and force preload)
--prince=[FILE]             PRINCE mode, read words from FILE
--encoding=NAME             input encoding (eg. UTF-8, ISO-8859-1). See also doc/ENCODING and --list=hidden-options.
--rules=[SECTION]           enable word mangling rules for wordlist modes
--incremental=[MODE]        "incremental" mode [using section MODE]
--mask=MASK                 mask mode using MASK
--markov[=OPTIONS]          "Markov" mode (see doc/MARKOV)
--external=MODE             external mode or word filter [not available]
--stdout[=LENGTH]           just output candidate passwords [cut at LENGTH]
--restore=[NAME]            restore an interrupted session [called NAME]
--session=NAME              give a new session the NAME
--status=[NAME]             print status of a session [called NAME]
--make-charset=FILE         make a charset file. It will be overwritten
--show[=LEFT]               show cracked passwords [if =LEFT, then uncracked]
--test[=TIME]               run tests and benchmarks for TIME seconds each
--users=[-]LOGIN|UID|...    [do not] load this (these) user(s) only
--groups=[-]GID|...         load users [not] of this (these) group(s) only
--shells=[-]SHELL|...       load users with[out] this (these) shell(s) only
--salts=[-]COUNT[:MAX]      load salts with[out] COUNT [to MAX] hashes
--save-memory=LEVEL         enable memory saving, at LEVEL 1..3
--node=MIN[-MAX]/TOTAL      this node's number range out of TOTAL count
--fork=N                    fork N processes
--pot=NAME                  pot file to use
--list=WHAT                 list capabilities, see --list=help or doc/OPTIONS
--format=NAME               force hash of type NAME. The supported formats can be seen with --list=formats and --list=subformats
```

I will be taking the shadow and passwd files that I got from the Meterpreter on that Linux host I compromised in the last chapter and will try to crack the passwords via John the Ripper. Here are my steps in the lab:

1. I need to combine the shadow and passwd files into one file:

```
root@pi-kali:~/META# ls
passwd shadow
root@pi-kali:~/META# ls -al
total 16
drwxr-xr-x 2 root root 4096 Feb 15 19:45 .
drwx------ 20 root root 4096 Feb 15 18:53 ..
-rw-r--r-- 1 root root 1581 Feb 15 18:48 passwd
-rw-r--r-- 1 root root 1207 Feb 15 19:45 shadow
root@pi-kali:~/META# unshadow passwd shadow > server-META.txt
```

2. Then, with that file, I can start running John the Ripper against it. Since I didn't specify any hash types, it will try and auto detect them. I will also not be specifying which mode to run in, so it will try the single crack mode first, the wordlist mode next, and finally incremental mode:

```
root@pi-kali:~/META# john server-META.txt
Warning: detected hash type "md5crypt", but the string is also recognized as "aix-smd5"
Use the "--format=aix-smd5" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 7 password hashes with 7 different salts (md5crypt, crypt(3) $1$ [MD5 32/32 X2])
Press 'q' or Ctrl-C to abort, almost any other key for status
postgres      (postgres)
user          (user)
msfadmin      (msfadmin)
service       (service)
123456789     (klog)
batman        (sys)
```

3. When completed, or during the crack (if it's still running and taking a long time), you can check to see if any of the passwords have been cracked. You just need to specify the -show command to have John the Ripper output the current password cracks. Here is a screenshot that shows the cracked accounts on my compromised Linux box:

```
root@pi-kali:~# john --show META/server-META.txt
sys:batman:3:3:sys:/dev/sh
klog:123456789:103:104::/home/klog:/bin/false
msfadmin:msfadmin:1000:1000:msfadmin,,,:/home/msfadmin:/bin/bash
postgres:postgres:108:117:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash
user:user:1001:1001:just a user,111,,:/home/user:/bin/bash
service:service:1002:1002:,,,:/home/service:/bin/bash

6 password hashes cracked, 1 left
```

As you can see, John the Ripper makes it very easy to crack passwords with little input needed from the user. There are some more advanced things you can do as well when it comes to searching the cracked password using some of the command-line switches to help make it easier on large password files. For example, you can search by UID, GID, or even shells. In the following screenshot, I wanted to see all the cracked users that were using a bash shell:

```
root@pi-kali:~# john --show --shells=bash META/server-META.txt
msfadmin:msfadmin:1000:1000:msfadmin,,,:/home/msfadmin:/bin/bash
postgres:postgres:108:117:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash
user:user:1001:1001:just a user,111,,,:/home/user:/bin/bash
service:service:1002:1002,,,:/home/service:/bin/bash
4 password hashes cracked, 1 left
```

![](img/095df2cd1930baecbcecaa51ee3417aa_146_0.png)

Depending on how powerful the hardware on which you are running any password cracking utilities is, the time it takes to crack passwords can vary. If you have a less powerful system internally, like in my case a Raspberry Pi, I will just use that to extract the necessary files, and then run the cracking software on my C&C server.

## THC-Hydra

Hydra is a great password cracking tool that can run on a Kali system running on a desktop machine, or even Kali running on the Raspberry Pi. It allows you to run cracks in parallel. This allows the application to scale quicker, and really get as many attempts in per minute as possible. You can control this number as well, and Hydra will notify you when you need to change this number when you go to run the crack. This is somewhat dependent on the service type you are trying to crack the password on as well.

Here is the syntax for THC-Hydra:

```
hydra [[[-l LOGIN|-L FILE] [-p PASS|-P FILE]] | [-C FILE]] [-e nsr] [-o FILE] [-t TASKS] [-M FILE [-T TASKS]] [-w TIME] [-W TIME] [-f] [-s PORT] [-x MIN:MAX:CHARSET] [-SouvVd46] [service://server[:PORT][/OPT]]
```

Here are the options:

- -l LOGIN or -L FILE  login with LOGIN name, or load several logins from FILE
- -p PASS  or -P FILE  try password PASS, or load several passwords from FILE
- -C FILE  colon separated "login:pass" format, instead of -L/-P options
- -M FILE  list of servers to attack, one entry per line, ':' to specify port
- -t TASKS  run TASKS number of connects in parallel (per host, default: 16)
- -U service module usage details
- -h more command line options (COMPLETE HELP)
server - the target: DNS, IP or 192.168.0.0/24 (this OR the -M option)
service - the service to crack (see below for supported protocols)
OPT - some service modules support additional input (-U for module help)

When Hydra is done with its checks and has been successful, you will see the following screen. In my lab, I was able to crack the msfadmin password for SSH on that particular host:

![](img/095df2cd1930baecbcecaa51ee3417aa_147_0.png)

When hydra is done with its checks, you will see the following screen if it was successful. In my lab, I was able to crack the msfadmin password for SSH on that particular host:

![](img/095df2cd1930baecbcecaa51ee3417aa_147_1.png)

![](img/095df2cd1930baecbcecaa51ee3417aa_147_2.png)

Depending on the size of the wordfile and how many tries per minute Hydra will use, this process can take a long time. In my one example, it would have taken almost 1 week to get through the whole list of about 1.2 million words. So plan accordingly. Sometimes having a smaller wordlist of typically used passwords is a good option to do a quicker scan, and grab user accounts that are using those bad passwords. Then, when time permit or at the same time, run the longer scans. My smaller wordlist scan took less than 1hr and grabbed the preceding password.

Hydra is a great tool because it is easy, and has the ability to attack other services. The latest list of services as of this release includes TELNET, FTP, HTTP, HTTPS, HTTP-PROXY, SMB, SMBNT, MS-SQL, MYSQL, REXEC, irc, RSH, RLOGIN, CVS, SNMP, SMTP, SOCKS5, VNC, POP3, IMAP, NNTP, PCNFS, XMPP, ICQ, SAP/R3, LDAP2, LDAP3, Postgres, Teamspeak, Cisco auth, Cisco enable, AFP, Subversion/SVN, Firebird, LDAP2, Cisco AAA.
