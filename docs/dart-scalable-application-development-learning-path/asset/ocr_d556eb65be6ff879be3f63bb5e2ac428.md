3. To handle the response, use the following code:

```
requestComplete(HttpRequest request) {
  if (request.status == 200 && request.readyState == HttpRequest.DONE) {
    Blob blob = request.response;
    flr.onLoadEnd.listen( (e) {
      img.src = flr.result;
    });
    flr.readAsDataUrl(blob);
  }
  else {
    print('Request failed, status=${request.status}');
  }
}
```

Try it out with the 5 MB file stadium.jpg.

# How it works...

To read the blob, we will need a FileReader object; we constructed this in step 1. In step 2, we build the request object, which is fairly general, except that we set responseType and mimeType. In the callback function in step 3, we test with request.readyState == HttpRequest.DONE to be sure that the request has been fully handled. Then, we performed the following steps:

+   - We created a Blob object from the Blob class in dart:html and set it to the response
+   - We read the blob with the method readAsDataUrl
+   - When this was completed (signaled by the onLoadEnd event), the source of the image tag was set to the result of the FileReader object

# Transforming streams

Listening to a stream captures the sequence of results coming from an event-like action, such as clicking on a button in a web page or opening a file with the openRead method. These results are data that can be processed, but the errors that occur are also part of the stream. Dart can work with streams in a very functional way, such as filtering the results with where or mapping the results to a new stream (for a complete list of these methods, refer to https://api.dartlang.org/apidocs/channels/stable/dartdoc-viewer/dart:async.Stream). To modify the incoming results, we can also use a transformer; this recipe shows you how to do this (refer to the project transforming_stream).