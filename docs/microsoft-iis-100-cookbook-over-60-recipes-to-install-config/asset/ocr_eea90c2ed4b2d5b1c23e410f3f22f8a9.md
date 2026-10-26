# How it works...

Multiple website hosting on the same IIS 10 server helps us easily manage all the website-related tasks on the same server. We have just created three websites on the same IIS server. You can create many websites as per your requirements.

A hosts file entry is required when you want to get access to the website with the domain name, for example, mysite.com. You can also use the DNS server to access this website through LAN or WAN.

# Creating a website folder

In this recipe, we are going to create a separate website directory for each website, for example, mysite, v2mysite, and v4mysite, inside the c:\inetpub\wwwroot path, where we can upload the website application files later in the chapter. We were using the default wwwroot folder to upload the website files, so now we have to publish more than one website. We have to upload all the web files in their own separate directory.

# Getting ready

To step through this recipe, you will need a running IIS 10 instance. You must have administrator privileges, which you need to create the website folders.

# How to do it...

We are going to create a website folder inside the wwwroot folder for each website.

+   1. Open the file explorer, and click on **This PC**. On the right hand side, you will see your available drives.
+   2. Open the c:\ drive, go to the folder name inetpub, and you will get the list of IIS 10.0-related folders. You need to find the wwwroot folder.