```
print("Starting testing...");
watch.start();

for (int i = 0; i < attemptedRequests; i++) {
  await callWebPage(hc, url, i, watch);
}

watch.stop();
print("${watch.elapsed.inMilliseconds}");
}
```

The callWebPage method needs to be marked as async too, as await will be used twice:

```
callWebPage(HttpClient webClient, String targetURL, int requestNumber,
    Stopwatch watch) async {
  HttpClientRequest request;
  HttpClientResponse response;
  request = await webClient.getUrl(Uri.parse(targetURL));
  response = await request.close();
  print("${requestNumber}, ${response.statusCode}, ${watch.elapsed.inMilliseconds}");
}
```

The two operations of unknown duration, the URL fetch and closing of the response, are waited upon before the output to the screen is processed.

# Summary

The server side opens up many facilities of the filesystem to save the content directly from the application, supplying content for web servers and metadata, such as log files.

Dart has the facilities to create, serve, and consume the modern web formats in the web client and on the server. We saw how quickly a specialized web server can be put together. You are now very familiar with using Dart packages outside the core SDK to implement vital application features.

The asynchronous facilities allow responsive applications to be written, and await/async helps us write clear code and bring asynchronous tasks together so that we operate them in a synchronous manner.

Our next, and largest, project will be end-to-end Dart. In this chapter, we explored how to build on the server aspects. Instead of the local filesystem, we will reach out to databases and web services to build a real-time display.