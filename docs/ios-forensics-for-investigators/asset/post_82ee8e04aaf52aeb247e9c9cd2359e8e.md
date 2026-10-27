
simple tool that simplifies the task of locating an application's containers.

## Using cda to locate an application's containers

In the first part of this chapter, we learned how to query the `applicationState.db` database to locate where an iOS application stores its data; however, if the examiner has direct access to the device, the cda tool can be used to quickly identify an app's containers.

This tool, which can be downloaded from [https://github.com/ay-kay/cda](https://github.com/ay-kay/cda), is written in Objective-C; the source files can be compiled by following the instructions on the project's page.

To make this process easier, the binary executable that's been compiled for 64-bit ARM devices can be downloaded from [https://github.com/tiepolocgian/iOS-Tools](https://github.com/tiepolocgian/iOS-Tools).

The next step involves copying the cda binary to the device. This can be done by using the scp command, which performs file transfers through an **SSH connection**. Assuming you downloaded the binaries and decompressed the ZIP file, follow these steps:

- 1. Open a Terminal shell on your local workstation and navigate to the `iOS_tools` folder by using the `cd` command.
- 2. Run the `scp -P 4242 cda root@127.0.0.1:/usr/bin/` command and press *Return*. You will be prompted to enter the device's password, which is `alpine`. Enter it and press *Return*.
- 3. If the command succeeds, the cda binary will be copied to the `/usr/bin/` folder on the iOS device. By placing the executable in that folder, the tool can be called from any location within the device.
- 4. To ensure that it worked, from the terminal shell that is connected to the device, run the `ls -la /usr/bin/` command. You should see a long list of files, including cda.

Once the tool has been copied to the device, we can use it to quickly locate any application that has been installed. Simply run cda, followed by the name (or partial name) of the app.

For example, on a test device that has the **Instagram** app installed, we can find where the application stores its data by running the following command:

```
iPhone:~ root# cda instagram
[1] Instagram (com.burbn.instagram)
Bundle: /private/var/containers/Bundle/Application/D054F8DC-95A4-4489-BBBD-68F3E457A575
Data:
/private/var/mobile/Containers/Data/Application/2A2FEE52-B59D-42F1-A810-333364E12525
```

As you can see, the tool provides the examiner with the list of containers associated with the application. This includes the **data container**, which is where the app stores user data and preferences, and the **group container**, which contains data that is shared between applications of the same group.

Locating an application's containers is often the very first step in dynamic analysis as this will narrow down the scope of the examination, allowing the examiner to focus on just one or two folders. In the next section, we'll learn how to monitor these folders to understand how and where the application stores its data.

## Using fsmon to monitor filesystem events

Previously, we introduced dynamic application analysis and learned that one of the steps of this process involves running the app and live monitoring its behavior. One aspect worth monitoring is how the application interacts with the filesystem; this includes detecting any files that are written, edited, or deleted by the application.

By using a filesystem monitor, the examiner can run the application, perform a task such as entering some data (user authentication, forms, and so on), and visualize how the application reacts to user input. This can help them understand which files store what data.

One of the most popular filesystem monitoring tools is `fsmon`, an open source tool developed by *Sergi Álvarez* that supports many operating systems, including **Linux**, **macOS**, **Android**, and, of course, **iOS**. The tool can be downloaded from GitHub at https://github.com/nowsecure/fsmon.

Pre-built binaries for 64-bit ARM devices can also be downloaded from https://github.com/tiepologian/iOS-Tools.

The first step involves copying `fsmon` to the device, as we did with the `cda` tool:

- 1. Open a terminal shell on your local workstation and, if you're using the pre-built binaries, navigate to the `ios_tools` folder by using the `cd` command.
- 2. Run the `scp -P 4242 fsmon root@127.0.0.1:/usr/bin/` command and press *Return*. Enter the device's password and press *Return*.
- 3. If the command succeeds, the `fsmon` binary will be copied to the `/usr/bin/` folder on the iOS device.

To run the tool, type `fsmon`, followed by the path to the location that should be monitored. For example, to run `fsmon` and monitor all events over the entire filesystem, type `fsmon /`. The following screenshot shows the tool's output:

![](img/0b17a17d4cb56b1c25763a396dd83117_261_0.png)

Figure 10.7 – The fsmon tool prints any filesystem events

As you can see, `fsmon` shows any events that occur on the filesystem in different colors, depending on the type of event. Events displayed in yellow indicate that a file was *edited* or its attributes were *modified*, a red event shows that a file was *deleted*, and a purple event indicates a newly *created* file.

To understand how an application works under the hood, use `cda` to find where the app's data container is located. Then, use `fsmon` to monitor that location and run the app normally, performing typical user activity. Monitor the tool's output to understand what files are created or edited following a specific action being performed in the application.

Finally, in the next section of this chapter, we'll take a brief look at how to monitor networking activity.

## Using mitmproxy to monitor network activity

When you're performing a dynamic application analysis, you may come across a situation in which you want to look at what **API calls** an application is making, what servers it connects to, and, more generally, what data is sent or received over the network.

One of the possible solutions involves using a **proxy server** to intercept and decode encrypted data that is sent over HTTPS/SSL, such as **mitmproxy**.

mitmproxy is an open source interactive **HTTPS proxy** that sits in the middle between the client (iOS application) and the server. **Man-in-the-middle (MITM)** is the name of the technique that's used by the proxy server to intercept the traffic: the idea is that `mitmproxy` pretends to be the server to the client, and pretends to be the client to the server, all while decoding traffic from both sides. Decrypting traffic is made possible by installing a digital certificate on the device, which registers `mitmproxy` as a trusted **Certificate Authority (CA)**.

To use `mitmproxy`, you must install the certificate manually onto the iOS device, then configure the device to send traffic to the proxy server. Finally, you must run the proxy server on your workstation so that you can monitor and analyze networking activity.

To install `mitmproxy`, go to https://mitmproxy.org and download the installer. If your workstation has `brew` installed, you can install the proxy by running the following command from your terminal:

```
brew install mitmproxy
```

Once the server has been installed, start it by running `mitmproxy` from your terminal.

When the server is up and running, the CA certificate needs to be installed on the iOS device. You can accomplish this by going to http://mitm.it from the device's browser; however, before you install the certificate, its networking settings need to be configured to allow traffic to be sent from the device to the proxy. Follow these steps:

- 1. On the device, open **Settings | Wi-Fi** and click on the wireless network to open **Advanced settings**.
- 2. Scroll down to **HTTP Proxy** and tap **Configure Proxy**.
- 3. Select **Manual** and, in the **Server** field, enter the IP address of the workstation that is running mitmproxy. In the **Port** field, enter 8080.
- 4. From the device, browse to [mitm.it](http://mitm.it) and follow the instructions to install the CA certificate.
- 5. Navigate to **Settings | General | About | Certificate Trust Settings**. Find the certificate and enable it by turning the toggle on.

If everything has been set up correctly, open any application on the device and monitor the terminal on your workstation: you should see the network traffic flowing in and out of the device.

The following screenshot shows mitmproxy intercepting network traffic:

![](img/0b17a17d4cb56b1c25763a396dd83117_263_0.png)

Figure 10.8 – Intercepting network traffic using mitmproxy
