# Using a proxy host

A proxy will, essentially, make the requests to external servers instead of having JavaScript do it. We set up a proxy host with some server side script (using python, php, cgi, and so on) that will sit on our server in between our JavaScript code and external servers, so the JavaScript can (by using the proxy) talk to external servers and get around the inherent security restriction.

So, basically, JavaScript will send requests to the proxy host, and the proxy host will make external requests for JavaScript. You can write the proxy script yourself (which isn't recommended unless you really know what you're doing), or use the standard proxy host CGI file from OpenLayers. You can find it at http://trac.osgeo.org/openlayers/browser/trunk/openlayers/examples/proxy.cgi.

Once you have the proxy script set up on your server, you will need to tell OpenLayers to use a proxy host to make external requests. To do this, put the following in your code:

```
OpenLayers.ProxyHost = '/cgi-bin/proxy.cgi?url=';
```

You will need to replace the URL with the URL to your proxy script—this code assumes that you are using standard apache settings and are using the proxy.cgi file.

![](bbox=[0.195, 0.465, 0.255, 0.505])

> This step is not required for the previous example. If you have a server, running the previous example then the server does not require the use of a proxy host, as the data file is on the same server.

# Using the Vector Layer without a Protocol class

If you desire to just load data from some data source, for most use cases, the standard Protocol / Format / Strategy usage we just covered will work well. However, you do not have to use those three classes together. The classes can be used somewhat independently.

For example, we could use the Clustering class to cluster our point data without using the Format or Protocol classes. We could use the Format class outside our vector layer to read from and output feature data in a variety of different formats.

# Time for Action – using the Format and Strategy classes alone

Let's take a look at another example now. Here, we'll do something very similar to what we did in the first example—this time, however, we won't use the Protocol. Instead, we'll read in feature data as a JavaScript object and, using the GeoJSON Format class, read in the data. This way, this example will work without having to use a server or proxy host.