The following is the output shown on the web page:

![](bbox=[0.31, 0.173, 0.69, 0.4])

Isolates in a web app

We see the messages coming in from the three isolates. At the count of 10, the timer stops, and the isolates receive a message to shut down.

# How it works...

The main isolate first sets up a ReceivePort and a listener to the isolates. This listener first captures the isolate's sendPort and then appends each message on the web page. An isolate in a web app must be started with spawnUri; the code sits in the script worker.dart. Then the root isolate starts a timer and sends each second the number of seconds passed. After 10 seconds, the timer is canceled, and an END message is sent.

We start three isolates, each executing the same code. The main method in the isolates code receives List<String>args as its first argument. This comes from the second parameter of spawnUri; this is a way for the main isolate to send initial data to the isolate, but here it was the empty list. Each isolate first sets up its ReceivePort, and sends its sendPort to the main isolate, so communication lines are established. Then, it starts listening and echoes back what it receives. The isolate exits when the END message is received. When compiling to JavaScript with dart2js, .js is automatically added to the end of the script name worker.dart.

# See also

+   Refer to the Using isolates in the Dart VM recipe for a general explanation on isolates.