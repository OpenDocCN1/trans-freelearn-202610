
Once you have created your application, VS will restore the NuGet packages in the background. When this is done, press F5 to build and run your app in the debugger. Your default browser should open containing the ASP.NET Core template site, and VS will show graphs of performance tracing:

![](img/5e416ac29f1bccaad147c6829853fd77_64_0.png)

Congratulations! You’ve just created an ASP.NET Core 2 web app.

## Mac

Apple computers have always been desirable and have a price tag to match. They are very popular with developers and web developers in particular. Perhaps this is because they are the only machines that can run all the things on which you might want to test. You can run Windows and Linux in a VM, but Apple’s macOS (previously called OS X, pronounced OS ten) won’t run on any non-Apple hardware (without some persuasion).

This could also be due to the meteoric rise of iPhone and iPad, along with the ecosystem that goes with them. If you want to develop apps for iOS and Android, then the only reasonable choice is to use a Mac—although, Progressive Web Apps are encroaching on much of the territory that was traditionally occupied by native applications.

## Visual Studio for Mac

VS Mac is a rebranding of Xamarin Studio, but it has also been expanded and taken a lot of the features and code from regular VS. It is still best suited for making Xamarin cross-platform mobile and desktop apps, but it also supports .NET Core and ASP.NET web apps.

First, download the files that you'll need. This includes software to build your apps in, such as VS for Mac or VS Code. VS Mac comes as a DMG disk image that contains an installer, whereas VS Code comes as a ZIP file that you can just unpack and run. You will also need the .NET Core SDK, which comes as a PKG file that needs to be installed. You may also want a handy guidebook:

![](img/5e416ac29f1bccaad147c6829853fd77_65_0.png)

### Installing VS Mac

VS for Mac is supported by the latest version of macOS (High Sierra), but it also works on older versions, such as OS X El Capitan (10.11) and macOS Sierra (10.12).

2. On the next screen, select .NET Core 2.0 as the target framework:

![](img/5e416ac29f1bccaad147c6829853fd77_70_0.png)

3. Finally, add a project name and finish the wizard:

![](img/5e416ac29f1bccaad147c6829853fd77_70_1.png)

You can also work on this solution in Visual Studio Code by opening the folder containing it:

![](img/5e416ac29f1bccaad147c6829853fd77_72_0.png)

## Visual Studio Code

Visual Studio Code runs on Windows, Mac, and Linux. VS Code is more of a text editor than an IDE, but it is a very capable one, and it blurs the line between the editor and full IDE. It supports IntelliSense code completion, building your code, and debugging your app.

It is built using web technologies (TypeScript in particular) and runs in a standalone web browser based app hosting environment. However, it is much quicker than similar Electron desktop apps, such as Atom or Slack. The same engine is used in the Azure web interface for editing code there.

To deal with huge data files, it is better to stick to a native text editor, for example, Notepad++ (on Windows) or Sublime Text.

## Linux

Linux is a hugely popular OS outside of the desktop market. Android (the mobile operating system acquired by Google) is based on it, and it runs on most of the world's smartphones. Sometimes it is the main desktop option, if using a computer with specialized hardware, such as the Raspberry Pi. Linux is very popular on embedded devices, in part because it is lightweight and usually much less demanding on resources than a typical Windows or macOS installation.

Linux is also the most popular operating system for web servers, which is what interests us most here. It could be argued that Linux is the most popular operating system on the planet, although many users won't interact with it directly.

![](img/5e416ac29f1bccaad147c6829853fd77_73_0.png)

Strictly speaking, Linux only refers to the kernel of the OS. The equivalent part of Windows would be the NT kernel. Many different distributions are built using the Linux kernel at their core, and they bundle it up with many other (often GNU) applications. You can even roll your own stripped-down version, perhaps using something like Buildroot.

Some examples of popular distributions are Ubuntu and CentOS (Community Enterprise OS, a free version of Red Hat Enterprise Linux). Linux distributions can be classified broadly by the type of package management they use. This is useful when trying to find the correct version of software to install, although you can usually get popular applications directly via the distribution's package manager.

![](img/5e416ac29f1bccaad147c6829853fd77_73_1.png)

If you've not used a package manager before, then you will be amazed at how much easier it is than manually installing software (or even using an App Store) as you'd normally do on Windows or Mac.

Many distributions are based on Debian and these include Mint, Ubuntu, and Raspbian (the official OS for the Raspberry Pi). These use a Debian-based package format and the APT package manager. The latest version of Debian is called Stretch and .NET Core supports it. It also uses it for the official .NET Docker images.

![](img/5e416ac29f1bccaad147c6829853fd77_73_2.png)

Debian versions are named after Toy Story characters; the previous version was called Jessie.

Other distributions use a **Red Hat Enterprise Linux (RHEL)** RPM package system and the yum package manager. These include CentOS and YellowDog, just to name a couple. There is also **SUSE Linux Enterprise Server (SLES)**, which is another different distribution, but .NET Core supports it too.

There are many other distributions, far too many to mention here. Linux is not one OS like Windows or Mac. We haven’t even covered BSD-based systems, which use a different kernel. These include FreeBSD, OpenBSD, and NetBSD. Even macOS is built on BSD, with its NeXTSTEP heritage (the OS that ran the first web server at CERN).

Most of the instructions here will also work on the command line of Windows and Mac. The tooling is the same; even the graphical tooling in VS uses the same templates as the command-line `dotnet` utility.

## Getting started with .NET Core on Linux

We’re going to mix things up a little and show how versatile and multiplatform .NET Core is. In the following section, we will demonstrate how to develop a .NET Core app on an Ubuntu Server VM running on Azure, using just the command line and no window manager.

This will be very similar to how you might set things up on any Debian-based distribution, including Ubuntu, Mint, and Raspbian (for Raspberry Pi). The fact that the Raspberry Pi uses a custom ARM processor rather than a traditional Intel x86 CPU does not matter. When Mono was initially ported to the Pi, there were issues with the specialist hardware-based floating-point implementation, but this is no longer an issue with .NET Core.

We will be using a command-line text editor, and it would be foolish to get into the Emacs versus vi debate here, so use whatever you prefer. This setup might make a good workflow if you use a tablet (such as an iPad with a Bluetooth keyboard) to connect to a cloud server over SSH or Mosh. However, if you are working locally and want a graphical editor in your X11/Wayland desktop environment of choice, then you could run VS Code or JetBrains Rider. No doubt you will be using terminal windows a lot too.

## Installing .NET Core 2

We’re using Ubuntu Server 16.04 LTS in this guide, but these instructions will work on other versions too. Using **Advanced Package Tool (APT)**, we can install .NET Core very easily, but first we will need to add a custom feed as .NET Core is not in the default repository.

First we need to add the .NET Core package source to the list used by APT. The package source is on Azure, so we write this URL (plus some other info) to a file in the folder of sources used by apt-get. We do this by redirecting the standard output of echo to it, using the following command:

```
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
```

All packages are signed with public key cryptography to verify their integrity (read the later chapters for more on this). We import the public key for the .NET Core packages from the Ubuntu key server with the apt-key command:

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

We need to update our sources so that the latest changes can be picked up with this simple command:

```
sudo apt-get update
```

The update command is different to the upgrade command, but they are often used together. While update simply refreshes the information that APT holds about packages, upgrade allows you to very easily update all of the software on the machine (that hasn't been manually installed) to the latest versions. This can be quite a time-saver.

Now we are in a position to install .NET Core with the following command:

```
sudo apt-get install dotnet-sdk-2.0.0
```

If the SDK package ever makes it to the default list of sources, then this will be all you need to do.

If you are trying out a preview, then the package will have the preview tag suffixed (for example, -preview2-006497).

After a wall of text scrolls, assuming you don't get any errors and you see the telemetry notice, you're ready to go. Type dotnet to see the usage information and dotnet --version to see the current version that you are running.
