
The following screenshot shows how our screen looks after sending the data:

![](img/52c05b53a56abd53584e4fed738ed509_729_0.png)

The client sends job data

In the previous screenshot, no server is shown because there is no web server to process the request.

### How it works...

In step 1, we added a `DateTime` property. Such a type is not natively serializable to JSON; the encode method does not know how to handle this case. We have to define this ourselves and provide a `toEncodable` closure as the second optional argument of JSON.encode; this returns an appropriate serialization of DateTime. The following code is the revised toJson method in the class Job:

```
String toJson() {
  var jsm = new Map<String, Object>();
  jsm["type"] = type;
  jsm["salary"] = salary;
  jsm["company"] = company;
  jsm["posted"] = JSON.encode(posted, toEncodable: (p) {
    if(p is DateTime)
      return p.toIso8601String();
    return p;
  });
  jsm["open"] = open;
  var jss = JSON.encode(jsm);
  return jss;
}
```

2. The `acceptRequests` function describes how the web server handles incoming requests based on their method as follows:

   ```
   void acceptRequests(server) {
       server.listen((HttpRequest req) {
           switch (req.method) {
               case 'POST':
                   handlePost(req);
                   break;
               case 'GET':
                   handleGet(req);
                   break;
               case 'OPTIONS':
                   handleOptions(req);
                   break;
               default: defaultHandler(req);
           }
       },
       onError: handleError, // Listen failed.
       onDone: () => print('Web server shuts down.'));
       print('Listening for GET and POST on http://$HOST:$PORT');
   }
   ```

3. The different request handlers are shown in the following code:

   ```
   void handlePost(HttpRequest req) {
       HttpResponse res = req.response;
       addCorsHeaders(res);
       res.statusCode = HttpStatus.OK;
       req.listen(processData, onError: handleError);
   }

   processData(List<int> buffer) {
       res.write('OK, I received: ');
       res.write(new String.fromCharCodes(buffer));
       // process incoming data
       res.close();
   }

   handleGet(HttpRequest req) { // not needed here }

   void handleOptions(HttpRequest req) { // not needed here }

   void addCorsHeaders(HttpResponse res) {
       res.headers.add('Access-Control-Allow-Origin', '*');
       res.headers.add('Access-Control-Allow-Methods', 'POST, OPTIONS');
       res.headers.add('Access-Control-Allow-Headers',
       'Origin, X-Requested-With, Content-Type, Accept');
   }

   void defaultHandler(HttpRequest req) {
       res = req.response;
       res.statusCode = HttpStatus.METHOD_NOT_ALLOWED;
       res.write('Unsupported request: ${req.method}.');
       res.close();
   }

   handleError(e) {
       print(e);
       // other error handling
   }
   ```

Run the client from the previous recipe (start `web\post_form.html`) and post a job in JSON format to the server. The web server responds with an acknowledgement and returns the data back to the client. The client shows the following response:

![](img/52c05b53a56abd53584e4fed738ed509_732_0.png)

The response of the web server

### There's more...

If the server has to set the content type for the response, do this as follows before the first write to the response in `handlePost`:

```
res.headers.contentType =
new ContentType("application", "json", charset: 'utf-8');
```

Here, we make it clear that we send JSON data using the UTF-8 character set.

### Writing data to a file on the server

If we wanted to write the data received from the client to a file, we could do this as follows:

- Add the following line to `handlePost` before `req.listen`:

  ```
  BytesBuilder builder = new BytesBuilder();
  ```

- In the following code, we see `processData`:

  ```
  processData(List<int> buffer) {
    builder.add(buffer);
  }
  ```

- The builder object collects the buffered data in chunks through the add method until all the data is delivered. Then, the `onDone` callback in `acceptRequests` is called, such as `onDone: writeToFile`). In the following code, we see `writeToFile`:

  ```
  writeToFile(builder) {
    var strJson = UTF8.decode(builder.takeBytes());
    var filename = "jobs.json";
    new File(filename).writeAsString(strJson, mode:
      FileMode.APPEND).then((_) {
      res.write('Job data was appended to file');
      res.close();
    });
  }
  ```

### See also

- See the *Using CORS headers* recipe in *Chapter 5, Handling Web Applications*, for more information on CORS

## Serving files with http_server

One of the main functions of a web server that we take for granted is the serving of static files. We can write this functionality completely with the classes from dart:io, but the Dart team has written a pub package called http_server with the aim to simplify web server programming to provide web content. We will use http_server in this recipe to code a web server that serves files. You can find the code in the project serving_files.

### How to do it...

Perform the following steps to construct a web server for serving files:

1. In the first example, `serving_file.dart`, you see the code for a web server delivering a file called `Learning Dart Packt Publishing.html`:

   ```dart
   import 'dart:io';
   import 'package:http_server/http_server.dart';

   InternetAddress HOST = InternetAddress.LOOPBACK_IP_V6;
   const PORT = 8080;

   void main() {
     VirtualDirectory staticFiles = new VirtualDirectory('.');
     HttpServer.bind(HOST, PORT).then((server) {
       server.listen((req) {
         staticFiles.serveFile(new File('Learning Dart Packt Publishing.html'), req);
       });
     });
   }
   ```

   Start the server by running `bin\serving_file.dart` and open a browser with the URL `localhost:8080`. You will see the **Learning Dart** web page.

2. To serve all files from the current directory, and all its subfolders, expand the code as shown in the following code (to see `serving_curdir.dart`):

   ```dart
   import 'dart:io';
   import 'dart:async';
   import 'package:http_server/http_server.dart';

   InternetAddress HOST = InternetAddress.LOOPBACK_IP_V6;
   const PORT = 8080;
   VirtualDirectory staticFiles;

   void main() {
     staticFiles = new VirtualDirectory('.')
       ..allowDirectoryListing = true;

     runZoned(() {
       startServer();
     }, onError: handleError);
   }

   startServer() {
     HttpServer.bind(HOST, PORT).then((server) {
       server.listen(staticFiles.serveRequest);
     });
   }

   handleError(e, stackTrace) {
     print('An error occurred: $e $stackTrace');
   }
   ```

   Start the server by running `bin\serving_curdir.dart`, and open a browser with the URL `localhost:8080`. You will see a list of what's inside the bin folder as shown in the following screenshot:

   ![](img/52c05b53a56abd53584e4fed738ed509_736_0.png)

   Directory listing from a web server

### How it works...

In step 1, we first add the package `http_server` to our project (adding it to `pubspec.yaml` and importing it in the code). Then, we define the folder from which serving will take place by making an object `staticFiles` from the class `VirtualDirectory`. This class handles all the details of serving files, specifying here that it will serve from the current directory. On this object, we call the method `serveFile` with a reference to the file as an argument.

In step 2, the `serveRequest` method handles a single request for any file in the current directory (the property `allowDirectoryListing` makes the content viewable). Notice that the server request handler is enveloped in a call to the method `runZoned` from `dart:async`. This is not needed to make it work, but it illustrates a way to make our code more robust.

When using `runZoned`, the function given as its first argument is executed as if in a sandbox, and the second optional argument `onError` handles all uncaught exceptions, synchronous or asynchronous. You can find more details about the use of zones at https://www.dartlang.org/articles/zones/.

### There's more...

- If you need the path to the current executing script, use the following code:

  `var path = Platform.script.toFilePath();`

- If you need the path to the web folder, use the following code:

  `final HTTP_ROOT_PATH = Platform.script.resolve('web').toFilePath();`

- You can point a virtual directory to that path in order to start serving files from that folder using the following code:

  `final virDir = new VirtualDirectory(HTTP_ROOT_PATH);`

Together, the classes `HttpServer`, `VirtualDirectory`, and `Platform` are sufficient to implement a basic web server. When different responses are needed according to the URL, the class `Router` (see the *Using WebSockets* recipe) can simplify the code.

> Use the pub package `path` and its cross-platform methods, such as `join` and `split` if you need to manipulate the path to certain folders or files. In particular, `toUri()` and `fromUri()` are useful when converting between a URI and a path.

## Using sockets

At a somewhat lower level in the OSI model than HTTP clients and servers, we find sockets. They also enable interprocess communications across a network between clients and servers and are implemented on top of the TCP/IP. The classes that offer that functionality can be again found in `dart:io` as follows:

- `Socket`: This is used by a client to establish a connection to a server
- `ServerSocket`: This is used by a server to accept client connections
