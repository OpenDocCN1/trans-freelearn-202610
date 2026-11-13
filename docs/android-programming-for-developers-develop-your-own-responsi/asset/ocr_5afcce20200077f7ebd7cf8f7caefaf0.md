# Introducing Android HTTP clients

In recent times, the ability to send and receive data from remote servers has become an essential feature that all applications should enforce in order to create dynamic and impressive experiences. Today almost every application uses the network to pull up data information, execute remote business logic operations, and download or upload resources.

The network interactions that happen between the application and a remote server are typically defined as a set of request/response messages that traverse the network using a network protocol.

In general, the HTTP protocol is often used to transport messages between each peer, and the Android SDK comes with two high-level HTTP clients available out of the box to send and receive data: AndroidHttpClient and HttpURLConnection.

The HTTP communication protocol is a stateless, standard text-based application protocol maintained by Internet Engineering Task Force (IETF) and the World Wide Web Consortium (W3C) and is widely used on the Internet to exchange data between a client, normally called a user agent, and a server.

The protocol has undergone some improvements over time, but most servers and clients base their implementation on HTTP 1.1, a revision of the original HTTP 1.0, which introduced the connection re-use feature and chunked transfer encoding to the original protocol.

In a typical HTTP flow, the client, the entity that initiates the operation, sends a request to the server over a connection and waits for the server's response. On the other end, the server reads the request from the communication channel, processes the request, and sends a response back to the client. In the next figure, we can visualize a request and response example exchanged between peers: