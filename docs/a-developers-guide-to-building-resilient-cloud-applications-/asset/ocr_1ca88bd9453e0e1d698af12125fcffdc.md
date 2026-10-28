and actors. The difference between stateful and stateless services is that in stateless services, no state is maintained in the service, but for stateful services, the state is stored with the service.

Moreover, we can use both .NET Core and .NET Framework runtimes to build reliable services. This is why we can use Service Fabric to migrate old applications. An actor is a dependent computing unit that runs on a single thread. The actor model is intended for concurrent or distributed systems since a large number of these actors can run simultaneously and independently of each other, communicating through messages.

Another application model is containers. Both Linux and Windows containers are supported in Service Fabric.

There is also the guest executable model. It allows us to bring any type of executable that we have built on any development platform and with any programming language to Service Fabric.

With these application models, we can use Service Fabric in any scenario that requires high availability, reliability, and a robust platform to deploy and run our applications.

We’re going to create a sample solution based on Service Fabric’s **Stateless Reliable Services**. The application is used for managing hospitals around some countries in Africa and Europe.

# Creating a Service Fabric application

Let’s go back to Visual Studio 2022. When we select **Create a new project**, we need to configure the project by specifying the name, the location, the solution name, and the framework. After, we will click on the **Create** button.

![](bbox=[0.131, 0.55, 0.856, 0.871])

Figure 5.19 – Creating a Service Fabric application