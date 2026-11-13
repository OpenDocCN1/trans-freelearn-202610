# # Retrieving a text response

With our asynchronous HTTP client supporting text responses, we are able to make use of it to obtain a dynamic text so, let's create an activity that displays the text available on a remote URL resource.

First, we have to build our HTTPRequest using the HTTPRequest.Builder class:

```
protected void onCreate(Bundle savedInstanceState) {
    ...
    HTTPRequest.Builder builder = new HTTPRequest.Builder();
    // Set the HTTP Verb to GET
    builder.setVerb(HTTPRequest.Verb.GET);
    // Sets location of the remote resource
    builder.setUrl("http://<hostname>:<port>/greetings");
    // Build the request object
    HTTPRequest request = builder.build();
    ...
}
```

We wish to draw your attention to the fact that you should replace <hostname> and <port> to make it work for your HTTP server.

As defined before, in order to execute the request over our asynchronous client, AsyncHTTPClient, we must provide a ResponseHandler object to the execute method. Moreover, we want to define an object that updates the UI with the text obtained.

First of all, we will extend our ResponseHandler abstract and create a class to process the body and forward the response to a callback that receives the text message as the input:

```
public abstract class TextResponseHandler
extends ResponseHandler {
    // Response Callback receiving the string body
    abstract void onSuccess(String response);

    @Override
    public void onSuccess(HTTPResponse response) {
        TextPlainBody body = (TextPlainBody)response.mBody;
        // Invoke the callback that receives a string
        onSuccess(body.getContent());
    }
}
```