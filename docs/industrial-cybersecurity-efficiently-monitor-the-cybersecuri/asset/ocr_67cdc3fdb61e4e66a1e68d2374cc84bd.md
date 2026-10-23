```
# cd Very-secret-files
# ls
drw-rw-rw-      0 Sat Aug 29 16:34:34 2020 .
drw-rw-rw-      0 Sat Aug 29 16:34:34 2020 ..
-rw-rw-rw-      0 Sat Aug 29 16:34:34 2020 company-passwords.txt
# get company-passwords.txt
# exit
pac@KVM0101011:~$ ls
total 1.9M
-rw-r--r-- 1 pac 0 Aug 29 16:46 company-passwords.txt
...
```

Dissecting this command, we use smbclient.py to connect to 172.25.100.220 using the guest account (guest@172.25.100.220). We omit sending a password with -no-pass.

In the preceding output, after we logged on to the 172.25.100.220 SMB service with the smbclient.py script, we enumerated the shares that had been configured (shares). Next, we connected to the shared folder (use Shared), performed a directory listing (ls), changed directories to the Very-secret-files directory (cd Very-secret-files), and downloaded the company-passwords.txt file (get company-passwords.txt). We then exited the smbclient.py script (exit) and listed the directory contents of the local (Kali machine) directory to verify that we got the goods.

Full disclosure, the file was a dud:

```
pac@KVM0101011:~$ cat company-passwords.txt
nice try
```

This exercise did not yield us any sensitive files, but it did show us that a simple misconfiguration opens the doors for someone to collect potentially sensitive data via the SMB shares that are over-exposed.

Next, we will look at finding out the OS version via SMB.