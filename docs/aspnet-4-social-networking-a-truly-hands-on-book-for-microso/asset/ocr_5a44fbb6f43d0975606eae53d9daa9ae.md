# Configuration

We have demonstrated three caching solutions one uses System.Web.Caching (check Cache.cs in the code), another with AppFabric Caching, and last one using MemCached. For this we have built one interface ICache and built a specific implementation wrapper Cache.cs, AppFabricCache.cs, and MemcachedCache.cs for each of the solution respectively in Components project.

In the real world you will need only one solution; whereas we have shown three so that you can build an understanding with each one. When you want to actually run the project you should use just one caching solution and exclude the other two caching class implementations from Components project because failure to do so will have MEF raise an exception at runtime. This happens because for ICache interface exporting MEF will find three matching imports (with the three class implementations) and the composition container will not know which one to use.

However, MEF also provides a solution to such a problem by providing ImportMany attribute, which can match multiple Import interfaces to one Export interface. Alternatively, one can mention the priority of matching Import in [Metadata] attribute and the highest priority Import will get assigned. However, to keep things simple you can just exclude the other two class files here.

# Where do I start?

The easiest way to know where to put caching is to analyze the performance of your site and find the slowest parts. There are at times sections of code that may benefit from a tweak to squeeze out better performance. But frequently, there are highly optimized sections of code that just can't run any faster. These are the areas that may benefit from a cache wrapper. To get major gains from your caching you don't necessarily need to stow your items away in the cache for days on end. You can see gains by caching something for only a few seconds. The key is how frequently your data changes, and how frequently that data is accessed by your users. If the data changes constantly and it is accessed constantly, huge gains can be made from seconds of caching. If the data doesn't change much you can put it into the cache and remove it only when your data changes.

# Email

How can sending email make the site slow? If you think about the normal emailing process, you will find that the web server has to, at the very least, connect to an SMTP server. This connection can take time. If you need to send hundreds of emails, then you need to possibly make the same amount of connections to an SMTP server.