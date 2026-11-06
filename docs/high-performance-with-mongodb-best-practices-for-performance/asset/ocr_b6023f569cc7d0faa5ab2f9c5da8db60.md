# Connection churn

Creating and destroying connections frequently creates significant overhead, which can dramatically impact application performance and resource utilization. Connection churn refers to the rapid establishment and termination of database connections, which introduces several costly operations each time. Here are some examples:

+   - New connections require TCP handshakes, often with TLS negotiation. Each TCP handshake requires a minimum of one round-trip between client and server, while TLS negotiation adds multiple additional round-trips and complex cryptographic operations that can take hundreds of milliseconds to complete, especially across geographic regions.
+   - MongoDB’s SCRAM authentication requires multiple round-trips. This secure authentication mechanism performs a challenge-response sequence that typically needs 2-3 network round-trips before the connection is authenticated and ready for use, adding latency to every new connection.
+   - Both client and server must allocate memory and resources for each connection. On the server side, MongoDB allocates memory buffers, creates thread resources, and maintains state for each connection. On the client side, similar resources are allocated, including socket buffers and management structures, which can become significant with hundreds or thousands of connections.
+   - Modern TLS handshakes involve complex cryptographic operations including key exchange, certificate validation, and cipher negotiation. These operations consume CPU cycles, which at high connection rates can contribute to CPU saturation.

Connection churn is particularly problematic in serverless environments or microservices architectures where services frequently start and stop. In serverless environments, each function invocation might establish a new database connection if not properly managed. At scale, this can result in thousands of connection attempts per second during traffic spikes, overwhelming both the database server and the application infrastructure.

In later sections, we’ll explore how we can address connection churn by using connection pooling. We’ll also briefly discuss how we can minimize connection churn when using a serverless environment.

# Network saturation

When connections or bandwidth reach their limits, performance degrades rapidly, which can result in a number of potential issues emerging: