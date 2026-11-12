they are proposed to be used in the chapters to come. Some of the popular ESB tools that are used with Salesforce today are MuleSoft, webMethods Integration Server, IBM Integration Bus, TIBCO ActiveMatrix Service Bus, and WSO2 Enterprise Integrator.

# ## Reverse Proxies

A reverse proxy is the opposite of a forward proxy; while a forward proxy is used as an intermediary by the client to connect to a server, a reverse proxy is something the server (or servers) puts between itself and potential clients.

For the end client, any retrieved resources would appear as if they originated from the proxy server itself, rather than the server or servers that are behind it. A reverse proxy is often used to provide a more secure interface to deal with untrusted clients (such as unauthorized internet users), as well as shielding the other applications behind it that might lack the ability to handle the excessive load or are unable to provide the right security measure required (such as the inability to support HTTPS).

A reverse proxy can provide capabilities such as transforming HTTPS requests into HTTP, handling cookies and session data, transforming one request into multiple requests behind the scenes, and then combining the responses and buffering incoming requests to protect the shielded servers from excessive load. Some providers of reverse proxy products are VMware, Citrix Systems, and F5 Networks.

# ## API Gateways

**API gateways** are historically used to protect your internal web services (or APIs—remember that the two terms can be used interchangeably in the web context as most modern APIs are offered as web services). The enterprise's internal APIs might not be designed to handle topics such as authentication and scalability, so an API gateway would provide a layer on top to protect the APIs, as well as enabling a set of other functionalities, such as monetizing the APIs, providing real-time analytics, and protection against **denial of service (DOS)** attacks.

API gateways are very similar in concept to the reverse proxies and can be described as a special type of reverse proxy. On some occasions, you might have both in your landscape, where the API gateway sits behind the reverse proxy that handles load balancing. API gateways can normally be configured via an API or a UI. On the other hand, reverse proxies are normally configured via a config file and require a restart to use a new set of configurations. API gateways also provide advanced API functionalities, such as rate limiting, quotas, and service discovery.

As a Salesforce Architect, you need to know about some of the popular API gateways that are in use today, such as MuleSoft, Microsoft's Azure API Management, Google (Apigee), and IBM API Management.