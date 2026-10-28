# Backend libraries

Behind the scenes, Zabbix encryption can use one of three different libraries: OpenSSL, GnuTLS, or mbedTLS. Which one to choose? If using packages, the easiest and safest is to start with whichever the packages are compiled with. If compiling from source, choose the one that is easiest to compile with. In both cases, that is likely to be the library that is endorsed by the packagers and maintained well. The Zabbix team has made a significant effort to implement support for all three libraries in as similar a way as possible from the user perspective. There could be differences regarding support for some specific features, but those are likely to be more obscure ones—if such problems do come up later, switching from one library to another should be as easy as recompiling the daemons. While in most cases it would likely not matter much which library you are using, it's a good idea to know that—one good reason for supporting these three different libraries is also the ability to switch to a different library if the currently used one has a security vulnerability.

> These libraries are used in a generic manner, and there is no requirement to use the same library for different Zabbix components—it’s totally fine to use one library on the Zabbix server, another on the Zabbix proxy, and yet another with zabbix_sender.

In this chapter, we will try out encryption with Zabbix server and zabbix_sender first and then move on to encrypting agent traffic using both PSK and certificate-based encryption. If you have installed from packages, your server most likely already supports encryption. Verify that by looking at the server and agent startup messages:

```
TLS support:          YES
```

> One way to find out which library the binary has been compiled against would be to run ldd zabbix_server | egrep -i "ssl|tls"—replace the binary name as needed.

If you compiled from source and TLS support is not present, recompile the server and agent by adding one of these parameters: --with-openssl, --with-gnutls, or --with-mbedtls.