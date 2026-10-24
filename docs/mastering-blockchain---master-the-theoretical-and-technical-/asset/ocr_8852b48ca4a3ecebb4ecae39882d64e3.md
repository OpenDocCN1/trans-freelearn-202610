Now suppose a file named testsign.txt needs to be signed and verified. This can be achieved as follows:

+   1. Create a test file:

```
~/Crypt$ echo testing > testsign.txt
~/Crypt$ cat testsign.txt
testing
```

+   2. Run the following command to generate a signature using a private key for the testsign.txt file:

```
~/Crypt$ openssl dgst -ecdsa-with-SHA1 -sign eccprivatekey.pem testsign.txt > ecsign.bin
```

+   3. Finally, the command for verification can be run as shown here:

```
~/Crypt$ openssl dgst -ecdsa-with-SHA1 -verify eccpublickey.pem -signature ecsign.bin testsign.txt
Verified OK
```

A certificate can also be generated using the private key generated earlier:

```
~/Crypt$ openssl req -new -key eccprivatekey.pem -x509 -nodes -days 365 -out ecccertificate.pem
You are about to be asked to enter information that will be incorporated into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:GB
State or Province Name (full name) [Some-State]:Cambridge
Locality Name (eg, city) []:Cambridge
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Dr.Equinox!
Organizational Unit Name (eg, section) []:NA
Common Name (e.g. server FQDN or YOUR name) []:drequinox
Email Address []:drequinox@drequinox.com
```