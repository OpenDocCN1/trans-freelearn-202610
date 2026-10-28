We won't provide detailed instructions here because this should be a cautiously considered decision, and you shouldn't apply advice blindly. You can easily find instructions online for the operating system that you use on your web server.

TCP slow-start is just one example of why you can't ignore the lower levels of internet technology, on the shoulders of which web applications stand. Let's move up the stack a little to the application layer.

# HTTP

As a web application developer who wants to deliver high performance, it's important to understand Hypertext Transfer Protocol (HTTP). You should know what version of HTTP you use, how it works, and how this affects things, such as request pipelining and encryption.

HTTP/1.1 is the version that you will probably be most familiar with today, because it has been in use for some time. HTTP/2 is becoming more popular, and it changes the best way to do many things.

# Headers

HTTP uses headers to provide metadata about a request along with the main payload in the body of the message, much like emails do. You won't see these headers when you view the source, but you can observe them using the browser developer tools. You can use headers for many things, such as cache control and authentication. Cookies are also sent and received as headers.

Browsers will only open a limited number of HTTP/1.1 connections at one time to a single host. If you require a lot of requests to retrieve all the assets for a page, then they are queued, which increases the total time taken to fully load it. When combined with the TCP slow-start mentioned previously, this effect can be amplified, degrading the performance. This is less of a problem with HTTP/2, which we will cover shortly. You can reduce the impact of this problem by allowing the browser to reuse connections. You can do this by ensuring that your web server doesn't send a Connection: close header with HTTP responses.