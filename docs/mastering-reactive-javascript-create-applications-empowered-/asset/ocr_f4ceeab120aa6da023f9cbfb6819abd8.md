# Connecting to the server application

We are going to use WebSockets to connect with the server through the socket.io-client. On the server we modeled, connections and requests as observable objects and we will do the same on the client, To do so, open the connection.js file inside the client folder, and import the modules that we are going to use:

```
var Rx = require('rx');
var io = require('socket.io-client');
```

So far we have only imported the rx module, which we will use to create the observables, and the socket.io-client, which we will use to start the connection.

![](bbox=[0.1, 0.41, 0.18, 0.5])

We will not use ES6 on the client, to avoid the extra step of transpiling it to ES5 and making it work on all major browsers.

Now we need to connect to the server. As we are running on port 3000, we can do this with this code:

```
var socket = io('http://localhost:3000');
```

We only can start to send messages after the connection is established; for this reason, we need observables to listen to connect and disconnect events:

```
var connectionObservable = Rx.Observable.fromEvent(socket,'connect');
var disconnectObservable = Rx.Observable.fromEvent(socket,'disconnect');
```

In this file we need a way to send data to the server and to listen to data from the server; for this reason we are going to export two functions.

The first exposed function is to send data to the server, and we are going to call it addSender; it has the following signature:

```
addSender(observable);
```