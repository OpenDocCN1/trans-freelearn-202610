
The repository directory consists of two main sub directories called `services` and `modules`. Also, you may have an optional sub directory called `lib` as well. If you want to deploy a service, you need to drop the service archive file into the `services` directory. Similarly, if you want to deploy a module, then you need to drop a module archive file into the `modules` directory. The idea behind the `lib` directory is to store the third-party libraries that are going to be shared across both services and modules. Refer to the next screenshot to see the directory structure:

![](img/e2fb05de168053e6227dab034eff9353_92_0.png)

If one or more modules in the `modules` directory are required to share some resources, then they can add those resources into the `lib` directory inside the `modules` directory. Similarly, if the services in the `services` directory want to share some common resources, the proper place is the `lib` directory inside the `services` directory.

## Change in the way of deploying handlers (modules)

The concept of **service extension** is a new feature to the Apache Axis paradigm, but the developers have achieved the same goal doing a hard job in Axis 1.x. So the idea is to extend the core functionality of the system or to provide quality of services. In the case of Axis 1.x, if you need to extend its core functionality, you need to write a handler (the smallest unit in the execution chain), change the global configuration files to add the handler, and finally restart the system.

## Global descriptor or axis2.xml

As mentioned earlier, the configuration in Axis2 can be specified using XML descriptors. This gives you much more flexibility for extending and changing Axis2. No need to go and change the code to have a different configuration. Even for core functionality, it's the same. If you consider Axis2's global configuration file—axis2.xml—it has all the minimal configurations that are needed to run Axis2. Axis2's minimal configuration includes:

- Configuration parameters
- Transport senders
- Transport listeners
- Execution chains and phases
- Default dispatchers
- Default message receivers
- Default client-side configurations
- Global modules
- WS-Policy (global level policy)

Some of the given terms are not familiar to you, but you do not need to worry about them now. We will discuss these terms in the following chapters. Axis2 comes with default axis2.xml, and it has the minimum configuration required for starting Axis2. However, you can change it as you wish and can start Axis2 with your own axis2.xml. The key thing to remember here is that if you make any changes to axis2.xml, you have to restart the system in order to make those changes effective.

## Service descriptor (services.xml)

As we discussed in the previous section, axis2.xml specifies the configuration that affects the whole system. However, services.xml is for configuring a particular service or a service group. In the next section, we will look at the available ways of deploying services in Axis2. Archive-based deployment and directory-based deployment can be considered as the two most commonly used techniques. In these two cases, for a service to be a valid service, it is required to have a services.xml file. A service descriptor is used to specify the following types of configurations (some of these are optional):

- Name of the service
- Target name spaces of the service
- Session scope
- Expose transports
- Service level and operation level parameters
- Message receivers
- Service level modules
- Operations – expose operations as well as non expose operations
- Bean mapping
- Object suppliers
- Service and operation level policy

We will learn more about services.xml and how to write and create services in Chapter 7, Writing an Axis2 Service.

## Module descriptor or module.xml

It is so obvious now that module.xml is for configuring Axis2 modules; so it also has different types of configurations:

- Handlers and their phase rules
- Module parameters
- End points
- WS-Policy

In Chapter 8, Writing an Axis2 Module, we will learn more about Axis2 modules and how to write them.

## Available deployment options

In the initial stage of Axis2, it only had archive-based deployment. However, later it introduced a number of convenient deployment options. This relay made the service authors' jobs easier. Adding them all together, Axis2 has the following deployment options for service deployment:

- Archive-based deployment
- Directory-based deployment
- Deploying programmatically using archive files
- Programmatically making the Java class into a web service
- Plain Old Java Object (POJO) deployment support alone with annotation
- Deploy and start Axis2 in one line

## Archive-based deployment

The most common and recommended approach for deploying a service in Axis2 is **archive-based deployment**. There you get many configuration options and more flexibility compared to other types. In this chapter, we will discuss more about archive-based deployment with samples.

## Directory-based deployment

**Directory-based deployment** is almost identical to archive-based deployment; the only difference is that rather than creating an archive file, you can deploy a service as a directory. The structure of the directory is identical to that of an archive file.

## Deploying a service programmatically

To deploy a service programmatically using an archive file is not really a user requirement; rather it is module author's requirement, where some modules require a web service to be deployed at runtime in order to provide the full functionality of that particular module.

To create a service (`ServiceGroup`) programmatically, you need to have the file object representing the service archive file and a pointer to an Axis2 runtime or `ConfigurationContext`. Once you have those two, we can create a web service, as shown in the following code fragment. The advantage of this approach is that you do not need to copy your service archive file into the repository and it is only visible at the runtime of your service.

```
//Need to have a reference to ConfigurationContext
ConfigurationContext configContext = getConfigurationContext();
File serviceArchiveFile = new File("Location of the file");
//Now let's create AxisServiceGroup which contains the service we want to have
AxisServiceGroup serviceGroup = DeploymentEngine.loadServiceGroup(
    serviceArchiveFile,
    configContext);
```

Once you have created a service, the next step is to add the service into the system. You can do that as follows:

```
//Getting a pointer to AxisConfiguration
AxisConfiguration axiConfiguration = configContext.
getAxisConfiguration();
//Adding the created service
axiConfiguration.addServiceGroup(serviceGroup);
```

Now, if you try to access the add method using the following URL, you should get the same results as before:

```
http://localhost:6060/axis2/services/MyService/add?a=10&b=15
<ns:addResponse>
    <return>25</return>
</ns:addResponse>
```

## Summary

In this chapter, we discussed how Axis2 deployment works, the available types of deployment descriptors, and their structures. In addition, we also discussed the different types of configuration files available in Axis2. At the end of the chapter, we learned the most important thing, that is, the available deployment options in Axis2. We tested those options with a sample as well. The next step would be to create a few more services and see what happens.

In the next chapter, we will discuss more about the Axis2 information model. There we will discuss about static and dynamic data hierarchies, different entities in the hierarchy, how they are created, and which descriptors are responsible for the different entities.
