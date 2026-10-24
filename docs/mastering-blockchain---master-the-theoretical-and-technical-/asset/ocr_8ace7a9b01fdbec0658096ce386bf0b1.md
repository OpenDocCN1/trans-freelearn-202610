*Xp != r mod q means invalid signature*

Various practical examples are shown here, which shows how the RSA digital signature can be generated, used, and verified using OpenSSL.

# How to generate a digital signature

The first step is to generate a hash of the message file:

```
:~/Crypt$ openssl dgst -sha256 message.txt
SHA256(message.txt)=
eb96d1f89812bf4967d9fb4ead128c3b787272b7be21dd2529278db1128d559c
```

Both hash generation and signing can be done in a single step, as shown here. Note that `privatekey.pem` is generated in the steps provided previously:

```
:~/Crypt$ openssl dgst -sha256 -sign privatekey.pem -out signature.bin message.txt
```

Now let's display the directory showing relevant files:

```
:~/Crypt$ ls -ltr
total 36
-rw-rw-r-- 1 drequinox drequinox 14 Sep 21 05:54 message.txt
-rw-rw-r-- 1 drequinox drequinox 32 Sep 21 05:57 message.bin
-rw-rw-r-- 1 drequinox drequinox 45 Sep 21 06:00 message.b64
-rw-rw-r-- 1 drequinox drequinox 32 Sep 21 06:16 message.ptx
-rw-rw-r-- 1 drequinox drequinox 916 Sep 21 06:28 privatekey.pem
-rw-rw-r-- 1 drequinox drequinox 272 Sep 21 06:30 publickey.pem
-rw-rw-r-- 1 drequinox drequinox 128 Sep 21 06:43 message.rsa
-rw-rw-r-- 1 drequinox drequinox 14 Sep 21 06:49 message.dec
-rw-rw-r-- 1 drequinox drequinox 128 Sep 21 07:05 signature.bin
:~/Crypt$ cat signature.bin
V_... [binary output redacted] ...
$ x... $:~/Crypt$
```