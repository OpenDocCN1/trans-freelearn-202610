# 12
Security Aspects

In this chapter, we will talk about different aspects of security. You will learn about validation of user data input and security best practices in our web application. This chapter covers the following topics:

+   - Web security
+   - Securing a server
+   - Securing a client
+   - Security best practices

# Web security

Crime is a disease that plagues the minds of many individuals. Hackers are interested in everything from personal mailbox credentials to bank account details. The responsibility of maintaining security lies with web developers. Developers should use HTTPS to access web pages and resources with sensitive data.

# Transport Layer Security and Secure Socket Layer at a glance

Secure Socket Layer (SSL) is one of the most common protocols in use on the Internet today. SSL is capable of securing any transmission over TCP. The Transport Layer Security (TLS) protocol is a successor of SSL and is based on the older SSL specifications. TLS Version 1.0 was defined for the first time in January 1999 as an upgrade of SSL Version 3.0. Both of them are based on asymmetric keys to encrypt data and digital certificates for authentication through an untrusted third party. We use TLS in a client-server model, but the client usually does not provide a certificate. Instead, the server is responsible for its own authentication through signed certificates and encryption via public and private keys.