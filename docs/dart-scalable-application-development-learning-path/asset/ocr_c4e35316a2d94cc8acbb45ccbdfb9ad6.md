```
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

Run the client from the previous recipe (start web\post_form.html) and post a job in JSON format to the server. The web server responds with an acknowledgement and returns the data back to the client. The client shows the following response:

![](bbox=[0.17, 0.46, 0.82, 0.7])

The response of the web server