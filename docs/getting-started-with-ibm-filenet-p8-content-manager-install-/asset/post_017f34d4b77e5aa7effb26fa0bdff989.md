
A fix pack will include a set of instructions for installing it. We won't go over that in detail, but you should read through it for the particular fix packs you are installing. The procedure we will illustrate next is fairly generic, and fix pack instructions sometimes include important special case items or limitations. A fix pack usually cannot be uninstalled, so it's important to have backups or some other recovery plan in case you run into problems or decide to abandon the installation of a fix pack.

## CE 4.5.1.4 server

Our first stop will be upgrading the CE server and client components. The CE server is running on our Linux server in a WAS profile. CE client updates must be done for the CE installation in a separate WAS profile, the PE server running on our Windows server, and any Windows machine where FEM is installed. If you set up the Java or .NET development environments, then those machines need the client update as well. We'll update the CE server first and then apply the client updates. That's the supported order for CE updates.

> Before making any change to your CE server, make sure you have safely saved a copy of your bootstrapped CE EAR file. If you didn't do that in Chapter 3, Installing the Content Engine, do it now. The CE EAR file will be named Engine-ws.ear, Engine-wl.ear, or Engine-jb.ear, or Engine-jbc.ear, depending on your application server brand (WebSphere, WebLogic, or JBoss, respectively). If you are taking a copy from a deployed location, your application server may have renamed the CE EAR to something else. You should not skip this step. There are a lot of recoverable mistakes that can become unrecoverable if you have lost your original bootstrap settings.

Download and unpack the CE server fix pack update. One of the files you will find in the unpacked directory is version.txt. This short file precisely describes the exact builds of the included components. Here are the contents of version.txt from our CE fix pack:

```
P8 Content Engine 4.5.1.4 build dap451.004.002
Using pui451.057
Using pch455.004
Using cm451.002.008
Using dep451.074
This build includes cambridge14.mdb revision
```

It's not too important to know what each of those things means. They're just internal build number designations for subcomponents, not all of which are actually visible in the installed product. We'll use this information later to double check that the fix pack installation went as expected.

With the CE server shut down, run the CE server fix pack installer. In our case, the name of the executable is 4.5.1.4-P8CE-FP004.BIN. Even though it's a graphical installer program, we recommend launching it from a shell window in case any important console messages are printed. Several of the installer panels are applicable only to a Windows environment, and you probably want the default location for the CE installation. That means you can click through most of the panels.

On the final panel, the installer suggests that you run the client updaters for PE and CSE if you are using those components and have updates for them. In other words, it is telling you to update those components if they are used by the CE server. We are using both but only have an update for CSE, so we'll take care of that next. Make sure you uncheck the box for Launch Configuration Manager and click the Done button.

You should look through the installation log file (in directory /opt/ibm/FileNet/ContentEngine/) to see if any problems occurred.

## CSE 4.5.1.1 client

Unpack the Linux client package for CSE fix pack 4.5.1.1. The installer executable name in this case is 4.5.1.1-P8CSE-CLIENT-LINUX-FP001.BIN. Again, we recommend running this from a shell window to launch the graphical installer. This installer will be updating the CSE client files inside the CE server EAR file. Assuming you are using the default paths, you can click through the installer panels and it will finish without incident.

> One thing you should look for in release notes for a CSE fix pack is whether or not it requires a re-index of your content. Typically, they do not, but it is possible that one would. For our tiny all-in-one site, that wouldn't be an issue. For a production site with a few million documents or more, the re-indexing time would be significant.

The CSE client file updates will be placed in /opt/ibm/FileNet/ContentEngine/CSEClient/. You will want to glance through the installation log file in that directory to check that no problems occurred.

## CSE 4.5.1.1 server

Since we just installed the CSE client update, we'll install the CSE server update to keep things in sync. Unpack the Linux server package for CSE fix pack 4.5.1.1. The installer executable name in this case is 4.5.1.1-P8CSE-LINUX-FP001.BIN. Again, we recommend running this from a shell window to launch the graphical installer. If you don't have an environment variable setting already for this, you will need to set a value for JAVA_HOME before running the installer. Here is the setting we used in Chapter 5, Installing Other Components:

```
# export JAVA_HOME=/usr/lib/jvm/java-1.5.0-ibm-1.5.0_11.2.x86_64
```

> You must shut down all CE and CSE servers during the update to CSE server software. Otherwise, the CSE may be trying to service CE requests during the update, and corruption of your text index can result.

The installer will be updating software in the CSE server itself, so we will first stop the CSE server. If you are using something like our up and down scripts from Chapter 2, you can just run the down script to stop everything. Otherwise, go to the CSE software location (the default is /opt/ibm/FileNet/ContentEngine/verity/k2/_ilnx21/bin/), and run the k2adminstop command.

> A quick way to see if any CSE software is running on a Unix or Linux server is to run the command ps -ef | grep k2. If nothing shows up (except perhaps the grep command itself), then no CSE software is running.

The fix pack files include scripts for backing up and restoring your CSE configuration files. You'll need a target directory for storing these files (they'll total 30-50 MB). You can safely put them into a directory under /tmp/ only if your system doesn't automatically clean up /tmp/ during a reboot. A safer policy would be to store them in a new subdirectory of the location where you unpacked the fix pack files. You should run the backup (and the later restore) as the user we designated in Chapter 5 as the k2_os_user.

```
# su - k2_os_user -c "/some/path/backup_k2.sh /opt/ibm/FileNet/ContentEngine/verity /some/path/k2save"
```

In a production environment, you might be splitting up the various CSE functions among multiple servers. If that's the case, you should run the backup procedure and the CSE installer on each separate server.

Restart the CE WAS profile. (If you stopped the WAS profile by shutting down everything, you will also have to restart the directory server.) From the WAS console, navigate to Applications | Application Types | WebSphere enterprise applications. Select the CE server (usually called FileNetEngine) and click the Uninstall button. Follow the prompts and Save the changes to the master configuration.

If your CE server application was called FileNetEngine, then the WAS cache files for it will be /opt/ibm/WebSphere/AppServer/profiles/AppSrv02/temp/localhostNode02/server1/FileNetEngine/. The most likely situation is that you will find only empty directories in that location. Stop the WAS profile, and delete everything in that FileNetEngine directory and below.

Restart the WAS profile. All vestiges of the CE server application are gone at this point.

When we initially installed the CE server in Chapter 3, we did so as the root user. We'll also run the P8 Configuration Manager as the root user. We'll fix up the file and directory ownerships later, just as we did in Chapter 3. Run the P8 Configuration Manager with the following command:

```
# /opt/ibm/FileNet/ContentEngine/tools/configure/configmgr
```

Navigate to and open the configuration profile (wjc-rhel) saved from the initial install. You may recall from Chapter 3 that we changed the WAS console administrator login from administrator to was_admin. The configuration profile probably still reflects the earlier administrator user. Click the icon that looks like a computer to open the panel for application server configuration settings. Supply the was_admin user name and password. Use the Test Connection button to make sure your settings are correct.

![](img/2295191c450a0c86b2d82df6cec04526_419_0.png)
