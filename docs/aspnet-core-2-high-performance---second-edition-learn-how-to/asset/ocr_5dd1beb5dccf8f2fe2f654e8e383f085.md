The detail view is also very similar, including the **Timings** tab. This tab is shown in the following screenshot:

![](bbox=[0.16, 0.165, 0.838, 0.397])

# Fiddler

Sometimes, browser tools aren't enough. Say, you are debugging a native application, an HTTP client in your code, or a mobile browser. Fiddler is a free debugging proxy that can capture all of the HTTP traffic between the client and server. With a little bit of work, it can also intercept HTTPS traffic. Fiddler is available to download for free at http://www.telerik.com/fiddler.

As this book focuses on web development, we won't go into more detail. The browser dev tools are suitable for most of the work these days. They now fulfill a large part of the role that Fiddler used to play before they acquired the same features (network request capture, throttling, and timing). Fiddler is still there if you need it, and it can be handy if your web server calls an external HTTP API, although this can also often be debugged directly inside VS.

# Network

Occasionally, you will need to debug at a lower level than HTTP or SQL. This is where network monitors or packet sniffers come in. Perhaps, you want to debug a **Tabular Data Stream (TDS)** message to a SQL Server DB or a TLS handshake to an SSL terminating load balancer. Or, maybe you want to analyze a SOAP web service envelope or **Simple Mail Transfer Protocol (SMTP)** email connection, to see why it's not working correctly.