The -s command option creates an individual certificate and adds it to a certificate database. The text after -s option provides a subject that identifies an owner of certificate. The -x option tells the certutil command that the created certificate is self-signed. The -v option sets the number of months for which a new certificate will be valid. The -m option sets a unique serial number to the certificate being created. When we run the certutil command, it asks us to press the keys on the keyboard to create a random seed that will be used in the creation of our key.

3. We now have a CA certificate and need to generate the key and certificate signing request. Let's do that with the following command:

```
certutil -R -s "CN=localhost, O=Mastering Dart, L=Cape Town, ST=WC, C=CA" -p "+27 21 1234567" -o mycert.req -d sql:/home/akserg/Project/server/bin/pkcert
```

The -R command option creates a certificate request file that can be submitted to a CA to be processed into a finished certificate. We specify the subject to identify the certificate owner; in this case, it's me. Extra information such as your telephone number can be an input as well. Output defaults to the output file marked with the -o option. When we run the certutil command, it asks for a password, and we can generate the key with a random seed again.

4. Now, we can see the list of keys in the database with the following command:

```
certutil -K -d sql:/home/akserg/Project/server/bin/pkcert
```

The result will be as follows:

```
< 0> rsa e22c881d9eb382ea69257410cf464dfedcd49354   NSS Certificate DB:CACert
< 1> rsa b909266e0d5a14523158bfc7903ea9460fad2da6   (orphan)
```

5. We need to sign in the key with the following command:

```
certutil -C -m 2345 -i mycert.req -o mycert.crt -c CACert -d sql:/home/akserg/Project/server/bin/pkcert
```

6. Finally, it's time to add a certificate to the database with the following command:

```
certutil -A -n localhost_cert -t "p,p,p" -i mycert.crt -d sql:/home/akserg/Project/server/bin/pkcert
```

The name of our certificate is localhost_cert after the -n option.