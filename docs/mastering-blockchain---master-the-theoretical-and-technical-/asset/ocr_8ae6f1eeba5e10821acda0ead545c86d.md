```
prime1:
00:d4:61:5d:38:c0:d4:84:8c:4c:bd:b5:82:74:cd:
aa:f2:0c:07:2f:77:f5:46:db:f5:3f:ea:a8:90:19:
a8:c1:79:2a:52:aa:81:04:4a:28:1b:ba:c2:a4:bb:
ec:15:b5:16:20:59:61:28:98:bf:d7:e7:cb:7e:2b:
4d:5e:30:c6:59
```

```
prime2:
00:c3:a3:d4:47:d6:0b:79:bb:26:fd:28:e2:88:e4:
a1:57:80:0a:b8:57:62:ee:de:c3:61:61:39:52:56:
f5:ba:a3:ff:8b:6f:33:37:fc:ad:b7:80:57:28:8b:
3f:29:c1:72:91:01:9e:00:2e:8b:0c:76:72:c9:cc:
9c:9e:d0:c8:39
```

Similarly, the public key can be explored using the following commands. Public and Private keys are base64-encoded:

```
~/Crypt$ openssl pkey -in publickey.pem -pubin -text
-----BEGIN PUBLIC KEY-----
MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQC iThQcz8trzn enpgZP1Bq8w8u0
74A2J2MQX4hCSH f6/hMjLfxdeJNnw0zlcMV2108YkIA/gSronsv+vfYzZ66lv8cv
6aHAzL6G4/EAp4BJ7TnTaFUmjfrdH/Z0TN41TPeJs9/wv7+L176GnD8j5K/9I4SF
dMKArq1xk4IYkiGx0QIDAQAB
-----END PUBLIC KEY-----
Public-Key: (1024 bit)
Modulus:
00:a2:4e:14:1c:cf:cb:6b:ce:77:a7:a6:06:4f:d4:
1a:bc:c3:cb:b4:ef:80:36:27:63:10:5f:88:42:48:
77:fa:fe:13:23:2d:fc:5d:78:93:67:c3:4c:e5:70:
c5:76:97:4f:18:90:80:3f:81:2a:e8:9e:cb:fe:bd:
f6:2b:67:ae:a5:bf:c7:2f:e9:a1:c0:cc:be:86:e3:
f1:00:a7:80:49:ed:39:d3:68:55:26:8d:fa:dd:1f:
f6:74:4c:de:25:4c:f7:89:b3:df:f0:bf:bf:8b:d7:
be:86:9c:3f:23:e4:af:fd:23:84:85:74:c2:80:ae:
ad:71:93:82:18:2a:21:b1:d1
Exponent: 65537 (0x10001)
```

Now the public key can be shared openly and anyone who wants to send us a message can use the public key to encrypt the message and send it to us. We can then use the corresponding private key to decrypt the file.

# How to encrypt and decrypt using RSA with OpenSSL

First in the section, an example will presented, which demonstrates how encryption can be performed using RSA.