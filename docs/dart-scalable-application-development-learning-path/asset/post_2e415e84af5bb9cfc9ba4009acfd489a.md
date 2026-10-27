
CORS headers must be returned in the header of the requested web server. To initiate a cross-origin request, we need to add new HTTP headers that allow the web browser to communicate freely with the API on another domain.

The cors project contains two servers. The first one, located in the server.dart file, is a web server listening to the secure connection on port 8443 from our previous topic, as shown in the following code:

```
import 'dart:io';
import 'dart:async';

import 'package:route/server.dart';

import 'urls.dart';
import 'files.dart';

main() {
  var pkcertDB = Platform.script.resolve('pkcert').toFilePath();
  SecureSocket
    .initialize(database: pkcertDB, password: 'changeit');

  HttpServer
    .bindSecure(InternetAddress.ANY_IP_V6, 8443,
        certificateName: 'localhost_cert')
    .then((server) {
      new Router(server)
        ..filter(allUrls, filter)
        ..serve(allUrls).listen(serveDirectory('', as: '/'))
        ..defaultStream.listen(send404);
    });
}

Future<bool> filter(HttpRequest request) {
  HttpResponse response = request.response;
  response.headers.add("Content-Security-Policy",
    "default-src 'self';" +
    "style-src 'self' 'unsafe-inline';" +
    "script-src 'self' https://apis.google.com;" +
    "frame-src https://*.google.com;" +
    "img-src https://*.gstatic.com;"
  );
  return new Future.value(true);
}
```

The second server, located in cors_server.dart, is the CORS web server listening on port 8080 and is not using HTTPS, as shown in the following code:

```
import 'dart:io';
import 'dart:async';

import 'package:route/server.dart';

import 'files.dart';

final allUrls = new RegExp('/(.*)');
final productUrl = new UrlPattern('/product');

main() {
  HttpServer
    .bind(InternetAddress.ANY_IP_V6, 8080)
    .then((server) {
      new Router(server)
        ..filter(allUrls, filter)
        ..serve(productUrl).listen(serverProduct)
        ..defaultStream.listen(send404);
    });
}

Future<bool> filter(HttpRequest request) {
  return new Future.value(true);
}

serverProduct(HttpRequest request) {
  return serveFile('products.json')(request);
}
```

The function filter in the second web server intends to set the header with CORS and allows any client to make cross-domain requests to this server. Our client will now look like the following code:

```
import 'dart:html';
import 'dart:convert';

void main() {
  onloadHandler();
}

onloadHandler() {
  var xhr = new HttpRequest();
  xhr.open('GET', 'http://localhost:8080/product', async: true);
  xhr.onLoad.listen((e) {
      Map repos = JSON.decode(xhr.response);
      var reposHTML = "";
      for (int i = 0; i < repos["repositories"].length; i++) {
          reposHTML += "<p>" +
              repos["repositories"][i]["name"] + "<br>" +
              repos["repositories"][i]["description"] + "</p>";
      }
      document.getElementById("allRepos").setInnerHtml(reposHTML);
  }).onError((e) {
      print('error making the request. ${e.toString()}');
  });
  xhr.send();
}
```

The client code makes a cross-domain request and prints the markup with the result. Let's run both the servers and open our web page in Dartium on the address https://localhost:8443/index.html. It immediately comes with the cross-domain violation exception, as shown in the following screenshot:

> Refused to connect to 'http://localhost:8080/product' because it violates the following Content Security Policy directive: "default-src 'self'". Note that 'connect-src' was not explicitly set, so 'default-src' is used as a fallback. index.html:1

Let's add connect-src in the following server code for a quick fix:

```
Future<bool> filter(HttpRequest request) {
  HttpResponse response = request.response;
  response.headers.add("Content-Security-Policy",
      "default-src 'self';" +
      "style-src 'self' 'unsafe-inline';" +
      "script-src 'self' https://apis.google.com;" +
      "frame-src https://*.google.com;" +
      "img-src https://*.gstatic.com;" +
      "connect-src http://localhost:8080/product"
  );
  return new Future.value(true);
}
```

Restart the server and refresh the web page, and you will get the following exception:

> XMLHttpRequest cannot load http://localhost:8080/product. No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'https://localhost:8443' is therefore not allowed access. index.html:1

Our request cannot pass the border of origins, so we will change the filter method in the CORS web server, as follows:

```
Future<bool> filter(HttpRequest request) {
  HttpResponse response = request.response;
  response.headers.add("Access-Control-Allow-Origin", "*");
  return new Future.value(true);
}
```

The preceding code will give the following result:

![](img/52c05b53a56abd53584e4fed738ed509_538_0.png)

This was a simple demonstration of how we can use CORS on data provided by the web server. Dartium sends an initial request to the CORS server with an Origin HTTP header that matches the origin of our web page, as shown in the following screenshot:

![](img/52c05b53a56abd53584e4fed738ed509_538_1.png)

We intend to specify Access-Control-Allow-Origin in the CORS server to allow all domains and a server-sent response with an asterisk symbol (*), as shown in the following screenshot:

![](img/52c05b53a56abd53584e4fed738ed509_538_2.png)

If you need access to other headers, you must expose them via the Access-Control-Expose-Headers header as follows:

```
Future<bool> filter(HttpRequest request) {
  HttpResponse response = request.response;
  response.headers.add("Access-Control-Allow-Origin", "*");
  response.headers.add("Access-Control-Expose-Headers",
      "session-id");
  response.headers.add("session-id", "123456");
  return new Future.value(true);
}
```

We added a sessionId span element to the web page as follows:

```
<p>This is a test page for CORS. <g:plusone></g:plusone></p>
Session ID: <span id="sessionId"></span>
<div id="allRepos"></div>
```

The following web page source code was updated as well:

```
var xhr = new HttpRequest();
xhr.open('GET', 'http://localhost:8080/product');
xhr.onLoad.listen((e) {
    var sessionId = xhr.getResponseHeader("session-id");
    document.getElementById("sessionId").text = sessionId;
```

Now, restart the CORS server and reload the web page to get the following result:

![](img/52c05b53a56abd53584e4fed738ed509_540_0.png)

CORS is supported across the following well-known web browsers:

- Chrome 3
- Firefox 3.5
- Opera 12
- Safari 4
- Internet Explorer 8

3. Click on the CREATE button to create a new API key. The server moves you to the list of your domains, as shown in the following screenshot:

![](img/52c05b53a56abd53584e4fed738ed509_542_0.png)

4. Choose your domain to see the following details:

![](img/52c05b53a56abd53584e4fed738ed509_542_1.png)

There are public and private keys that we will use in our solution. To integrate the reCAPTCHA solution in the captcha project, we used the small library recaptcha created by me and which is available on the https://pub.dartlang.org/ server. So, we can add it in the pubspec.yaml file under the dependencies packages. Open the captcha project in Dart Editor and navigate to index.html, which is shown in the following code:

```
<!DOCTYPE html>
<html>
<head>
<title>Registration</title>
<meta charset="utf-8">
<meta name="viewport"
content="width=device-width, initial-scale=1">
<link type="text/css" href="index.css">
</head>
<body>
    <H1>Registration form with CAPTCHA</H1>
    <form name="captcha_form" method="post" action="/register">
        <label for="username">Username:</label>
        <input type="text" name="username"><br>
        <label for="password">Password:</label>
        <input type="password" name="password">
        <script type="text/javascript"
            src="http://www.google.com/recaptcha/api/challenge?k=6Lc8a_kSAAAAABk-6joEQu_wurhopTGt4xCPndnX">
        </script>
        <noscript>
            <iframe src="http://www.google.com/recaptcha/api/noscript?k=6Lc8a_kSAAAAABk-6joEQu_wurhopTGt4xCPndnX"
                height="300" width="500" frameborder="0"></iframe><br>
            <textarea name="recaptcha_challenge_field" rows="3"
                cols="40"></textarea>
            <input type="hidden" name="recaptcha_response_field"
                value="manual_challenge">
        </noscript>
        <button type="submit" value="Submit">Submit</button>
    </form>
</body>
</html>
```

Copy and paste the public key of the domain registered on reCAPTCHA as the parameter for JavaScript and the parameter for the source of the iframe tag. You need to change these values for your public key. Now, let's open the server.dart file and move to line 10 where we created an instance of the ReCaptcha class. Again, copy and paste a pair of private and public keys of your domain here so that the class instance can pass them via the free service reCAPTCHA on Google in order to ensure that the sender is correct and has a registered domain, as shown in the following code:

```
...
    final ReCaptcha reCaptcha = new ReCaptcha(
        '6Lc8a_kSAAAAABk-6joEQu_wurhopTGt4xCPndnX', // public key
        '6Lc8a_kSAAAAAB4921bel1ToEM2e3SDmPG4ZvXVNL'); // private key
```
