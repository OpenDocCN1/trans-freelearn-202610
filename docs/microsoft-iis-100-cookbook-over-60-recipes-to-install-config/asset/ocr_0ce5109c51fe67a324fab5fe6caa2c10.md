# How it works...

In this recipe, we checked www.microsoft.com and saw it uses **Hyper Text Transfer Protocol Secure (HTTPS)**. We also checked the SSL certificate of the website. Finally, we opened the certificate properties and saw the details of the website and certificate provider.

# Installing SSL

In this recipe, we are going to open IIS Manager on the WIN2016IIS server and create a certificate request. We will also see how we can import a third-party SSL certificate.

# Getting ready

To step through this recipe, we are going to create an SSL certificate request for a certificate provider. Also, we will see how we can import/install the certificate on IIS. You will need a running IIS 10.0 server and an administrator account, which will be used to make changes to IIS 10.0.

# How to do it...

+   1. Go to **WIN2016IIS** and log in to IIS 10.0 Server. Open Server Manager and go to the **Tools** menu.
+   2. Open IIS Manager and click on the **WIN2016IIS** IIS server, as shown here: