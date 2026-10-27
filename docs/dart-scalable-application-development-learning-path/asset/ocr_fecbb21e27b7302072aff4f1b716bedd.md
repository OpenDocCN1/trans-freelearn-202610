```
request.response.write(", Params:" +
    request.uri.queryParameters.toString());
}
request.response.close();
}
```

Let's take a look at how we change the AJAX polling request example to organize the long polling request at the client side. You can initiate the long polling request by calling the longPolling function as follows:

```
void main() {
  DivElement log = querySelector("#log");
  int num = 0;
  longPolling(log, num);
}
```

In this function, we send the AJAX request and wait for the response from the server. When the response is available, we call the responseHandler function and start the new request immediately:

```
longPolling(DivElement log, int num) {
  String query = "number=${num++}";
  HttpRequest.getString("$url?$query")
    .then((response) {
      responseHandler(log, response);
      longPolling(log, num);
    });
}
```

Create a Dartium launch configuration, run the server, and open the index.html file in the web browser to see the following result:

```
GET: /data, Data:12, Params:{number: 0} (51:0)
GET: /data, Data:9, Params:{number: 1} (51:2)
GET: /data, Data:60, Params:{number: 2} (51:3)
GET: /data, Data:41, Params:{number: 3} (51:5)
GET: /data, Data:15, Params:{number: 4} (51:7)
GET: /data, Data:0, Params:{number: 5} (51:8)
```

For now, we reduced the consumption of bandwidth and server utilization; hence, the clients will mostly receive only valid data.