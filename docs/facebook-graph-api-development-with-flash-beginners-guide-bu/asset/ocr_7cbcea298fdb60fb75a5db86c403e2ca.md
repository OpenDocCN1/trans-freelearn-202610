```
{
    variables.access_token = this.accessToken;
}
urlRequest.data = variables;

loader.addEventListener(Event.COMPLETE, onDeleteComplete);
loader.addEventListener(IOErrorEvent.IO_ERROR, onIOError);
loader.addEventListener(HTTPStatusEvent.HTTP_STATUS,
    onHTTPStatusReturned);
loader.addEventListener(SecurityErrorEvent.SECURITY_ERROR,
    onSecurityError);
loader.load(urlRequest);
}
```

5. What about HTTP request methods? There's one called DELETE, which is definitely not a "safe" method, and does exactly what we want. Unfortunately, due to security concerns, we can only use it in AIR. Here's the code, in case you write an AIR application later:

```
public function deleteObject(a_objectID:String):void
{
    var loader:URLLoader = new URLLoader();
    var urlRequest:URLRequest = new URLRequest();
    var variables:URLVariables = new URLVariables();

    urlRequest.url = "https://graph.facebook.com/";
    urlRequest.url += a_objectID;
    urlRequest.method = URLRequestMethod.DELETE;

    if (this.accessToken != "")
    {
        variables.access_token = this.accessToken;
    }
    urlRequest.data = variables;

    loader.addEventListener(Event.COMPLETE, onDeleteComplete);
    loader.addEventListener(IOErrorEvent.IO_ERROR, onIOError);
    loader.addEventListener(HTTPStatusEvent.HTTP_STATUS,
        onHTTPStatusReturned);
    loader.addEventListener(SecurityErrorEvent.SECURITY_ERROR,
        onSecurityError);
    loader.load(urlRequest);
}
```