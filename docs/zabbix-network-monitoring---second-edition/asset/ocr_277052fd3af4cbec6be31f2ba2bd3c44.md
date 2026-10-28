# Encrypting Zabbix Traffic

You might have a certificate infrastructure in your organization, but for our first test, we will generate all required certificates ourselves. We will need a new **certificate authority (CA)** that will sign our certificate. Zabbix does not support self-signed certificates.

> It is strongly recommended to use intermediate certificate authorities to sign client and server certificates—we will not use them in the following simple example.

# Being our own authority

We'll start by creating the certificates in a separate directory. For simplicity's sake, let's do this on `A test host`—choose any directory where our certificate signing will happen.

The following is not intended to be a good practice. It is actually doing quite a few bad and insecure things to get the certificates faster. Do not follow these steps for any production setup.

```
```bash
$ mkdir zabbix_ca
$ chmod 700 zabbix_ca
$ cd zabbix_ca
```
```

Generate the root CA key:

```
```bash
$ openssl genrsa -aes256 -out zabbix_ca.key 4096
```
```

When prompted, enter a password twice to protect the key. Generate and self-sign the root certificate:

```
```bash
$ openssl req -x509 -new -key zabbix_ca.key -sha256 -days 3560 -out
zabbix_ca.crt
```
```

When prompted, enter the password you used for the key before. Fill in the values as prompted—the easiest might be supplying empty values for most except the country code and common name. The common name does not have to be anything too meaningful for our test, so using a simple string like zabbix_ca will suffice.

Now, on to creating a certificate we will use for the Zabbix server—first, let's generate a server key and **certificate signing request (CSR)**:

```
```bash
$ openssl genrsa -out zabbix_server.key 2048
$ openssl req -new -key zabbix_server.key -out zabbix_server.csr
```
```