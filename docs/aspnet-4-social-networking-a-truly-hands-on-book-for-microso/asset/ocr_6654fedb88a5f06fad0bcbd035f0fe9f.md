# Scaling Up

Caching, like indexing, comes with some downfalls. If we put data into our cache and then modify / delete that data from the database, we may find that our site is using stale data. For example, if we try to create blog entries with an account that no longer exists, we will get a database error. So while implementing a caching layer we must be vigilant in making sure that our data is kept clean.

Another problem is if we have data in the cache that is stale but valid. We again end up showing data that is incorrect to the user. It is important that we maintain this side of the clean data equation too!

One more aspect is to decide how much data to cache? Cache too much data and you end up with out-of-memory problems as cached data is going to consume memory on the server. If the intent is to cache large data, you should plan for servers with large RAMs (multi GB to TB) which are commonly used in social networking sites.

# Caching—AppFabric solution

Windows Server AppFabric caching services are useful for building highly scalable and available applications in distributed and non-distributed scenarios. Let's see how we can use this in our solution.

# Client

In Components project, create an AppFabricCache class. This class will implement ICache interface and expose AppFabric Caching APIs. The current interface ICache (we had looked at this in Chapter 2) has limited methods than what AppFabric caching services can provide. However for now we will stick with these many only. If you intend to use more of the AppFabric APIs, you can add the corresponding method signatures in ICache interface and implement the same in AppFabricCache.cs class.

We need to add reference of the following AppFabric caching assemblies to the Components project.

+   - `Microsoft.ApplicationServer.Caching.Client.dll`
+   - `Microsoft.ApplicationServer.Caching.Core.dll`

These assemblies contain the cache methods that will be used by the application. The assemblies can be found in ~:\Windows\System32\AppFabric folder.

The AppFabricCache wrapper class with relevant methods is as shown below. We have implemented a few get and set methods. There are lot many APIs provided by AppFabric caching service which can be wrapped up, but for simplicity we are just focusing on two methods and in other methods we are just throwing exception. See the full code for this class in the code associated with this chapter.