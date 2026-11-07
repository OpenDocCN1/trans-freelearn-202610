# Docker networking

Docker uses the concept of the **Container Network Model (CNM)**. In essence, CNM allows you to create small, segmented networks for groups of containers to communicate over. Let's explain the model using the following diagram:

![](bbox=[0.09, 0.22, 0.93, 0.38])

There are three entities present in the CNM:

+   - **sandbox:** This is an isolated environment holding the networking configuration for a container.
+   - **endpoint:** This is a network interface that can be used for communication over a specific network. Endpoints join to exactly one network. Multiple endpoints can exist within a single network sandbox.
+   - **network:** It's a group of endpoints that are able to communicate with each other. You could create, for example, two separate networks, and they will be completely isolated. Network can be identified by their names (such as **backend** and **frontend**) or IDs (generated automatically by Docker during the network creation).

Docker networking model also introduces the concept of network drivers. By default, two network drivers are provided by Docker: the `bridge` and the `overlay` driver. We will get back to it in a moment.

All containers on the same network can communicate with each other freely. An endpoint provides network connectivity for a sandbox – if you need to join a container to multiple networks, there should be multiple endpoints per container. This networking model is highly configurable (and allowing quite complex networking configurations), but also quite easy to set up using just the defaults. In fact, Docker creates some default networks for us out of the box.