This route will react as soon as the cache content changes. The `hazelcast:map` endpoint sets some headers to get details about the change, especially the following changes:

+   - `CamelHazelcastListenerAction`: This contains the action performed on the cache, that is, add, remove, update, or evict
+   - `CamelHazelcastListenerTime`: This is when the change has been performed
+   - `CamelHazelcastObjectId`: This is the ID of the object that has changed
+   - `CamelHazelcastCacheName`: This is the name of the distributed map that has changed

Thanks to the `hazelcast:map` endpoints, you can easily implement a distributed cache mechanism.

# Summary

In this chapter, we saw that by leveraging Hazelcast, Cellar offers more than a simple provisioning framework. Combined with Camel, it's a good solution to spread your execution logic over multiple nodes and to implement a fully-scalable and distributed system. It brings more flexibility to Camel in terms of provisioning and executing the routes in a clustered environment.

The `camel-hazelcast` component provides other data structures than the ones introduced in this chapter. You can find details on the component documentation at http://camel.apache.org/hazelcast-component.html.

In the next chapter, we will see new directions and features for preparation in Cellar, providing even more clustering solutions.