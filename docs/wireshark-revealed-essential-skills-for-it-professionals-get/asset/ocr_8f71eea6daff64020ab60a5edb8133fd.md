# # How it works...

SIP is an application-layer control protocol that is used to establish, maintain, and terminate calls between two or more end nodes.

SIP defines two basic classes of network entities—clients and servers:

+   - A client is an entity (or application) that sends SIP requests
+   - A server is an entity (or application) that responds to those requests

For connectivity to other network types, we have gateways. A gateway connects between SIP and **Public Switched Telephone Networks (PSTN)**, or SIP and H.323.

As illustrated in the following diagram, a client is made of **User Agent Client (UAC)** and **User Agent Server (UAS)**, and each client can initiate or respond to requests.

![](bbox=[0.074, 0.398, 0.566, 0.696])

SIP servers can be of various types:

+   - **Proxy Server:** This receives SIP requests from a user agent or another proxy and forwards or proxies the request to another location
+   - **Redirect Server:** This receives requests from a user agent or proxy, and returns a redirection response (3xx), indicating where the request should be present
+   - **Registrar:** This receives SIP registration requests and updates the user agent information to a location service or other database