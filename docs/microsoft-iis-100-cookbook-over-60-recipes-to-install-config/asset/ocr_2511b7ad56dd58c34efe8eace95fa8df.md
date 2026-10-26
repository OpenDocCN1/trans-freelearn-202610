# 5

# Installing HTTP/2 on IIS 10.0

In this chapter, we will cover the following recipes:

+   - Understanding HTTP/2
+   - Installing HTTP/2 on IIS 10.0
+   - Configuring HTTP/2 on IIS 10.0
+   - Uploading .NET web pages
+   - Testing uploaded web pages

# Introduction

HTTP/2 is the newer version of HTTP/1.1, used to reduce the impact of latency and connection load on web servers by semantic flow over TCP connections. The HTTP/2 protocol can work on Windows 10 and Windows Server 2016. Windows 10 and Server 2016 both have the latest version of IIS Server, IIS 10.0. Currently, only IIS 10.0 supports HTTP/2. In HTTP/2, a persistent connection can be used to provide services to multiple simultaneous requests.

We know that every TCP connection requires a round trip to set up a connection between server and client. HTTP/2 sharply reduces the need for a request to wait while a new connection is established or wait for an existing connection to become idle.

HTTP/2 introduces HPACK, a compression scheme for HTTP headers that reduces the redundancy between requests. Compression helps multiplexing, because requests are smaller. This enables clients to make many requests in their first packets on a connection, while TCP flow control windows are still small.