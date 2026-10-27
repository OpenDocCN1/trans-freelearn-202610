# Serving a 404 error

Even nontechnical web users are familiar with the 404 error that indicates a page on a website cannot be found:

```
void _serve404(HttpRequest request) {
  request.response
    ..statusCode = HttpStatus.NOT_FOUND
    ..headers.set('Content-Type', 'text/html')
    ..write(page404)
    ..close();
}
```

This page is a regular text page (the constant string page404 contains the content), and the client is further informed of the 404 status by setting the status code to NOT_FOUND, as shown in the following screenshot:

![](bbox=[0.216, 0.392, 0.784, 0.572])

Let's hope that the blog server does not need to serve many of these pages!

# Introducing Dart's server frameworks

The blog server in this project is written directly on top of dart:io, so you can see what is going on. As you might expect, there are a number of frameworks for Dart to help write server applications, so that basic tasks, such as serving text files, do not have to be implemented from scratch for every application.

These are all available at the pub package library.