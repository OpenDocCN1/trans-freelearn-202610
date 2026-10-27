There are several versions of protocols used in web browsing, e-mail, internet faxing, instant messaging, and Voice over IP (VoIP). TLS has the following benefits:

+   - It encrypts information
+   - It provides authentication
+   - It accepts credit card payments on websites
+   - It protects against phishing
+   - It adds power to brands and improves customer trust

Information submitted on the Internet passes through more than one node in the network before reaching the final destination, so it can be obtained by a third party. A TLS certificate inserts random characters into the original information to change it beyond recognition so that only the proper encryption key can help decrypt it. Server certification is another type of protection issued when the server's owner obtains a TLS certificate. This certificate is available to the client to validate that the TLS certificate is up to date and the client's information is being delivered to the right place. Online businesses that use credit card payments must be in compliance with the Payment Card Industry standards. This means that the server needs a TLS certificate with the proper encryption of at least 128 bits. Online businesses often offer site seals and other brand images to indicate that a trusted encryption is in use. This information gives customers an added level of assurance and creates trust between the customer and the business.

# The TLS certificate

It is really complicated to decide at which level we can address the TLS protocol in the TCP/IP stack. The TLS security protocol describes how algorithms should be used and how the TLS certificate establishes a secure connection. To get a certificate, you must create a Certificate Signing Request (CSR) on your server. This process creates a pair of private and public keys on your server. Then, you must send the CSR datafile that contains the public key to a Certified Authority (CA) in order to obtain the TLS certificate. The CA creates a data structure from the CSR file to match a private key in the future. The CA never sees the private key and it can't be compromised. Once you receive the TLS certificate, you can install it on the server. Dart uses the Network Security Services (NSS) library of Mozilla to handle TLS. We need to use certutil, a certificate database tool from NSS Security Tools, to manipulate the certificate database. You can obtain the source code and quickly build certutil for your platform, but I have installed the following prebuilt version of the program on my Ubuntu workstation:

```
sudo apt-get install libnss3-tools
```