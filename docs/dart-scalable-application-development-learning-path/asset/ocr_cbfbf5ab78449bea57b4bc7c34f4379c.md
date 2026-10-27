```
// Next function was changed to return data only:
String fetchData() {
    return dataGenerator.nextInt(100).toString();
}
```

The new longLasting function is shown in the following code. It runs two periodic timers. The first one emulates the process based on the request timeout. If no data was collected every 30 seconds, it resets the second timer and response with an empty string. The second one is managing the data availability. If the data is available in a second's interval, it resets the first timer and responds with the collected data:

```
longLasting(HttpRequest request) {
    Timer reqTimer, dataReadyTimer;

    reqTimer = new Timer.periodic(
        new Duration(seconds:30), (Timer t) {
        dataReadyTimer.cancel();
        processParams(request);
    });

    dataReadyTimer = new Timer.periodic(
        new Duration(seconds:1), (Timer t) {
        if (ready.nextBool()) {
            reqTimer.cancel();
            processQueryParams(request, ready:true);
        }
    });
}
```

The updated version of the processQueryParams function is illustrated in the following code. This function returns data from fetchData or returns an empty string depending on the value of the ready attribute. The code is as follows:

```
processQueryParams(HttpRequest request, {bool ready:false}) {
    request.response.write(
        "${request.method}: ${request.uri.path}");
    if (ready) {
        request.response.write(", Data:" + fetchData());
    } else {
        request.response.write(", Data:");
    }
    if (request.uri.queryParameters.length > 0) {
```