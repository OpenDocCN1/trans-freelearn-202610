# Working with Web Servers

This produces the following server console output:

![](bbox=[0.177, 0.176, 0.824, 0.372])

Console output from the web server

Then, start any browser with the URL `http://localhost:8080` to see the response text appear on the client as shown in the following screenshot:

![](bbox=[0.177, 0.449, 0.824, 0.56])

The browser client shows the response

# There is more...

The `HttpRequest` object also has properties that provide information about the client's request; the most important ones are as follows:

+   - `method`: This is derived from the way the web form was submitted. In the `<form action="http://localhost:4041" method="GET">` code, the values it can take are **GET, POST, PUT,** or **DELETE**.
+   - `headers`: This gives general information on the request, such as content type, content length, and date.
+   - `uri`: This gives the location where the request originated from.