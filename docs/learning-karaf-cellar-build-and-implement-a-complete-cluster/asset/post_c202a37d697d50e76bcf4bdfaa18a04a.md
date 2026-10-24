
## Apache Karaf – Provisioning
and Clusters

**Open Software Gateway initiative (OSGi)** has been "hidden" for a long time and reserved to middleware such as IDE or application servers. However, OSGi can be applied in a lot of different contexts and applications. An OSGi application needs an environment to run. Apache Karaf is a lightweight, powerful, and enterprise-ready OSGi container where you can deploy your applications. On a production system, especially a mission-critical platform, it makes sense to be able to manage a set of Apache Karaf containers and to spread the deployment (or provisioning) of applications to these different instances.

In this chapter, we will cover the following topics:

- What is OSGi and what are its key features?
- The role of the OSGi framework
- The OSGi base artifact – the OSGi bundle and the concept of dependencies between bundles
- The Apache Karaf OSGi container and the provisioning of applications in the container
- How to manage the provisioning on multiple Karaf instances?

### The OSGi bundle

In OSGi, the components are packaged as OSGi bundles. An OSGi bundle is a simple Java JAR (Java ARchive) file that contains additional metadata used by the OSGi framework. These metadata are stored in the manifest file of the JAR file.

The following is the metadata:

```
Manifest-Version: 1.0
Bundle-ManifestVersion: 2
Bundle-Version: 2.1.6
Bundle-Name: My Logger
Bundle-SymbolicName: my_logger
Export-Package: org.my.osgi.logger;version=2.1
Import-Package: org.apache.log4j;version="[1.2,2)"
Private-Package: org.my.osgi.logger.internal
```

We can see that OSGi is very descriptive and verbose. We explicitly describe all the OSGi metadata (headers), including the package that we export or import with a specified version or version range.

As the OSGi headers are defined in the META-INF/MANIFEST file contained in the JAR file, it means that an OSGi bundle is a regular JAR file that you can use outside of OSGi.

The life cycle layer of the OSGi framework is an API to install, start, stop, update, and uninstall OSGi bundles.

### Dependency between bundles

An OSGi bundle can use other bundles from the OSGi framework in two ways.

The first way is static code sharing. When we say that this bundle exports packages, it means a bundle can expose some code for other bundles. On the other hand, when we say that this bundle imports packages, it means a bundle can use code from other bundles.

For instance, we have the bundle A (packaged as the bundleA.jar file) with the following META-INF/MANIFEST file:

```
Manifest-Version: 1.0
Bundle-ManifestVersion: 2
Bundle-Version: 1.0.0
Bundle-Name: Bundle A
Bundle-SymbolicName: bundle_a
Export-Package: com.bundle.a;version=1.0
```

We can see that the bundle A exposes (exports) the com.bundle.a package with Version 1.0. On the other hand, we have the bundle B (packaged as the bundleB.jar file) with the following META-INF/MANIFEST file:

```
Manifest-Version: 1.0
Bundle-ManifestVersion: 2
Bundle-Version: 2.0.0
Bundle-Name: Bundle B
Bundle-SymbolicName: bundle_b
Import-Package: com.bundle.a;version="[1.0,2)"
```

We can see that the bundle B imports (so, it will use) the com.bundle.a package in any version between 1.0 and 2 (excluded). So, this means that the OSGi framework will wire the bundles, as the bundle A provides the package used by the bundle B (so, the constraint is resolved).

This mechanism is similar to regular Java applications, but instead of embedding the required JAR files in your application, you can just declare the expected code. The OSGi framework is responsible for the link between the different bundles; it's done by the modules layer of the OSGi framework. This approach is interesting when you want to use code which is not natively designed for OSGi. It's a step forward for the reuse of components. However, it provides a limited answer to the purposes seen earlier in the chapter, especially visibility and discovery.

The second way in which an OSGi bundle can use other bundles from the OSGi framework is more interesting. It uses Service-Oriented Architecture (SOA) for low-level components. Here, more than exposing the code, an OSGi bundle exposes a OSGi service. On the other hand, another bundle can use an OSGi service. The services layer of the OSGi framework provides a service registry and all the plumbing mechanisms to wire the services.

The OSGi services provide a very dynamic system, offering a Publish-Find-Bind model for the bundles.

![](img/86f349f6a758f2c3088dbeb4d35dcaca_22_0.png)

### Downloading the example code

You can download the example code files for all Packt books you have purchased from your account at http://www.packtpub.com. If you purchased this book elsewhere, you can visit http://www.packtpub.com/support and register to have the files e-mailed directly to you.

- Apache Karaf is an enterprise-ready container and provides features dedicated to enterprise. The following enterprise features are not installed by default (to minimize the size and footprint of the container by default), but a simple command allows you to extend the container with enterprise functionalities:
  - WebContainer allows you to deploy a **Web Application Bundle (WAB)** or WAR file. Apache Karaf is a complete HTTP server with JSP/servlet support, thanks to Pax Web.
  - **Java Naming and Directory Interface (JNDI)** adds naming context support in Apache Karaf. You can bind an OSGi service to a JNDI name and look up these services using the name, thanks to Aries and Xbean naming.
  - **Java Transaction API (JTA)** allows you to add a transaction engine (exposed as an OSGi service) in Apache Karaf, thanks to Aries JTA.
  - **Java Persistence API (JPA)** allows you to add a persistence adapter (exposed as an OSGi service) in Apache Karaf, thanks to Aries JPA. Ready-to-use persistence engines can also be installed very easily (especially Apache OpenJPA and Hibernate).
  - **Java Database Connectivity (JDBC)** or **Java Message Service (JMS)** are convenient features, allowing you to easily create JDBC DataSources or JMS ConnectionFactories and use them directly in the shell console.
- If you can completely administrate Apache Karaf using the shell commands and the JMX MBeans, you can also install Web Console. This Web Console uses the Felix Web Console and allows you to manage Karaf with a simple browser.

Thanks to these features, Apache Karaf is a complete, rich, and enterprise-ready container. We can consider Apache Karaf as an OSGi application server.

### Provisioning in Apache Karaf

In addition, Apache Karaf provides three core functionalities that can be used both internally in Apache Karaf or can be used by external applications deployed in the container:

- OSGi bundle management
- Configuration management
- Provisioning using Karaf Features

As we learned earlier, the default artifact in OSGi is the bundle. Again, it's a regular JAR file with additional OSGi metadata in the MANIFEST file. The bundles are directly managed by the OSGi framework, but for convenience, Apache Karaf wraps the usage of bundles in specific commands and MBeans.

A bundle has a specific life cycle. Especially when you install a bundle, the OSGi framework tries to resolve all the dependencies required by your bundle to promote it in a resolved state. The following is the life cycle of a bundle:

![](img/86f349f6a758f2c3088dbeb4d35dcaca_26_0.png)

The OSGi framework checks whether other bundles provide the packages imported by your bundle. The equivalent action for the OSGi services is performed when you start your bundle. It means that a bundle may require a lot of other bundles to start and so on for the transitive bundles.

Moreover, a bundle may require configuration to work. Apache Karaf proposes a very convenient way to manage the configurations. The etc folder is periodically monitored to discover new configuration files and load the corresponding configurations. On the other hand, you have dedicated shell commands and MBeans to manage configurations (and configuration files). If a bundle requires a configuration to work, you first have to create a configuration file in the etc folder (with the expected filename) or use the `config:*` shell command or ConfigMBean to create the configuration.

Considering that an OSGi application is a set of bundles, the installation of an OSGi application can be long and painful by hand.
