
- 3. By default, all the website files will be stored in the `wwwroot` folder. The path is `C:\inetpub\wwwroot`. The following screenshot shows the default IIS 10.0 website files:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_51_0.png)

- 4. Now, we are going to see the `logs` folder. It is located at `C:\inetpub\logs`:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_51_1.png)

- 5. Start IIS Manager and select the **Default Web Site**. You will get the **Actions** window on the right-hand side. Inside the **Actions** list, we have the **Edit Site** option.

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_52_0.png)

- 6. Click on the **Basic Setting...** text button. You will get the following window for basic settings:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_52_1.png)

- 7. We selected the **Default Web Site**; now you can see the **Site name**, **Application pool**, **Physical path**, and **Connect as...** authentication option.

- 8. In the **Edit Site** window, you can change the **Application pool** and define your website folder path or default type of authentication. Finally, you can test the default settings.

- 9. Let's look at some binding options. In the **Actions** list, you will see that there is a **Bindings...** option. Bindings are used for defining your website parameters: **Type** `http`, **Host Name** `www.xyz.com`, **Port** `80`, and **IP Address** `*` for example, as shown here:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_53_0.png)

- 10. Now, set the **Limits** for website access. In the **Actions** list, which is available on the right-hand side of IIS Manager, go to the **Default Web Site** properties.

## Hosting static web page

In this recipe, we will host our simple HTML web page.

### Getting ready

We need to create an HTML web page and have IIS 10.0 running. You will also require user administrator rights for hosting the site.

### How to do it...

- 1. Open Notepad and type something, like the text shown in the following screenshot. You can write anything you want.
- 2. Save the Notepad file on your desktop with the name of index.html.

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_55_0.png)

- 3. Next, you have to open IIS Manager, select the Default Web Site, and right-click on it:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_56_0.png)

- 4. Click on the Explore option from the pop-up menu.

- 5. It will open the Inetpub folder from the C:\inetpub\wwwroot default website folder location, where you will have kept your web application or website files. The wwwroot folder is your hosting directory.

- 9. Click on **Default Web Site**. You will see the property page come up:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_58_0.png)

- 10. Open the **Default Document**. You will find Default.htm, Default.asp, Index.html, and many names listed. Our filename is Index.html, and the default document already has the same name.

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_59_0.png)

### How it works...

Once we check the Default Document configuration and find the index.html name already listed, we have to open the web page, and the default home page will open. The Default Document setting is very helpful for launching your website as you will select it and you need to tell IIS Server what your home page is.

## Testing a static-page website

We've already uploaded the static index.html page to the wwwroot folder, and we've checked the Default Document settings for the default home page. Now let's see how to test the static web page.

### Getting ready

We have to test whether the web page is working. You need an up-and-running IIS 10.0 server instance and a static HTML web page.

We have already created the `index.html` page and uploaded it in the IIS `wwwroot` directory.

### How to do it...

- 1. Open Internet Explorer.
- 2. You have to provide the URL of your web page in the address bar. We have IIS 10.0 installed on the localhost (local server).
- 3. Type the URL `http://localhost/` and press **Enter**. Your page will look like this:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_60_0.png)

### How it works...

Once you access your web page through IIS, the page is displayed through HTTP port 80. You can try uploading more pages.

The benefit of application pools is that if you have an issue with one of the application pools, it will only affect the associated application pool.

Application pools enable us to isolate our web application for better security, reliability, and availability.

Two types of application pipelines are available. We can select integrated mode (supports IIS 7.5 or later versions) or classic mode (IIS 6.0).

## Understanding application pools in IIS 10.0

In this recipe, we will understand application pools. We can simply say that the application pool is the heart of IIS 10.0. Application pools are logical groupings of web applications that will execute in a common process, thereby allowing greater granularity over which programs are clustered together in a single process. For example, if you require every web application to execute in a separate process, you simply go and create an application pool for each application of different framework versions.

Let's say that we have more than one version of a website, one that supports framework 2.0 and another one supporting framework 4.0 or some different application such as PHP or WordPress. All these website processes are managed through application pools.

### Getting ready

To step through this recipe, you will need a running IIS 10.0 instance. You should also have administrative privilege.

- 3. Click on **DefaultAppPool**, and then you will get the **Actions** panel of **DefaultAppPool**. Here, you will get the option **Application Pool Tasks** highlighted in the right-hand side, with which you may **Start**, **Stop**, and **Recycle** the services of IIS 10.0.

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_64_0.png)

- 5. In the **Edit Application Pool** section, you can change the settings of the application pool to **Basic Settings...** and **Advanced Settings...**, **Rename** the application pool, and also perform **Recycling....**

### How it works...

Let's take a look at what we explored in IIS Manager and application pools. We understood the basics of application pools and the properties in which we can get the changes we want made. The default IIS 10.0 application pool framework is v4.0, which is supported up to v4.6, but we have some more options for installing different versions of the application pool. We can easily customize the application pool, which helps us fulfill our typical web application requirements.

We have several options for application pools in the action pane. We can add a new application pool and we can start, stop, and recycle application pool tasks. We can perform editing and automated recycling as well.

In the next recipe, we will learn more about application pools to install lower framework versions.

## Installation of lower framework version

In this recipe, we are going to install framework 3.5 on Windows Server 2016. By default, IIS 10.0 has framework version 4.0. We will install the lower version of the framework, which supports web applications with .NET framework versions 2.0 through 3.5.

For example, if you have a web application you created a few years back and it was developed in the v2.0 .NET framework, you can use this recipe in order to make it run on IIS 10.0. Let's begin.

### Getting ready

To step through this recipe, you need to install framework v3.5, which is based on the v2.0 framework. You will need a Windows Server 2016 instance. You should have the Server 2016 OS media for the framework 3.5 files or an Internet connection. You also need admin privileges.

### How to do it...

- 1. Open Server Manager on Windows Server 2016, and click on the highlighted Add roles and features option.

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_66_0.png)
