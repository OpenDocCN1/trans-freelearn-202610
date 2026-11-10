You can download the binary version of the latest version at `http://tomcat.apache.org/download-70.cgi`. After being downloaded, Tomcat can be installed by simply unzipping it, which creates the following directory hierarchy:

![](bbox=[0.42, 0.22, 0.58, 0.42])

Arguably, the most important directory is the webapps directory. This is the deployment directory, and files that need to be made available to client browsers, JAR files, configuration files, and so on are placed into it. Throughout this chapter we are going to provide details on how to organize web applications under this directory.

To set up and run Tomcat outside Eclipse, you need to configure two environment variables, namely, `JAVA_HOME` and `CATALINA_HOME`. The `JAVA_HOME` environment variable should be set to the base path of your JDK. Tomcat 7.0 requires Java SE 6.0 or later. `CATALINA_HOME` should point to the base path of the Tomcat directory hierarchy. Instructions on how to set these environment variables for all operating systems can be found at `http://tomcat.apache.org/tomcat-7.0-doc/setup.html`. In addition, the file `RUNNING.txt` in the Tomcat's root directory also contains installation instructions.

> It is worth mentioning that, although we use Tomcat throughout this chapter, the examples shown here are not Tomcat-specific. Therefore, you can deploy and run all examples shown here in other servlet containers.