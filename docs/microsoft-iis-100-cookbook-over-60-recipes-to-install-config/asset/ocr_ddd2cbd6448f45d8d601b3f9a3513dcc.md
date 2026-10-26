You need a domain name, IP address, and port to run your website or web application.

We have already performed the installation and created application pools. Now let's start multiple web hosts on the same IIS 10 server and access the websites using their host names (domain names).

# Hosting multiple websites

In this recipe, we will create three websites in IIS 10.0: mysite.com, v2mysite.com, and v4mysite.com. We will make the hostname entries in the `hosts` file later in this chapter. We will access the website with its hostname.

Let's move on to the next section, where we will be covering this in detail.

# Getting ready

To step through this recipe, you will need a running IIS 10 instance and administrator user account. You are going to create and configure the website's properties.

# How to do it...

+   1. Open Server Manager in Windows Server 2016. Click on the **Tools** menu and open IIS Manager.
+   2. Expand the IIS server from the **Connections** panel. We get the listed **Application Pools** and **Sites** folder.
+   3. Expand the **Sites** menu in the left-hand side panel under the computer name and click on **Default Web Site**. You can find it using this figure: