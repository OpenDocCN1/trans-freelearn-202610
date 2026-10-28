The web servers can now ask the cache if the data they need is in there before going to the database. This reduces load on the DB and is often quicker, as the data will be in memory if present. If there is a cache miss and the database does need to be queried, then the result can be written to the cache for other servers to use.

# Redis

Redis is a popular in-memory store that can also persist data to disk for permanent storage. It runs best on Linux, but a version is available on Windows for development purposes. Redis additionally supports pub/sub messaging, which can be useful for cache invalidation. You can read more about Redis at http://redis.io/.

You may wish to use the Windows version of Redis for local development work, but still deploy to the supported version on Linux. You can get the Windows version at http://github.com/MSOpenTech/redis or you could run the Linux version in a virtual machine, perhaps using Docker or Vagrant.

Redis cache is provided as a service on both Azure and AWS (ElastiCache offers both Memcached and Redis). You don’t need to manage your own server, but because the technology is not cloud-specific, you won’t get locked in if you want to migrate in the future.

As Redis keeps the entire dataset in memory but is also able to persist to disk it can be suitable as a primary data store, unlike Memcached. However, it is more commonly used only as a cache, especially if a cloud service is used and it’s paired with a cloud database such as Azure SQL Database or AWS Relational Database Service (RDS).

There are two recommended .NET C# clients for Redis: ServiceStack.Redis and StackExchange.Redis. The Stack Exchange client is used heavily on sites such as Stack Overflow and is easier to use correctly than the the ServiceStack one. You can read more about it at http://github.com/StackExchange/StackExchange.Redis and install it via NuGet.

If using caching at the application layer, then you will probably need to write a significant amount of custom code. You will also need to work out what format to serialize your data into for storage in the cache. If serving directly to browsers, then JSON could be useful. But if is to be used internally, then you may prefer a binary format such as MS Bond or protocol buffers.