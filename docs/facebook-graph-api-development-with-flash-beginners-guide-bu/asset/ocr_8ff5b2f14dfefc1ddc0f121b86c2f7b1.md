2. In HTTPRequestor.as, set the properties of the URLVariables object depending on these new strings:

```
public function request(a_request:GraphRequest):void
{
    var loader:URLLoader = new URLLoader();
    var urlRequest:URLRequest = new URLRequest();
    var variables:URLVariables = new URLVariables();

    //We construct a URL from the parameters of the GraphRequest
    urlRequest.url = "https://graph.facebook.com/" +
        a_request.objectID;
    if (a_request.connectionID)
    {
        //remember, this means a connection (and thus a list)
        //was requested
        urlRequest.url += "/" + a_request.connectionID;
    }

    variables.metadata = 1;
    variables.limit = a_request.limit;
    variables.offset = a_request.offset;
    variables.since = a_request.since;
    variables.until = a_request.until;
    if (accessToken != "")
    {
        variables.access_token = accessToken;
    }
    urlRequest.data = variables;

    //this is used to figure out which GraphRequest created the
    //loader later
    _requests[loader] = a_request;

    loader.addEventListener(Event.COMPLETE,
        onGraphDataLoadComplete);
    loader.addEventListener(IOErrorEvent.IO_ERROR, onIOError);
    loader.load(urlRequest);
}
```

3. Modify the third parameter passed to the Facebook.api() method in the request() function inside SDKRequestor.as:

```
public function request(a_request:GraphRequest):void
{
```