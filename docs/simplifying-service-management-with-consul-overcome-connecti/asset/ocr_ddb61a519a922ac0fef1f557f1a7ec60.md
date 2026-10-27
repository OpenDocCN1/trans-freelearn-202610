Of course, these examples are all interactive with the Consul service, and it only provides the results of the watch. The following example specifies a checks type watch that will call the `check-handler.py` Python script whenever the check is executed:

```
watches = [
    {
        type = "checks"
        service = "httpd"
        args = ["/etc/consul/consul.d/check-handler.py"]
    }
]
```

In order to notify the external applications or services of the check results, including the location of our healthy services, the handler is able to post this information out to a multitude of services. One potential option is to update a messaging queue that services are subscribed to, such as **RabbitMQ** or **ActiveMQ**. Once Consul posts the service information, any component that is listening on the respective queue can retrieve the information.

There we have it! Throughout this section, you've learned three different ways to obtain information about the services within the Consul catalog, starting with DNS, which appeared to be the most straightforward, leading to the direct API integration, through to a notification mechanism that can notify just about anything or anybody!

# Summary

I hope you enjoyed this chapter…the topic of defining and managing our services isn't a horribly complex one, but it is critically important for the healthy operation of any dynamic network. A huge advantage of our cloudy world isn't the existence of cloud computing; it is how being able to quickly create and remove resources changes our human operational model. Unfortunately, that flexibility does mean our services have the ability to migrate and roam amongst the clouds. Using our basic example, we were able to register our `httpd` service within the Consul system, ensure that the service is healthy, and review the different methods that can be used to discover that service. Now would be a good time to take a break and play with the Consul system, as the next topic, *service mesh*, has quite a few more working pieces to it. Hang on…things are about to get meshy!