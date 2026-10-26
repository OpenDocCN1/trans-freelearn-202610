# Securing Your Websites on IIS 10.0

+   3. Click on the **Tools** menu from Server Manager, you will find IIS Manager. Open it and click on the **WIN2016IIS** IIS server. Expand the **Sites** folder and click on **mysite.com**. Go to the **Features View** of **mysite.com**, select **IP Address and Domain Restrictions**, and open the **IP Address and Domain Restrictions** settings, as shown here:

![](bbox=[0.105, 0.224, 0.895, 0.662])

+   4. You can see in the figure that we have already created an Allow rule for IP address `172.16.15.60` and Deny rule for IP address `172.16.15.65`. Let's test the IP `172.16.15.60`.