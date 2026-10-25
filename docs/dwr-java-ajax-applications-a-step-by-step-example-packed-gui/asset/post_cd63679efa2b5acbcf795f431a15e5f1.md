
The configuration in `dwr.xml` also includes a create entry for each remoted Java class. We can use singleton, new, or some other mechanism. We can also limit which methods are allowed using the include and exclude elements in `dwr.xml`. There are also security parameters in the `web.xml` configuration file such as `allowScriptTagRemoting` and `crossDomainSessionSecurity`, which when enabled, may be a huge security risk. So make sure you understand what you are doing if you are enabling the parameters.

There is one configuration parameter that is very useful while developing a DWR application, but that is a security risk in the production (especially public) site. The following init-param, debug, and test mode can be added in the web.xml.

```
```xml
<init-param>
    <param-name>debug</param-name>
    <param-value>true</param-value>
</init-param>
```
```

When debug/test mode is enabled, DWR generates test pages (there is a sample screen in Chapter 4) that are useful tools to get to know the DWR functionality.

It is also possible to use JEE access control mechanisms and limit access to DWR itself for specific JEE roles. These are specified in the web.xml file. Within DWR, there is also a role-based access control to the methods of remoted Java objects. DWR can also be integrated with Spring Security (also known as Acegi Security), `http://www.acegisecurity.org/`, the official security projects of the Spring Portfolio. The description of Acegi and DWR integration is found on `http://java-x.blogspot.com/2007/08/handling-security-with-ajax-dwr-and.html`.

DWR is also designed so that there is no possibility of an attacker accessing and manipulating DWR core files and changing the security parameters for example.

## Minimize Risks

Security is not a trivial issue, and we could spend years in the security field and still leave a lot unfinished. As developers, our job is to make sure that security risks are minimized, and at least realize what might happen if we remote a certain Java method.

A very good example from the DWR website is that if we make a remote call to a method called `appendStringToFile()`, which appends a given string to a predefined file, then a possible attacker can send a large number of requests and fill the file system with nonsense and thus cause an unexpected server downtime. The cost of downtime may be anything from little inconvenience to millions of dollars.

In order to prevent giving accidental access that may cause harm, we have to think of what we want to do and also check the input parameters. Checking input parameters is the most important thing that we as developers can do to make sure accidents do not happen.

In the previous example, if we have remoted the `appendStringToFile()` method, we can do lots of checks on whether the file system is filling, whether it is a string valid for the purpose for which it was meant (such as log entry to a log file), who the requester is, and so on.

Also make sure that `escapeHTML` is used in order to prevent cross-site scripting (XSS) attacks. XSS is something worth knowing about. So please refer to Wikipedia (http://en.wikipedia.org/wiki/Cross-site_scripting) for a detailed description.

## Summary

This chapter presented an overview of DWR features including JavaScript libraries (`util.js`, `engine.js`, and `gi.js`), security, and Reverse AJAX and on basic operational elements of how DWR works.

In addition to the topics discussed in this chapter, DWR has lots of other useful features that may or may not be relevant to developers. It depends on the application. Topics in this chapter are intended to be high-level overviews, but they will give you a good starting point to learn more from the DWR website (http://directwebremoting.org/dwr/documentation) and even from the DWR source code.

Very often, most developers do not need all the features of a technology when they start working on a project. After the project has been going on for some time, we may find out that the solution has matured to include most if not all features of the technology. But that is typically not the initial case and, besides, we have to start somewhere and it is practical to start taking small steps at a time, like toddlers learning to walk.

## 3
Getting Started

In this chapter, we are going to set up the development environment for the coding work we will do in the following chapters. We will also have a look at supported environments, configuration, testing, and debugging.

As the development environment for the sample code in this book, we have the following components: Eclipse as IDE, Apache Geronimo 2.x.x as the application server, Java 1.5.x as Java runtime, Firefox 2.0.0.x as the client, and Windows as the operating system (Linux would work just as well).

This chapter includes the following sections:

- Supported Browsers and Environments – describes which software components are supported by DWR
- Configuration – presents DWR configuration, including `dwr.xml` and `web.xml` files
- Setting up a development environment – has steps to set up components for development
- Testing and Debugging – describes how to test and debug while developing applications using DWR
- Error Handling – describes how DWR handles error handling, and how it is configured to do it
- Packaging and deployment – shows how to package and deploy DWR applications using Eclipse and Geronimo

## Supported Browsers and Environments

As DWR applications are based on Java, the server that runs a DWR application must be a Java server. JDK 1.3 and servlet specification 2.2 are minimum requirements. Almost all Java application servers support DWR applications. Apache Tomcat, Apache Geronimo, WebSphere, Weblogic, and many other servers are supported. In this book, we have used Apache Geronimo.

DWR supports most of the latest browsers:

- Firefox 1.x and later versions are supported.
- Internet Explorer 6.0 and the later versions of Windows are supported (IE Mac is not supported). IE 5.5 may also work, but IE 5.0.2 is not supported.
- Mozilla-based browsers are supported from version 1.7.
- Opera version 7.5.4 and later are supported (only one minor issue reported with strings containing nulls).
- Safari browsers from version 1.2 on OSX are supported.
- Konqueror browser and its latest versions may work, but they are not tested by the DWR team.

In general, most current browsers are supported by DWR, and it is unlikely that browser support is an issue in any current or future development project. Especially, when the project is for SMB (Small and Medium Businesses) or an enterprise customer, they probably have a company policy for a preferred browser, and that browser is very likely one of the two major browsers.

## Configuration

DWR is configured using XML files, annotations, or Fluent Interface style. Using XML files is the most typical method for DWR configuration, and there are two files: web.xml and dwr.xml. The first one, web.xml, is a must for other configuration methods also. Annotations and fluent-style configurations may be used in place of dwr.xml. This section describes the configuration methods and how to use them.

### web.xml

Configuration starts by defining the DWR servlet in the web application's web.xml configuration file. This is, of course, mandatory since DWR would not work without it. A minimum configuration does not have any init parameters, and is shown as follows:

Initialization parameters for a DWR servlet also include several parameters for server load protection. The init parameters for load protection are listed in the following table:

| Parameter | Description |
|---|---|
| activeReverseAjaxEnabled | When set to true, this enables reverse AJAX support, polling, and Comet. The default value is false. |
| maxWaitingThreads | The maximum number of threads that wait for requests. The default value is 100. |
| maxHitsPerSecond | The number of poll requests that we should get per second. The default value is 40. |

Other init parameters also exist such as sessionCookieName and overridePath. The DWR web page lists all parameters, including a few undocumented parameters.

### dwr.xml

The other XML configuration file for DWR is dwr.xml. This file is used to configure remoted Java classes to JavaScript functions, and is the basis of DWR functionality.

The dwr.xml file has the following structure:

```
<!DOCTYPE dwr PUBLIC "-//GetAhead Limited//DTD Direct Web Remoting 2.0//EN" "http://getahead.org/dwr/dwr20.dtd">
<dwr>
  <init>
    <creator id="cr1" class="org.mysource.CreatorClass1"/>
    <converter id="co1" class="org.mysource.ConverterClass1"/>
  </init>
  <allow>
    <create creator="new" javascript="ServerClass"/>
    <convert converter="co1" match="org.mysource.MyDataClass"/>
  </allow>
</dwr>
```

The most important entries are within the allow element. Without the allow element, DWR is of no use since nothing is allowed. Entries within the init element are rarely used except when extending DWR. In addition to init and allow elements there can be a signatures element that holds method signatures and is used when our remoted objects have Collection parameters (such as List). When a method has a List parameter, DWR has no way of knowing what types List holds. That is why signatures are used to tell DWR that the List holds, for example, String objects.
