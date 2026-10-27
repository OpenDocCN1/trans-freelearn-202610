
Notice that the connect portion of this configuration is slightly more involved. For the sidecar service, we need some additional parameters to tell the proxy server what service curl will be connecting to (httpd), as well as the local port on which the proxy server should listen for requests. Within my system, and I encourage you to play along, I have the httpd service running on one Consul client, with the curl process on another. However, when I try to look at these services within the Consul UI, I find they are not in a healthy state!

![](img/52479dafaefba00517c48a137e037b7e_178_0.png)

Figure 6.5 – Unhealthy httpd and curl services

What we can see in Figure 6.5, however, is that the services are in service mesh with proxy. Well, this is new! I wonder if the fact that the service is in a service mesh has something to do with the lack of availability. If we look at the service instances, we can see that there is this thing called Connect Sidecar Listening, which we also haven’t seen before. This looks to be the problem, and it obviously appears similar to the sidecar stuff we’ve been playing with.

![](img/52479dafaefba00517c48a137e037b7e_179_0.png)

In order to get this service healthy, the sidecar needs some attention. What we've done so far is register an httpd service in the catalog, and we've told Consul that the service will be accessible via proxy. However, we haven't done anything to start that proxy! When utilizing the service mesh, every application (unless it supports the mesh natively) requires its own proxy service. Therefore, if Consul isn't aware of any running proxy service that is paired with the service, the service will not be in a healthy state. To fire up the proxy server for the `httpd` service, all we need is a single command:

```
```bash
$ consul connect proxy -sidecar-for httpd-1 > ~/httpd-proxy.log
&
```
```

What we've done here is tell Consul to issue a connect request, utilizing the internal proxy, operating as a sidecar for the `httpd-1` service. *But wait, I thought it was just httpd!* How astute you are . . . I'm very impressed! When creating the proxy services, we utilize the **ID** of the service, not the **name** of the service. Any output of that request is being redirected to a log file, and we're running the command in the background. Once we run that command, the service is now healthy and happy within the Consul system.

Now, we have to get the curl service up and running. I find it strange calling curl a service since it's only a simple command line, but for our purposes, we are going to imagine curl is a client application accessing some web server. To get the proxy server for the curl service running, we're going to run a similar command:

```
$ consul connect proxy -sidecar-for curl > ~/curl-proxy.log &
```

Now we have proxy servers for both curl and our web server running. From the Consul UI, both services are healthy, and we can even see the topology for the service connections. The topology isn't very exciting at the moment (like curling), but as the number of services grows, having a view of what services are connected is critical to understanding the communication paths. The path that we've established is represented in Figure 6.7:

![](img/52479dafaefba00517c48a137e037b7e_180_0.png)

Figure 6.7 – Connected services via proxies

Let's walk through this flow a bit from the client's perspective. We have a proxy server running alongside a service we are calling curl. The definition of that service, specifically the local_bind_port, identifies a port (3579) that the proxy should be listening to for messages. Any messages that the proxy receives on that port are going to be sent upstream to the httpd service. Note that we have no port or address for that service configured; all we have is the service name used as the destination. On the server side, we have another proxy server running alongside our httpd service. We haven't defined a port for that service, the proxy server dynamically allocates the port when it starts. For this instance, the proxy server allocated port 21000. Because the httpd service is running on port 8080 on the internal localhost interface, the proxy server knows to forward requests to that port.

To demonstrate that we have the mesh working properly, we're going to pretend that we are that client application. So, emulating a client request, from the client machine, we are going to reach out to port 3579. That message will flow through both proxy servers to the httpd service:

```
$ curl http://localhost:3579
```

The response is a directory listing of the server node from /home/ubuntu, where the Python web server is running. If you want to double-check, you can use a utility called tcpdump:

```
$ sudo tcpdump -nnvvXSs 1514 -i any port 21000
```

If you run that command on the server node that is running the httpd service and then issue the curl command, you'll see packets received and sent. Don't worry, you aren't expected to be able to read the packet information. As a matter of fact, you won't be able to! The proxy servers are utilizing certificates provided by Consul to encrypt that communication. Although there is no encryption configured on the web server or the curl utility, the communication between the two devices is successfully encrypted. Furthermore, the routing that we did was all based on the service name, without any address configurations! At this point, you deserve a congratulatory virtual hug.

It is important to note at this point that the Consul server cluster isn't participating in the actual exchange of information between the client and the server. The data between these applications flows through the proxies directly to each other. This path of data is often called the data plane. Very inventive, I know. However, the information about those proxy servers that is necessary to establish that data connection is managed by Consul. All of the activities that Consul performs in this effort take place on the control plane. After all, these are the communications necessary to control your cluster! With that centralized control point, we have the ability to create rules to either permit or deny communications between services. Within the world of Consul, we call these intentions.

## State your intentions

Now that we understand how our applications will communicate over the service mesh, we need to understand how to configure and manage that communication. We may want to think that all of our services will live in harmony, and only those that should speak to each other shall do so. However, as altruistic as this idea is, it isn't very realistic. Therefore, we need a mechanism to easily define what applications can, and more importantly cannot, access one another within the Consul system. For this, we use what is called a Consul intention.

The nice thing about where we are with Consul services is that each service is defined by a human-understandable name. I would strongly recommend clear and distinct service names within any system simply to avoid ambiguity. However, that effort provides some additional comfort in that we can now specify intentions via the service name. If you've ever had to deal with any sort of IP-based firewall, you should find this very reassuring. Similar to an IP-based firewall, however, the Consul intention defines the connection based on the source of the communication method, and the destination service or application. The granularity of that control, however, depends on what protocol the service is defined to use.

The service definition enables the ability for the administrator to define the protocol on which the application resides. There are four options for the protocol: tcp, http, http2, and grpc. However, for intentions, we are going to focus on the first two, tcp and http.

The `tcp` protocol definition provides greater flexibility than the `http` protocol, as it extends supportability beyond `http` servers. If you want to utilize Consul for a database, for example, this uses the `tcp` protocol. In this instance, the identity of the application encoded within the mTLS certificate is a combination of IP address and TCP port. Intentions that manage these connections are called layer 4 intentions. If we look at the Application layer, layer 7, we can set intentions based on the actual URL utilized within the communication. This is necessary for service routing and splitting, which we discussed previously regarding the proxy servers.

One additional important distinction between layer 4 intentions and layer 7 intentions is when intention updates become effective. Without delving too deep, when there is a TCP connection between client and server, that connection is established and remains connected for the life of the communication path. For HTTP, however, the connection is only active for the request and response for the information. Therefore, layer 7 intentions may become effective much more quickly than layer 4 intentions. Consul doesn't tear down existing connections, so only when a new connection request is made will the intention have an impact.

Regardless of the layer at which the intentions are enforced, the configuration procedure is identical, and it only requires three pieces of information: the name of the service, the source of the communication request, and whether to allow or deny the connection. Before we get into the definition of the intentions, I want to revisit the ACL section of *Chapter 4, Data Center (Not Trade) Federation.*
