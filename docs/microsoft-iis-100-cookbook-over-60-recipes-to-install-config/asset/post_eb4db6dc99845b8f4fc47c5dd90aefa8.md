
4. Now, open the http://demo.learningpoint.co.in website:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_175_0.png)

Here, we are accessing the subdomain demo, and the main, or parent, domain is learningpoint.co.in. It has been created for demonstration purposes; the subdomain site may later be removed by the owner, in general, there is no guarantee that a subdomain will stay forever for reader access.

For example, consider a company that has several branches over the world. The company doesn't want to change the main domain name, so they decide to create a wildcard host or subdomain or child domain of the main domain. Let's see how we can use the main domain and subdomain.

The company name is **Learningpoint.co.in**. The main domain for the demo of the product section we have created is `demo.learningpoint.co.in`. For the UAE office, we can add the subdomain `uae.learningpoint.co.in`.

Wildcard hosts can be used in several ways, as per the company's requirements.

In the next recipe, we will learn how to create a wildcard host on IIS 10.0.

## How it works...

In this recipe, we have an overview of wildcard hosts, also called subdomains or child domains. We have a parent domain called `learningpoint.co.in`, and we created `demo.learningpoint.co.in`. We can use different names and create unlimited wildcard hosts for our parent domain. For a wildcard host, there is no need to pay any extra fees for perform registration. If you have a parent domain, you can create unlimited wildcard hosts.

We launched the URL `http://learningpoint.co.in`. We then got redirected to the home page of `learningpoint.co.in`. Next, we opened the URL `http://demo.learningpoint.co.in`. We were redirected to the home page of `demo.learningpoint.co.in`.

We also discussed the basics of DNS.

## Creating a wildcard host

In this recipe, we are going to create a wildcard host step by step on IIS 10.0. We will also create an HTTPS wildcard host and add an SSL certificate on it. We will edit the `hosts` file and make an entry for the created wildcard host.

Let's move to the following sections, where we will be covering this in detail.

## Getting ready

To step through this recipe, you will need a running IIS 10.0 instance. You should also have administrative privileges.

## How to do it...

- 1. Open Server Manager on Windows Server 2016. Click on the Tools menu and open IIS Manager.
- 2. Expand the IIS server (**WIN2016IIS**). We then get the **Application Pools** and **Sites** listed.
- 3. Expand the **Sites** option. You will get the list of existing websites: **mysite.com**, **v2mysite.com**, and **v4mysite.com**.

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_177_0.png)

- 4. Click on the Sites folder, go to the **Actions** panel, and you will get the **Add Website...** option. You can see this highlighted in the previous figure.

## Getting Your Wildcard Host Up and Running

15. Open the hosts file in Notepad using administrator rights. Make the entry highlighted in the following screenshot:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_181_0.png)

16. Now you are done with all the required entries for the domain `SSL.v2mysite.com` and IP `172.16.15.60`. Press the Tab button and type the domain name `SSL.v2mysite.com` and one more entries for domain `demo.v2mysite.com` and IP `172.16.15.60`.

## How it works...

In this recipe, we added a wildcard host called http://demo.v2mysite.com. Also, we added one more wildcard host with the HTTPS protocol, https://ssl.v2mysite.com. We then assigned it to the https://ssl.v2mysite.com wildcard host.

## Configuring a wildcard host

In this recipe, we are going to configure the wildcard host SSL.v2mysite.com for an application pool. We will implement the Default Document settings of the wildcard host, with the permissions of that wildcard host. We will also implement the basic and advanced settings of the wildcard host.

## Getting ready

To step through this recipe, you need a running IIS 10.0 instance. You should have already created wildcard hosts. You should also have administrative privileges.

## How to do it...

- 1. Log in to Windows Server 2016 and go to Server Manager.
- 2. Click on the Tools menu and open IIS Manager.
- 3. Expand the IIS server (WIN2016IIS) from the Connections panel. We get the listed Application Pools and Sites.
- 4. Expand the Sites folder, and you will see the listed sites--mysite.com, v2mysite.com, v4mysite.com--and you will get the wildcard hosts, ssl.v2mysite.com and demo.v2mysite.com:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_183_0.png)

- 5. Select demo.v2mysite.com to get the properties of the demo.v2mysite.com wildcard host from the Actions panel.
- 6. Go to the Actions panel, and you will see Edit Permissions. This is used to provide or deny access to your wildcard host's content directory. You can manage this permission from the Actions pane. You can go to the wildcard host's content directory and right-click on the directory to open the property's pop-up window and select the Security tab. Here, you can edit the access level and deny permission as well.
- 7. Now, we have the Bindings option; we already know how to change the bindings of any website. The following actions can be performed through the binding option: adding, removing, and editing the IP, port, protocol, host name, and SSL certificate binding for any website or wildcard host.
- 8. Basic Settings... is also the same as a website's basic settings, because a wildcard host is also a child website for the main website. Here, you can find more clear information about basic settings:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_184_0.png)

- 9. As shown in the previous screenshot, we have the Site name, Application pool, and Physical path of the wildcard host demo.v2mysite.com. We can set any directory as the physical path.
- 10. Let's have a look at the Advanced Settings now:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_184_1.png)

- 11. In **Advanced Settings**, just like the website settings, we have the application pool, website ID (number of websites you've created, also known as the unique order number of the site index), name of the wildcard host, physical path, and physical path credentials (if you have set them). We'll leave it at the defaults. The physical path credentials logon type can be clear text, network, batch, or interactive as per our configuration of physical path credentials. We are not using any credentials, so we'll use the default settings. **Preload Enabled** is used to keep the application pool always running.
- 12. In the **Behavior** section of **Advanced settings**, we have **Enabled protocols**, with the default option type **HTTP**, and the **Failed Request Tracing** option, which enables us to trace websites or wildcard host tracking logs. In **Limits**, we can apply settings to limit the number of users that can access a wildcard host at one time.
- 13. Let's move to the wildcard host, **ssl.v2mysite.com**. We've already configured bindings for it: HTTPS protocol and SSL certificate.
- 14. Go to IIS Manager and click on **ssl.v2mysite.com**. Go to the **Actions** panel and click on **Basic Settings...** You will get the **Basic Settings** window:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_185_0.png)

- 15. Let's verify the site application pool and wildcard host (subdomain) settings of **ssl.v2mysite.com**. The application pool is the same as the site name, as seen in the previous figure.

> TIP
> 
> It's my recommendation to verify the application pool manually for the .NET CLR version according to the hosted website's framework version.

- 16. Click on the Select button. You will get the application pool properties:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_186_0.png)

- 17. You can see that the ssl.v2mysite.com application pool version is 4.0. We can also select any other type of application pool available in IIS 10.0 on Windows Server 2016. You can check the list of available application pools by clicking on the Application pool dropdown, and you will get the list of application pools created in IIS 10.0.
- 18. Let's move further to check the Default Document settings. Click on any of the wildcard hosts we've created and go to the Features View. Find the default document, double-click on it to open it, and check for the filename default.aspx, which we are going to access in the wildcard ssl.v2mysite.com.
- 19. Optionally, select a Default Document in the list, and in the Actions pane, click on Move Up or Move Down to change the file's precedence.

## How it works...

In this recipe, we configured the wildcard host ssl.v2mysite.com for application pool version 4.0 and had an overview of the permissions of wildcard hosts and content directory folder. We implemented the basic and advanced settings of wildcard hosts. Finally, we implemented the Default Document settings of wildcard hosts.
