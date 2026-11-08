Lastly, we need a runner to execute the tests of our application, and for this we will install `mocha` with the following command:

```
npm install mocha@3.2.0 --save-dev
```

Inside your project folder, you should create two new folders: one called `src` where we will place the source code of our application, and the other called `tests` where we will place all the tests of our application.

After this initial step, you should have your project folder containing a file called `package.json`, and three folders named `node_modules`, `src`, and `tests`.

![](bbox=[0.12, 0.32, 0.2, 0.38])

You can choose different names for your folders containing the source and tests, but this will be the name used in this book so, if you do change them, remember to change names when executing certain commands.

# The architecture of the server

To implement the features we described before, we will use a simple architecture, and we will need only three files on our source code.

The first one is the `index.js` file containing the initialization of our WebSocket server and receiving the messages from the chat users; the data from each user will be implemented as an observable. All data passed through this observable will call the service responsible for that message, and the attribute `service` of each message will contain the name of the service.

All services are going to be implemented in a single file, called `services.js`; each service in this file might return an observable and, in the case of an observable being returned, our client will start to listen to messages from that observable. Initially we will have only four services:

+   - `sendMessage()`: This service sends message to the connected clients; it can be a private message or a broadcast message. The attribute of the message will contain the receiver of that message.
+   - `listenToMessages()`: This service returns an observable containing all messages directed to that client.
+   - `blockUser()`: This service returns an observable without the messages from the blocked user.