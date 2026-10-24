![](bbox=[0.11, 0.09, 0.85, 0.27])

Figure 2.16: requests library – HTTP for Humans

We took a look at requests in the Installing libraries section. For our purposes, we are using requests version 2.28.1.

Compared to other HTTP libraries in Python, requests is highly rated because of its readable, developer-friendly, and simple-to-use attributes. Some of its main qualities are as follows:

+   - Short, simple, and readable functions and attributes
+   - Access to various HTTP methods
+   - Automatic content decoding and decompression
+   - Easy processing of query strings
+   - Customization of HTTP headers
+   - Session and cookie processing
+   - Ability to deal with JSON requests and content
+   - Proxy support
+   - Multipart file uploads (form handling)
+   - Connection pooling and timeouts

We will be using the requests library, and we’ll explore some of its main properties. In the Loading URLs section, we used the get() method to load a URL and view its source using content. We were actually using the HTTP GET method to communicate with the URL using get().

The requests library also supports other HTTP methods, such as PUT, POST, DELETE, HEAD, and OPTIONS, using the put(), post(), delete(), head(), and options() methods, respectively.

With this brief introduction and overview of requests, we will cover some more details with code examples in the following sections.