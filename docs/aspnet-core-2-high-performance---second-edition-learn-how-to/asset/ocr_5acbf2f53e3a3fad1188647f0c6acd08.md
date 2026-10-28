In Orleans, actors are known as grains, and you can write them in C# by inheriting from an interface. These are then executed by an Orleans server called a silo. Grains can be persisted to storage, such as SQL or Azure Tables, to save their state and to reactivate later. Orleans can also make use of the Bond serializer for greater efficiency.

Unfortunately, Orleans does not currently support .NET Core. Support is planned for version 2.0 of Orleans, which is currently a work in progress. Orleans allows you to write simple and scalable systems with low latency, and you can read more at http://dotnet.github.io/orleans/.

# Custom transports

In Chapter 6, Addressing Network Performance, we started with an introduction to TCP/IP and briefly mentioned User Datagram Protocol (UDP). We also covered Transport Layer Security (TLS) encryption and how to minimize the impact of secure connections while still reaping performance benefits.

UDP is simpler and quicker than Transmission Control Protocol (TCP), but you either need to not care about reliable delivery (multiplayer games and voice/video chat) or build your own layer to provide this. In Chapter 10, The Downsides of Performance-Enhancing Tools, we highlighted StatsD, which uses UDP to avoid blocking latency while logging to a remote central server.

There are alternatives to TLS if you aren’t constrained to a browser, but if you’re developing a web application, this will probably only apply inside your server infrastructure. For example, the WhatsApp messaging app uses Noise Pipes and Curve25519 from the Noise Protocol Framework (http://noiseprotocol.org/) between the smartphone app and its servers, in addition to the Signal Protocol for end-to-end encryption.

Using Noise Pipes instead of TLS increases performance because fewer round trips are required to set up a connection. Another option with similar benefits is the secure pipe daemon (spiped), as used by and created for the secure Linux/BSD backup software, Tarsnap. However, you do need to pre-share the keys, but you can read more about this at http://www.tarsnap.com/spiped.html.