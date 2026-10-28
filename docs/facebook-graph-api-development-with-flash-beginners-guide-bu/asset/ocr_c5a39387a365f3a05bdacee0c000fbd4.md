# What just happened?

We can now see all of the connections of a Graph Object right there in its renderer. Of course, that's not very interesting unless we can see what's at the other end of each connection!

# Introducing the Requestor

Ideally, whenever the user clicks a connection from the scrolling list, a new Graph List Renderer of that connection will be created and displayed.

To do this, we'd need to add a MouseEvent.CLICK listener to the list, and use it to trigger a new URLLoader request for the clicked connection.

Fortunately, all the UI code has already been provided elsewhere in the project; we just need to tap into that. To do this, we'll need to make use of what I call a Requestor.

# Time for action – creating an HTTP Requestor

The idea is, we move all of the code regarding the URLLoader from CustomGraphContainerController to a separate class, called HTTPRequestor. We will then replace the CustomGraphContainerController constructor with this:

```
public function CustomGraphContainerController(a_graphControlContainer:GraphControlContainer)
{
    super(a_graphControlContainer);
    _requestor = new HTTPRequestor();
    _requestor.request(new GraphRequest("PacktPub"));
}
```

Why bother? Well, apart from being neater, there are two main advantages:

+   1. It's much simpler to request several Graph Objects or Graph Lists; no need to deal with multiple instances of URLLoader.
+   2. In the next chapter, we'll see how to use the official Adobe AS3 Facebook SDK to retrieve information from the Graph API. If all the code for a request is encapsulated in one class, then we only need to change one line to switch from using HTTP to using Adobe's SDK:
+   ```
+   public function CustomGraphContainerController(a_graphControlContainer:GraphControlContainer)
+   {
+       super(a_graphControlContainer);
+       _requestor = new SDKRequestor();
+       _requestor.request(new GraphRequest("PacktPub"));
+   }
+   ```