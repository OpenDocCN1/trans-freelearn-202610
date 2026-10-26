
## Uploading a .NET web page

In this recipe, we are going to upload website application pages. Website application pages are developed in .NET framework version 4. These website application pages will run from the VDapplication pool virtual directory.

The website application pages have been developed in .NET framework version 2.0 for the virtualdirectory virtual directory.

### Getting ready

To step through this recipe, you will need a running IIS 10.0 instance with a virtual directory created. You should also have administrative privileges.

### How to do it...

- 1. Open Server Manager on Windows Server 2016. Click on the Tools menu and open IIS Manager.
- 2. Expand the IIS server (WIN2016IIS). You will get all listed Application Pools and Sites. You will get three websites listed: mysite.com, v2mysite.com, and v4mysite.com. We have already created them.

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_145_0.png)

- 7. Copy your version 4.0 website application pages and paste them in the C:\WebSites\VDApplication\ folder.
- 8. Now we have to click on virtualdirectory and go to the Actions panel. Click on Explore, and paste them in the version 2.0 website application pages folder:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_145_1.png)

- 9. Close both the virtual directory and the physical folder.
- 10. Open IIS Manager, expand mysite.com, and click on the VDapplication virtual directory. Go to the feature view, open the Default Document, and add `default.aspx` to the top. Repeat the same steps for virtualdirectory.

### How it works...

We have two virtual directories, VDapplication and virtualdirectory, already created. We uploaded the version 4.0 website application files to the VDapplication virtual directory and the version 2.0 website application files to virtualdirectory. We also made the Default Document entry for our default landing page.

## Testing the uploaded web page

In this recipe, we are going to test web application pages developed in framework version 4 and uploaded into our custom virtual directory VDapplication pool. Web application pages for the lower version, framework 2.0, have been uploaded to the virtualdirectory virtual directory.

We will also test a normal HTML page using a custom virtual directory.

### Getting ready

To step through this recipe, you will need a running IIS 10.0 instance with a virtual directory created as well as a website application.

### How to do it...

- 1. Open Server Manager on Windows Server 2016. Click on the Tools menu and open IIS Manager.
- 2. Expand the IIS server (WIN2016IIS). You will get a list of Application Pools and Sites under the Connections pane. You will get three websites listed, which we uploaded in the previous recipe: mysite.com, v2mysite.com, and v4mysite.com.
- 3. Go to the Sites folder, expand mysite.com, and click on virtualdirectory. This is a simple virtual directory with the default application pool.
- 4. Go to the **Actions** panel and click on **Browse**. You will get the `http://mysite.com/virtualdirectory` URL:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_147_0.png)

- 5. Now let's test both custom virtual directory sites we had created in previous steps for **VDapplication** and **virtualdirectory**. You can access both site URLs through any browser, but in my case I used Internet Explorer.
- 6. Open Internet Explorer and type the URL `http://mysite.com/vdapplication` in the address bar. You will get the website application:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_147_1.png)

## 5. Installing HTTP/2 on IIS 10.0

In this chapter, we will cover the following recipes:

- Understanding HTTP/2
- Installing HTTP/2 on IIS 10.0
- Configuring HTTP/2 on IIS 10.0
- Uploading .NET web pages
- Testing uploaded web pages

### Introduction

HTTP/2 is the newer version of HTTP/1.1, used to reduce the impact of latency and connection load on web servers by semantic flow over TCP connections. The HTTP/2 protocol can work on Windows 10 and Windows Server 2016. Windows 10 and Server 2016 both have the latest version of IIS Server, IIS 10.0. Currently, only IIS 10.0 supports HTTP/2. In HTTP/2, a persistent connection can be used to provide services to multiple simultaneous requests.

We know that every TCP connection requires a round trip to set up a connection between server and client. HTTP/2 sharply reduces the need for a request to wait while a new connection is established or wait for an existing connection to become idle.

HTTP/2 introduces HPACK, a compression scheme for HTTP headers that reduces the redundancy between requests. Compression helps multiplexing, because requests are smaller. This enables clients to make many requests in their first packets on a connection, while TCP flow control windows are still small.

HTTP/2 also introduces the concept of **push**: the server responds to requests the client hasn't made yet, but in a form that the client can cache and reuse on other pages. If **push** is supported by the underlying connection, two things happen:

- 1. A `PUSH_PROMISE` is sent to the client so that the client can check whether the resource already exists in the cache.
- 2. A new request is added to the request queue for the pushed resource.

## Understanding HTTP/2

In this recipe, we are going to understand HTTP/2. We will visit a few websites and check whether HTTP/2 is supported or not later in this chapter. We are going to see how we can check whether the website is running on HTTP/2 or HTTP/1.1.

Let's cover this in detail.

### Getting ready

To step through this recipe, you will need a running HTTP/2 website and internet connection.

### How to do it...

- 1. Open Internet Explorer on your Windows 10 PC or Server 2016; here, we are using a Windows 10 PC.
- 2. Go to the address bar and type `www.google.ae`:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_151_0.png)

- 3. Once you open `www.google.ae`, it will redirect to `https://www.google.ae/`. Now we have to check whether `www.google.com` uses HTTP/2 or HTTP/1.1.
- 4. Hit F12. You can also do the same as the following step:
- 5. Go to **Settings** and enable F12 Developer Tools; you will get DOM Explorer, Console, Debugger, Network, Performance, and Memory. Now you have to switch to the **Network** tab:

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_152_0.png)

- 6. Here, you will see that `https://www.google.ae` is using HTTP/2. Next, we will apply HTTP/2 settings on our IIS 10.0 server in order to use the HTTP/2 protocol.

### How it works...

In this recipe, we launched the URL www.google.ae. The browser was automatically redirected to https://www.google.ae. Next, we tested for HTTP/2 through developer tools, which can be run through the F12 key, or can be enabled from the Settings menu.

## Installing HTTP/2 on IIS 10.0

In this recipe, we are going to set up HTTP/2 and enable the logging option through IIS 10.0. We will also cover how and where we can perform the necessary changes in the registry.

- 3. Double-click on Logging. You will see in the next figure that Site is selected as the default log file type, and the Log File format is W3C, with a lot of fields as options.

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_154_0.png)

- 4. Logging configuration steps help us trace log files and check website access through HTTP or HTTP/2.
- 5. As shown in the figure, One log file per Site means each website has its own log file. Hence, it will be easy to use the log file information. We have an option to select the log file for the server, so it will create one log file for your server. This means that if you have hosted many websites per server and you have one log file for that server, it could be very difficult to trace the log information. That's why we have selected site-wise log files—there are a lot of options available in W3C logging fields to identify using Hostname, IP address, and port as well as Client IP address, and so on. We've just verified our recommended selection.
