```
SimpleHTTPServer simpleServer = new SimpleHTTPServer(configCtx,
8070);
simpleServer.start();
```

One thing you need to remember when using a URL repository is that the services directory has to have a file named services.list, listing all the services archive files. For example, if you have services named foo.aar and bar.aar, then the services.list will look like the following:

```
foo.aar
bar.aar
```

In the same way, the modules directory should contain a file named modules.list listing all the modules.

# Deploying Axis2 in various application servers

As we discussed in Chapter 1, Apache Web Services and Axis2, Axis2 is available in the form of several distributions. The application server distribution is one of them. You can download the Axis2 application server distribution or the WAR distribution from the Axis2 website or build them from the binary distribution. Once you have the WAR distribution, deploying it is just a matter of copying the WAR file into the webapps directory of the application server. In the case of Apache Tomcat, it is the webapps folder, whereas in the case of Sun Glassfish, it is the autodeploy folder, and so on. So, depending on the application server, you have to figure out the correct location to drop the WAR file.

There are some application servers that will help in unpacking the WAR distribution to a permanent or temporary location. For example, Apache Tomcat will unpack the WAR file to a permanent location. You can make changes there, and when the application server restarts, all of those changes will be available. However, there are some application servers that do not do this. Also, there are application servers that will not unpack the WAR file. Even in Apache Tomcat, you can configure whether you want to unpack or not. Depending upon the application server configuration, hot deployment and hot update also vary. For example, if the application server is not going to unpack the WAR file, you do not have the hot deployment available at all.