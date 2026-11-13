```
return new SSLOptions(ctx, this);
       }
   }
}
```

With the preceding code we are able the control the cipher suite used by our
SSLContext; however, in order to use our own private key, certificates, and
trusted CAs, we will have to to initialize the SSLContext with our own
TrustManager and KeyManager:

```
void init(KeyManager[] km, TrustManager[] tm, SecureRandom sr)
```

For simplicity, we will load our KeyManagers and TrustManager from a keystore
file stored on the application assets directory. The Java keystore is available on
the Packt Publishing website. So, before we go any further, download the
asynchronous_client.ks file from the Packet website with a ready-to-use
private key, a certificate, and the custom CA certificate that signed the certificate
as the trusted Authority.

# Note

Notice that you can build your own TrustManager and KeyManager custom
subclass that can load the private key and the certificates from a different source
but for simplicity we will load them from a file.

Let's take a look at our asynchronous_client.ks java keystore file with the
help of the keytool application.

On the command line, please run the following command to list the content of
the keystore:

```
keytool -list -v -keystore asynchronous_client.ks -storetype BKS
provider org.bouncycastle.jce.provider.BouncyCastleProvider
providerpath bcprov-jdk15on-146.jar
```

```
Alias name: asynchronous_client
Entry type: PrivateKeyEntry
Certificate[1]:
Owner: C=UK,ST=Birmingham,L=Birmingham,O=Packt Publishing,OU=Packt
Publishing,CN=asynchronous_client
Issuer: C=UK,...,CN=packt
Certificate[2]:
Owner: C=UK,...,CN=packt
```