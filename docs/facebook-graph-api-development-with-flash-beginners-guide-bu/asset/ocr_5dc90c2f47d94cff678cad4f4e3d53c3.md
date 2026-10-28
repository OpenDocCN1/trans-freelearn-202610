```
var variables:URLVariables = new URLVariables();

urlRequest.url = "https://graph.facebook.com/oauth/authorize";
variables.client_id = "«your_application_id»";
variables.type = "user_agent";
variables.redirect_uri = "http://google.com";
urlRequest.data = variables;

loader.addEventListener(Event.COMPLETE, onAuthenticationComplete);
loader.addEventListener(IOErrorEvent.IO_ERROR, onIOError);
loader.load(urlRequest);
}
```

Make sure that you replace «your_application_id» with your actual application ID.

We're using the same IO_ERROR handler function as when we load a Graph object, but we need a new function, onAuthenticationComplete(), for when the loading has finished; add it to HTTPRequestor.as:

```
private function onAuthenticationComplete(a_event:Event):void
{
    trace("Authentication complete");
    var loader:URLLoader = URLLoader(a_event.target);
    trace(loader.data);
}
```

Nothing complicated there; it will trace whatever gets sent back.

In CustomGraphContainerController.as, let's change the constructor function so that it kicks this authentication off instead of loading a user profile:

```
public function CustomGraphContainerController(a_graphControlContainer:GraphControlContainer)
{
    super(a_graphControlContainer);
    _requestor = new HTTPRequestor();
    addEventListenersToRequestor();
    _requestor.attemptToAuthenticate();
    //_requestor.request(new GraphRequest("«your_user_id»"));
}
```

Before this can work, we need to add the function to the IRequestor interface. Modify \src\graph\apis\base\IRequestor.as to:

```
package graph.apis.base
{
    import flash.events.IEventDispatcher;
```