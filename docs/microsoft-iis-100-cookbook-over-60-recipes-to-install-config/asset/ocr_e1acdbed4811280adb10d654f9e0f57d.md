# Hardware requirements

Let's look at the minimum hardware requirements to run Windows Server 2016 and Windows 10:

# Windows Server 2016

+   - Processor: 1.4 GHz 64 bit
+   - RAM: 2 GB for 64-bit OSes
+   - Hard disk space: 32 GB

# Windows 10

+   - Processor/SoC: 1 GHz
+   - RAM: 1 GB for 32-bit or 2 GB for 64-bit OSes
+   - Hard disk space: 16 GB for 32-bit or 20 GB for 64-bit OSes
+   - Graphics card: DirectX 9 with the WDDM 1.0 driver

# Operating system media

IIS 10.0 installation requires operating system media. While installing IIS 10.0 on Windows Server 2016, sometimes, it asks for the operating system media for required files. We have to insert the media either in CD or USB drive form.

To install IIS 10.0 Express only, we need the IIS 10.0 Express software, which is available on the Microsoft IIS website (https://www.microsoft.com/en-us/download/details.aspx?id=48264). You only need the software and an Internet connection.

# IIS 10.0 architecture

We need to understand what resources are available to your IIS application; how it handles content files of different types, such as static files, ASP files, or ISAPI files; and how it groups content files into applications. An introduction to the client/server relationship is included for first-time readers unfamiliar with web servers.