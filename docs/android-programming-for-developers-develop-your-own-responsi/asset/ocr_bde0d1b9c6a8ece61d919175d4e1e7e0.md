# Performing HTTP requests asynchronously

So far we have been using the HttpURLConnection client and AsyncTask to retrieve remote data asynchronously in our code examples.

While this solution can work in most cases, we could end up with loads of duplicate code in our applications.

In this section, we will create a neat high-level asynchronous HTTP client to perform remote requests outside of the main thread that forwards the result of the request to the application's main thread using a callback handler. This approach fits well with the application UI model because the callback handler, which executes on the main thread, is able to update the UI with the data retrieved from the server.

First of all, let's define the basic interface that our asynchronous client should honor to execute remote requests in the background:

```
```java
public interface AsyncHTTPClient {
    void execute(HTTPRequest request, ResponseHandler handler);
}
```
```

The HTTPRequest class is a Java model used to define all the parameters required to build the HTTP request. We will omit some of the implementation details, but with the help of the Builder class, we will be able to define the HTTP request verb, the request URL, the HTTP headers, the HTTP query parameters, and the HTTP body:

```
```java
public class HTTPRequest {
    final Verb mVerb;
    final String mUrl;
    final List<Header> mHeaders;
    final Map<String, String> mParameters;
    final Body mBody;

    private HTTPRequest(Builder builder) {...}
}
```
```