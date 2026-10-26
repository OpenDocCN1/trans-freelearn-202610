+   6. We have the IIS 10 server, which has the IP address `172.16.15.60`. We are running two websites, `mysite.com` and `v2mysite.com`, on IP `172.16.15.60`. We added a dedicated IP `172.16.15.61` for the website `v4mysite.com`, and in the same IIS server, you can configure multiple IP address to use specific IPs for individual sites.

# How it works...

In this recipe, we tested the website mysite.com with **DefaultAppPool** in steps 1 and 2. In step 3 and 4, we tested website v2mysite.com with the default landing page `Demo.html`, which was configured under the Default Document through IIS Server Manger. In steps 5 and 6, we tested the website v4mysite.com with dedicated IP `172.16.15.61`.