
We can address this design change in two ways. We can either design the service operations in a completely autonomous manner with all relevant in-out parameters OR we can pass the HttpContext and Session information to the WCF Service.

10. While the first one is a better approach, it is more intrusive and will need a lot more changes in our code. For now we will go with the second option. For this we need to add an attribute to decorate our service:

```
[AspNetCompatibilityRequirements(RequirementsMode = AspNetCompatibilityRequirementsMode.Required)]
public class AccountService : IAccountService
```

And in WcfServiceHost project's web.config file, mark aspNetCompatibilityEnabled as true.

```
<system.serviceModel>
    <serviceHostingEnvironment multipleSiteBindingsEnabled="true" aspNetCompatibilityEnabled="true">
        <serviceActivations>
            <add relativeAddress="AccountService.svc" service="Fisharoo.BusinessLogic.AccountService"/>
        </serviceActivations>
    </serviceHostingEnvironment>
</system.serviceModel>
```

With this we are done with converting AccountService into a WCF Service and consuming it into Web client project.

## Web Server and application server—Windows IIS 7.5 and AppFabric

It is important that we deploy the Presentation and Application tiers onto the software that can scale. We will use Windows Internet Information Server (IIS) 7.5 for deploying the presentation layer (Web project) and Windows Server AppFabric for deploying services, business, and data access logic.

Windows Server AppFabric is a Microsoft server for hosting and managing WCF and Workflow Foundation (WF) services and also provides distributed caching services. It gets installed on top of IIS 7 or above. Deployment of the Web and WcfServiceHost projects in IIS can be done as follows.

Ensure you have IIS 7.5 installed (assuming you are running on Windows Server 2008 R2). Windows Server AppFabric is free and can be downloaded from the following site:

http://www.microsoft.com/downloads/en/details.aspx?displaylang=en&FamilyID=467e5aa5-c25b-4c80-a6d2-9f8fb0f337d2

### Scaling Up

You can refer to this link for information about the software's installation and configuration: http://msdn.microsoft.com/en-us/library/ff637707.aspx

Once installed, launch IIS and deploy the Web project by:

- Right-clicking on **Default Web Site** and selecting **Add Application**.
- Providing the alias for application pool and ensuring that you selected a dedicated app pool running under .NET v 4.x.x.x (a dedicated App pool can be created under **Application Pools** node).

![](img/15a62e134135dde036b0ac3cbb01dab6_426_0.png)

> It is important that the application gets a dedicated application pool. This helps in ensuring the necessary application isolation level as each application pool runs in a dedicated w3wp process. If more than one application is run under the same application pool and w3wp crashes due to any issues in any one application, then the other applications running under the same w3wp will also crash.

With this, you will see the **Fisharoo** Web application is created under the default web site.

Now let's look at how to deploy the services. For this, first we need to package the services. For packaging, select the WcfServiceHost project, right-click, and provide package properties such as the **create deployment package as a zip file** option, the location where the package will be created, and the web application name.

![](img/15a62e134135dde036b0ac3cbb01dab6_427_0.png)

Once this is done, right-click the project and select **build deployment package**. This will create a deployment package which we need to import onto IIS. Right-click **Default web site | Deploy | Import** and provide the path from where to import.

This will get the Services installed. You can configure, manage, and monitor the WCF services from AppFabric's dashboard.

## Web farming

**Web farming** (or load balancing) is the concept of spreading out your site's traffic over many servers. This addresses most of the performance problems whether in the application or on the server, by expanding your infrastructure and sharing the load across many servers. Generally, whoever is doing your load balancing will take a look at all the servers in your farm and route the user to the server with the lowest load. There are several ways to do this, and we will touch upon many of them lightly.

We are going to assume that we are interested in learning how to create a web farm in a Windows environment.

> For those who need a Linux solution you might try **HAProxy** (http://haproxy.1wt.eu/) or something similar.

Specifically, we are going to talk about setting up a web farm using Windows' **Network Load Balancing (NLB)**. This allows you to host a handful of servers behind one virtual IP address on your network.

For this to work, each server in the farm needs to be configured identically so that any request that goes to any server will be handled in the same fashion. When a request comes to the virtual IP address, the packet will be routed to the least busy server. This configuration allows support of the concept of a reliable service. Meaning that if one server goes down, the farm will simply rebalance itself and route traffic to the servers that are still up.

To set this up, you need at least two machines running Windows Server 2008. Each machine will need to have one fixed IP address. While the machine can run on one network card, two cards are preferred—one card having the fixed IP address and the other using the virtual IP address. The IP addresses must be on the same class C network.

Windows Server 2008 comes with limited preinstalled features. You will need to enable **Network Load Balancing** on it. For this, go to **Server Manager** from **Administrative Tools** and in the **Features** section select **Add Features** option.

![](img/15a62e134135dde036b0ac3cbb01dab6_429_0.png)

Once installed, open the **Network Load Balancing Manager** from **Administrative Tools**. Right-click on the **Network Load Balancing Clusters** and select **New Cluster** and continue to follow the wizard steps to configure load balancing.

For more detailed information about setting up a network load balanced web farm in Windows Server 2008, check out:

- http://technet.microsoft.com/en-us/library/cc771300(WS.10).aspx
- http://www.techotopia.com/index.php/Building_a_Windows_Server_2008_Network_Load_Balancing_Cluster
- http://telnetport25.wordpress.com/2008/03/24/quick-tip-configuring-network-load-balancing-nlb-on-windows-2008-for-exchange-cas-servers/

## Caching

Typically, to cache is to take some data or objects from backend database and store them in memory for later use. When we need the data/object, we can simply retrieve it from the cache (memory). In terms of caching data from a database, we can take queried results and store them in memory. This means that when we need the information again, we don't have to perform a server-to-server connection, we don't have to wait for the query to be executed, and we don't have to wait for the results to be sent back to us. We can just pluck the data from RAM. Take this one step further up the chain and we can enjoy further optimizations. We can take the data from the database, parse that data, build a list of fully hydrated objects, and then store that list in the cache. This means that in addition to skipping the database stuff we can also skip creation and hydration of our objects.

### Scaling Up

Caching, like indexing, comes with some downfalls. If we put data into our cache and then modify / delete that data from the database, we may find that our site is using stale data. For example, if we try to create blog entries with an account that no longer exists, we will get a database error. So while implementing a caching layer we must be vigilant in making sure that our data is kept clean.

Another problem is if we have data in the cache that is stale but valid. We again end up showing data that is incorrect to the user. It is important that we maintain this side of the clean data equation too!

One more aspect is to decide how much data to cache. Cache too much data and you end up with out-of-memory problems as cached data is going to consume memory on the server. If the intent is to cache large data, you should plan for servers with large RAMs (multi GB to TB) which are commonly used in social networking sites.

### Caching—AppFabric solution

Windows Server AppFabric caching services are useful for building highly scalable and available applications in distributed and non-distributed scenarios. Let's see how we can use this in our solution.

#### Client

In Components project, create an AppFabricCache class. This class will implement ICache interface and expose AppFabric Caching APIs. The current interface ICache (we had looked at this in Chapter 2) has limited methods compared to what AppFabric caching services can provide. However for now we will stick with these many only. If you intend to use more of the AppFabric APIs, you can add the corresponding method signatures in ICache interface and implement the same in AppFabricCache.cs class.

We need to add references to the following AppFabric caching assemblies to the Components project.

- `Microsoft.ApplicationServer.Caching.Client.dll`
- `Microsoft.ApplicationServer.Caching.Core.dll`

These assemblies contain the cache methods that will be used by the application. The assemblies can be found in ~:\Windows\System32\AppFabric folder.

The AppFabricCache wrapper class with relevant methods is as shown below. We have implemented a few get and set methods. There are many APIs provided by AppFabric caching service which can be wrapped up, but for simplicity we are just focusing on two methods and in other methods we are just throwing exceptions. See the full code for this class in the code associated with this chapter.
