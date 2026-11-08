# Installing the Business Analytics Platform

The installation of the Pentaho Business Analytics Platform is straightforward, following an *unzip and run* task. The prerequisites to run Pentaho Business Analytics Platform are simple; it requires a JRE 8, already installed in your environment. Both Windows-based operating systems and Linux-based operating systems are supported. The screenshots and previews shown in this chapter are done using an Ubuntu 16.04.2 LTS distribution.

Before installing the application, you can download the artifacts distributed in a ZIP file directly from the official Pentaho Community website (http://community.pentaho.com/) or the Pentaho SourceForge website (https://sourceforge.net/projects/pentaho/). In both cases, you should download a ZIP file named something similar to `pentaho-server-ce-<version>.zip`.

Once the ZIP file has been downloaded into your environment, unzip it into the final path (on a Linux-based operating system, the `/opt` path is suggested), and you will have all the Pentaho Business Analytics Platform data available in the folder `pentaho-server`.

Though you probably cannot believe it, this is all that is needed to install Pentaho Business Analytics Platform into your environment.

Pentaho Business Analytics Platform is mainly a web application deployed in an Apache Tomcat Servlet Container (https://tomcat.apache.org/). You can easily run the Pentaho Business Analytics Platform by opening a Terminal and executing the `start-pentaho` script stored in the `pentaho-server` folder (the one with the `.sh` extension for the Linux-based operating systems, and the one with the `.bat` extension for the Windows-based operating systems).

To check what is happening to the web application, you can take a look at the log file at `<pentaho-server>/tomcat/logs/catalina.out`. Once the application has started, you can access the user interface using a supported web browser, at the link `http://<server>:8080/pentaho`. With Pentaho Business Analytics Platform