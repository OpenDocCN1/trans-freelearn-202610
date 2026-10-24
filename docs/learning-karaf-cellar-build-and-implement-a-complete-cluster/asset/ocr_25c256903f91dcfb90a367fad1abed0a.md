# 9
Roadmap

In the previous chapters, we saw that Cellar is more than just a simple provisioning cluster that provides advanced features (such as DOSGi) or leverages other projects (such as Camel).

Cellar's aim is to provide more runtime features. These new features can be grouped into the following three categories:

+   - **HTTP**: Cellar will be used by the Karaf HTTP layer to deal with the clustering of the deployed web applications
+   - **Monitoring**: Cellar will be used by the Karaf log and JMX service to provide a global monitoring solution for the cluster
+   - **Other projects**: Cellar can be used to run projects in Karaf, such as ActiveMQ, Camel, and CXF, to provide clustering features on top of these projects

This chapter introduces the new forthcoming features in HTTP and monitoring categories.

# HTTP load balancing and session clustering

Karaf directly leverages Pax Web to provide a complete WebContainer service.

With the installation of the war feature using the following command, Karaf gets full support from the web bundle thanks to the Jetty web container and Pax Web:

```
karaf@root> features:install war
```