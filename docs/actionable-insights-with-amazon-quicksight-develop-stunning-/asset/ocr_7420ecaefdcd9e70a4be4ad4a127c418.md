# # Exploring the architecture and user authentication

In this section, we will focus on the architectural components of embedded analytics. To understand this end-to-end architecture, we will break it down into three layers:

+   - Web application layer
+   - BI and data layer
+   - Authentication and authorization layer

This is better represented with the following diagram:

![](bbox=[0.115, 0.337, 0.848, 0.662])

Figure 7.1 – Generic architecture for embedded analytics

Generally, a client typically accesses a web app or a web portal using their web browser. In many cases, the user will need to authenticate with the web app. The client will present user credentials (typically, a username and a password) to the **authentication layer**, which sends back an access code/token so that the client can communicate with the **web application layer**. In embedded analytics, the web application layer will be responsible for getting the embedded visuals from the **BI layer**, which, in turn, would be responsible for querying the data from the data sources and visualizing them (**data layer**).

In the next section, we will analyze each of these architectural components and identify AWS services that can be used to build them.