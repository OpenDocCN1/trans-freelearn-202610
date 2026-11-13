# Chapter 6. Network Interactions with GCM

In previous chapters, in order to update any kind of dynamic data that our examples required, we explicitly initiated a connection to a remote server, waking up the network radio and other resources required to perform the network operation. The application might fetch either fresh data or exactly the same data if nothing has changed since the last fetch.

Although this communication-fetching model might work for most use cases, it could consume battery resources and internet bandwidth in vain when the data does not change often.

This technique, commonly known as data polling, may also increase the server load when a great number of clients try to fetch or verify whether any data has changed.

An alternative to the polling technique is data pushing. In this technique, the server tells the application when new data is available or when the data has changed. When the data consumer (application) gets notified, it will initiate a new interaction with the server to retrieve the fresh data.

Since fewer synchronizations are required, it will lead to fewer network interactions, which will lead to less battery resources consumed.

In this chapter, we will introduce you to Google Cloud Messaging (GCM), a service delivered by Google Play Services that will help you to build applications that require data pushing or pulling messaging services. GCM delivers a framework to deliver push messages to multiple devices or group of devices in a battery-efficient way.

In this chapter, we will cover the following topics:

+   - Polling versus pushing messaging
+   - How to setup and configure GCM for your application
+   - Receiving downstream messages from the server with GCM
+   - Receiving downstream messages from GCM topic streams