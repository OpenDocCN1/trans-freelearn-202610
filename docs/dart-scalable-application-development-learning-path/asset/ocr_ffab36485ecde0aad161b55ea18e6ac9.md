We requested the index.html file via the Postman extension again and see the following result in the console:

```
Resource /index.html
Hello, world!
```

As expected, our code prints the information that comes from both the nested zones. Now, we change the value to false and restart the server. The following request only prints the message from the first zone:

```
Resource /index.html
```

Interaction between zones can be easily organized via the zone variables.

# Tracking the zone execution

The server listener contains two zones. The first one is used to authenticate the files and the second one is used to read the content of the static files and send them back to the client. It is quite interesting to know how long each static page takes to load and process. Zones support several run methods to execute a given function in the zone. We can override the run method in ZoneSpecification to count the time spent by the request processing function. We use Stopwatch as the timer in our example. We are processing each request and print profiling the time just after sending the response back to the client, as shown in the following code:

```
//...
runServer() {
  HttpServer
    .bind(InternetAddress.ANY_IP_V4, 8080)
    .then((server) {
      Set tokens = new Set.from(['1234567890']);
      bool allowPrintContent = true;
      Stopwatch timer = new Stopwatch();
      server.listen((HttpRequest request) {
        runZoned(() {
//...
        });
        runZoned(() {
          readFile(request.uri.path).then((String context){
            Zone.current.print(context);
            request.response.write(context);
            request.response.close();
            Zone.current.print(
```