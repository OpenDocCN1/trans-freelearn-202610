# How it works...

**SSL** and **TLS** are protocols that secure a specific application, for example, HTTP, SMTP, Telnet, and others. SSL Versions 1, 2, and 3 were developed by Netscape in the mid 90s for their Navigator browser, while TLS is a standard from the IETF (`RFC 2246`, `RFC 4492`, `RFC 5246`, `RFC 6176`, and others). TLS 1.0 was first introduced in `RFC 2246` in January 1999 as an upgrade of the SSL Version 3.0 (third paragraph at http://tools.ietf.org/html/rfc2246).

The TLS handshake protocol involves the following procedures for establishing a TLS connection:

+   1. Exchange hello messages to agree on the algorithms to work with, and exchange random values for the key generation.
+   2. Exchange the necessary cryptographic parameters to allow the client and the server to agree on a premaster secret key.
+   3. Exchange certificates and cryptographic information to allow the client and server to authenticate each other.
+   4. Generate a master secret key from the premaster secret and exchanged random values.
+   5. Allow the client and server to verify that their peer has calculated the same security parameters and that the handshake occurred without being tampered with by an attacker.

These procedures are performed in the following order:

+   1. Select cryptographic algorithms:
+      - The **Client Hello** message (marked as **1** in the following screenshot)
+      - The **Server Hello** message (marked as **2** in the following screenshot)
+   2. Authenticate the server and exchange key (marked as **3** in the following screenshot).
+   3. Authenticate the client and exchange key (marked as **4** in the following screenshot).
+   4. Complete the handshake (marked as **5** in the following screenshot).