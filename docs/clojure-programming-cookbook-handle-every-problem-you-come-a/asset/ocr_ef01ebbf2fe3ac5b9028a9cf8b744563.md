Anyway, where in your application you can now use this messaging mechanism? By registering to messages coming from the messaging system.

It would not be a proper messaging system if it could only talk to itself, so let's see how to set up to connect remotely!

# Remote messaging

Now we want to set up two servers talking to each other using WildFly messaging queues.

The first server setup is almost identical to the previous example, except that to connect remotely, we need to bind to a different IP address, externally accessible. This is done by adding JVM settings to the Leiningen JVM option, so in project.clj, we will add the following:

```
:jvm-opts ["-Dactivemq.netty.host=0.0.0.0"]
```

And if you look carefully in the console logs, you will notice that indeed the messaging system is bound using the preceding settings:

```
22:46:55.000 INFO [org.apache.activemq.artemis.core.server]
(main) AMQ221020: Started Acceptor at 0.0.0.0:5445 for
protocols [CORE]
22:46:55.003 INFO [org.apache.activemq.artemis.core.server]
(main) AMQ221007: Server is now live
```

Wonderful. Now on to setting up the second server. In the context of this book, the second server is actually running on a Raspberry Pi again, and the most difficult thing is actually to get the IP addresses in the proper order.

This second server uses Clojure code similar to what you have seen up to now. Queue lookup and ring handler routing are basically the same. What changes is the way to retrieve the queue from a remote location.

The first serer was on IP 192.168.1.3, so the following example will