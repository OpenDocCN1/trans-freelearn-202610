
## The development environment

A **development environment** is a term that refers to having everything you need in order to develop, set up, and be ready to go in one place. We need the following two things to get started:

- We talked a fair bit about compiling our Java code, as well as other people's Java code, into DEX code that will run on the DVM, on people's Android devices. In order to use Java code, we need a free software called the JDK. The JDK also includes other people's code, which is separate from the Android API.
- There is a whole range of tools that are required to develop for Android, and we also need the Android API, of course. This whole suite of requirements is collectively known as the Android **software development kit (SDK)**. Fortunately, downloading and installing a single application will give us these things all bundled together. This single application is called Android Studio.

Android Studio is an **integrated development environment (IDE)** that takes care of all the complexities of compiling our code and linking with the JDK and the Android API. Once we have installed the JDK and Android Studio, we can do everything we need inside this application.

### Tip

### What could possibly go wrong?

I got a lot of feedback via my website [http://gamecodeschool.com](http://gamecodeschool.com) about a previous book of mine that showed how to install an Android development environment. People said that setting this up can be the hardest part. So I've written this section to be as thorough as possible because not everybody has a smooth setup experience.

Hopefully, you won't need all the extra tips and detailed figures, but there are a few vagaries that can trip us up while setting up the development environment.

Most likely, these instructions will get you up and running quickly and cover all your issues. If there is an issue that I haven't managed to anticipate, don't let it beat you! I guarantee that you are not the first to have that exact issue. Perform a eat you. I guarantee that you are not the first to have that exact issue. Perform a web search (use Google), be really specific in your search criteria, and I am confident you will be coding in no time at all.

This guide will get around 99% of the Windows user's setup in a couple of hours. All the coding and development information you need will be covered 100% step by step.

For Mac and Linux users, most of these instructions can be easily interpreted as the key points of the tutorial are more about what we do inside of the setup programs and less about the specific environment we are using. My apologies for not providing comprehensive instructions for every operating system.

## A note to the existing developers who use Eclipse

There are multiple options regarding which software to use when developing Android apps. In the early days of developing Android, an IDE called Eclipse was most commonly used. A few years ago, a new "official" contender for the best Android IDE was released. This was Android Studio. The problem after its first release was that it was still in the beta (not finished) stage. It had numerous bugs, including some quite awkward ones. However, even then it was the preferred IDE for many because of its smooth operation, cool looks, official status, and prestigious heritage.

Now that Android Studio has exceeded version 1, there is virtually no reason to use anything else, especially if you are just getting started with Android. If you already have Eclipse set up for Android development and really don't want to change, that's fine, the code in this book will work. However, there will be significant differences in the instructions, especially regarding the **user interface** (UI) designer. Also, the code in this book has been thoroughly tested by others and me in Android Studio.

The sections that follow will set up a development environment with the JDK and Android Studio as our IDE of choice.

## The JDK

This can be as simple as downloading, double-clicking on the downloaded file, and following the installation instructions. However, sometimes it isn't. So, it is probably worth running through the installation process step by step and pointing out along the way a few options that can make things easier.

### What if I already have Java?

Most PC/Mac computers already have Java installed. Many modern apps require Java, and a classic example of this is the game *Minecraft*. Java is subtly, but significantly, different from the JDK. Java on its own just runs programs that have been written in Java for PC. This is the PC equivalent to the DVM. Java on its own, however, will not compile our code or make other people's code that we need available. This more common version of Java is called the **Java Runtime Environment (JRE)**. When we install the JDK, it will also install the JRE, whether you already have it or not. So, if you are conscientious about keeping your hard drive in order, you could uninstall the JRE using the Windows Control Panel in the usual way before proceeding. Then find and delete your existing Java folder.

This tutorial will then reinstall the latest version of the JRE as well as the JDK. If, however, you know that you have a program that uses Java and you don't want to mess with its configuration, then go ahead with this tutorial, but just be aware that there will be a JRE and an associated Java folder in two places on your hard drive.

To summarize the preceding in case anything is unclear: this tutorial assumes that you don't have an existing folder named Java, but will work just fine even if you do.

### Installing the JDK

As a little bit of preparation before we install the JDK, you need to know which operating system you have and whether it is 32 or 64 bit. If you are unsure, use this little tip to find out.

### Tip

### Do I have a 32-bit or 64-bit Windows system?

To find out, right-click on My Computer (This PC on Windows 8) icon, left-click on the Properties option, and look under the System heading of the

![](img/35d01831c19688a44e50bce7c489007a_51_0.png)

System type entry like this:

Now we are ready to install the JDK. This fairly simple set of steps will set up the JDK quickly. The only slight delay is the download itself, which could take a while on slower Internet connections. The actual installation process should be fast and trouble free:

- Visit the Java JDK downloads page at http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html.
- If the link has changed, conduct a web search for Java JDK download. The only potential stumbling block at this stage is that you click on a link for the JRE instead. JDK is what we need. The following is a screenshot of the important part of this page:

| Product / File Description | File Size | Download |
|---|---|---|
| Linux x86 | 146.9 MB | jdk-8u51-linux-i586.rpm |
| Linux x86 | 166.95 MB | jdk-8u51-linux-i586.tar.gz |
| Linux x64 | 145.19 MB | jdk-8u51-linux-x64.rpm |
| Linux x64 | 165.25 MB | jdk-8u51-linux-x64.tar.gz |
| Mac OS X x64 | 222.09 MB | jdk-8u51-macosx-x64.dmg |
| Solaris SPARC 64-bit (SVR4 package) | 139.36 MB | jdk-8u51-solaris-sparcv9.tar.Z |
| Solaris SPARC 64-bit | 98.8 MB | jdk-8u51-solaris-sparcv9.tar.gz |
| Solaris x64 (SVR4 package) | 139.79 MB | jdk-8u51-solaris-x64.tar.Z |
| Solaris x64 | 96.45 MB | jdk-8u51-solaris-x64.tar.gz |
| Windows x86 | 176.02 MB | jdk-8u51-windows-i586.exe |
| Windows x64 | 180.51 MB | jdk-8u51-windows-x64.exe |

- Your download page will most likely be slightly different. There will be a newer version of the JDK by the time you read these words, so the 8u51 will be different. That's OK, we just need whichever is the newest version. Simply click on the **Accept License Agreement** radio button that is highlighted in the previous screenshot and then click on the download link, which is on the right-hand side column corresponding to your operating system in the **Product/File Description** column. If you are not sure whether you have 32-bit or 64-bit Windows, refer to the tip before this section.
- Wait while the JDK is downloaded to your hard drive.
- In the folder where you've downloaded the JDK, right-click on the jdk-8u51-windows-x64.exe file and select **Run as administrator**. The precise name of the file you have will vary based on whether you have 32-bit or 64-bit Windows and what the current version of the JDK happens to be at the time.
- There will be a series of windows that will guide us through the installation process. The most we have to do is just click on **Next** to proceed. As promised, I will guide you through them one at a time and point out when you might like to make changes or make a note of things. The following is a screenshot of the first window that you will see during the installation:

![](img/35d01831c19688a44e50bce7c489007a_52_0.png)

- Now, click on **Next** and you will see this window:
