You shouldn't need to configure Varnish too much if you are using HTTP caching headers correctly. However, you can also use the custom HTTP PURGE method to remove entries from the cache, which works with the Squid proxy software too. You may occasionally see a cryptic guru meditation error if Varnish is not properly configured, but you should be able to track down the issue in the Varnish logs. It could indicate that no healthy web servers are available.

Varnish configuration is beyond the scope of this book, but it's very well documented on the Varnish website. If you don't want to run your own proxy server, then you could use a CDN. You may still want your own proxy in addition to using a CDN, as large CDNs, with many points of presence (PoP), might request the same resource via each PoP, and not share assets across them. This can be an issue if you pay a lot for bandwidth, although some CDNs have a feature (often called origin shielding) that can help with this.

# Working with a content delivery network

A CDN is commonly used in two ways, as a proxy for offloading your content or as a hosting provider for common third-party libraries and frameworks. You can use a dynamic CDN service, such as CloudFlare or Akamai, for the first use case, but the second situation (using a static CDN from Google or Microsoft) is more common and that's what we'll cover here.

Although using a CDN for your libraries, such as jQuery and Twitter bootstrap, is becoming less useful with the adoption of HTTP/2, it can still be helpful for reducing your hosting costs. Also, if you use a popular CDN and library, then a user may already have a copy of some required assets. For example, if the user has been to another site that uses jQuery from Google's CDN, then it may already be in their browser cache.

It is essential to have a fallback copy of whatever files you require from a CDN. This is easier than ever with the Razor view engine support built into ASP.NET Core.

The following code shows how jQuery is included in the default MVC Razor layout, for non-development environments. Both the CDN and local versions are specified along with a test:

```
<script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js"
asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
asp-fallback-test="window.jQuery"
crossorigin="anonymous"
integrity="sha384-K+ctZQ+LL8q6tP7I94W+qZsfRV2a+AfHIi9k8z81g gpc8X+Ytst4yBo/hH+8Fk">
</script>
```