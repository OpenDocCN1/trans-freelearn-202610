+   - Request Version: HTTP/1.1

# HTTP Methods

Some of the more common HTTP Methods are listed and described in the following table:

| Method | Description |
|---|---|
| GET | This retrieves information defined by the **Uniform Resource Identifier (URI)** field |
| HEAD | This retrieves meta data related to the desired URI |
| POST | This sends data to the HTTP server/application |
| OPTIONS | This determines the options associated with a resource |
| PUT | This sends data to the HTTP server/application |
| DELETE | This deletes the resource defined by the URI |
| CONNECT | This is used to connect to a proxy device |

# Host

The **Host** field identifies the target host and port number of the resource being requested. In the preceding screenshot, **Host** is `pktiqsvr1` on port `8080`.

# Request Modifiers

HTTP requests and responses use Request Modifiers to provide details for the request. In the preceding screenshot, Request Modifiers includes:

+   - **Connection:** This indicates the preference for a persistent connection (keep-alive).
+   - **Accept:** This is a list of data formats (`text/html` and `application/xhtml` plus `xml`) accepted.
+   - **User-agent:** This is a list of browser and operating system parameters (Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit) for the requesting device.
+   - **Accept-encoding:** This is a list of the acceptable HTTP compression schemes