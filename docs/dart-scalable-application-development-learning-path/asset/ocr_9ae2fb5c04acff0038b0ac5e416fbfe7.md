# 5

# A Blog Server

> Forget about someone's resume or how they present themselves at a party. Can they blog or not? The blog doesn't lie.
> 
> – Nick Denton

I wish I could remember which was the first blog that I read on the Web, but I do remember reading (and having) diary sections on a home page, long before the term "blog" was invented! Despite the meteoric rise of various social media platforms, blogging has maintained its place.

It is time for us to explore the server side of the Dart story, and a blog gives us great opportunity to explore different aspects of this language. On the server side, new capabilities are made available, as the context is outside the web browser.

# The Hello World server example

It is very simple to create a minimal Hello World server in Dart. Open the sample HelloWorld file of this chapter in the Dart editor. This project is a command-line project, so the main.dart file is placed in the bin folder:

```
import 'dart:io';
void main() {
  HttpServer.bind('127.0.0.1', 8080).then(
    (server) {
      server.listen((HttpRequest request) {
        request.response
          ..write('Hello Dart World')
          ..close();
      });
    });
}
```