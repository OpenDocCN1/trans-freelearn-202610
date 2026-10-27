In step 2, the client first opens a connection to www.google.com on port 80 (the port that serves web pages). After the socket is connected to the server, the IP address and port are printed to the screen. Then, the socket is shut down using socket.destroy. In the second part of this step, we connect to the local socket server, write a message to it with socket.write, and start listening to the server with socket.listen. We transform the data that comes in as a list of integers into a string that is printed out.

# There's more...

Socket communication is often blocked by firewalls; if this is an issue, take a look at WebSockets in the following recipe.

Dart also supports UDP socket programming; the article by James Locum offers a detailed discussion at http://jamesslocum.com/post/77759061182.

# Using WebSockets

This recipe will show you how to use WebSockets in a client-server application (both web and command-line clients) and what its advantages are. You can find the code in the project websockets.

# Getting ready

HTTP is a simple request-response-based protocol, and then the connection is broken from the application's point of view until the next request. In a modern web application (for example, in online multiplayer games), the client and server are of equal importance; changes in the state of the application can take place on both sides. So, we need a bi-directional communication channel between the client(s) and the server that allows for two-way real time updates and more interaction; this is exactly what WebSockets has to offer. WebSocket connections between a browser and a server are made through a handshake request. This is a regular HTTP client request with an upgrade flag in the header, also containing a Sec-WebSocket-Key, which is a random value that has been base64 encoded. To complete the handshake, the server responds with a Sec-WebSocket-Accept response.

WebSockets are implemented in all modern browsers (Internet Explorer v10 and above), and can also be used in non-web applications. The communication takes place over TCP port 80, so it can pass unhindered through firewalls.