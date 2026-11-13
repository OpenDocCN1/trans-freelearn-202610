```
KeyManagerFactory kmf = getKeyManagerFactory(ctx);
TrustManagerFactory tmf = getTrustManagerFactory(ctx);
// Use the cipher suite defined SSL, SSLv3 , TLS, TLSv1,
SSLContext result = SSLContext.getInstance(
 cipherSuite.toString()));

result.init( kmf != null ? kmf.getKeyManagers() : null ,
 tmf != null ? tmf.getTrustManagers() : null,
 new SecureRandom());
return result;
}
}
```

Notice that we initialize SSLContext using the Keymanager list and
TrustManager list returned by KeyManagerFactory and TrustManagerFactory
respectively. Hence, the next step is to write the code member method to acquire
our factories. So, let's start with the getKeyManagerFactory:

```
KeyManagerFactory getKeyManagerFactory(Context ctx)
throws Exception {
KeyManagerFactory kmf = null;
// Initialize Key store
if ( keyStore != null ) {
// Load the file keystore from the assets directory
InputStream keyStoreIs = ctx.getResources().
 getAssets().open(keyStore);

String algorithm = KeyManagerFactory.getDefaultAlgorithm();
kmf = KeyManagerFactory.getInstance(algorithm);

// Create BouncyCastle Key Store
KeyStore ks = KeyStore.getInstance("BKS");

// Load the Keymanagers available on the file using
// a password
ks.load(keyStoreIs, keyStorePassword.toCharArray());
kmf.init(ks, keyStorePassword.toCharArray());
}
return kmf;
}
```

The previous code will load up a public and private key from a BCS keystore
that we previously prepared. So, all that remains is the
getTrustManagerFactory function used to load our trusted CA:

```
TrustManagerFactory getTrustManagerFactory(Context ctx)
 throws Exception f
```