And then run the wget command to fetch a web page from the nginx server running on a different host:

```
wget -O- http://nginx
```

![](bbox=[0.1, 0.19, 0.89, 0.5])

The wget command will output the downloaded index page of nginx to the console: that's it – we have created a multi-host container network, using three separate machines. The first one was running the Consul server and enabling the remaining two hosts to find themselves and communicate. Just like with the bridge networks, you should use these overlay networks with small groups of containers that actually need to communicate with each other.

When creating a multi-host network, we opened a port on the Consul server. Exposing and publishing network ports is a great way to make the container be able to communicate with the other containers and the external world. Let's look at exposing and mapping ports in more detail.