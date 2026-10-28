```
function request(a_request:GraphRequest):void;
function attemptToAuthenticate(...permissions):void;
function initialize():void;
function search(a_query:String = "", a_type:String = ""):void;
}
```

This means we don't have to cast _requestor as an HTTPRequestor in CustomGraphContainerController.search() any more, so open CustomGraphContainerController.as and alter that method:

```
override public function search(a_query:String = "", a_type:String = "", a_userID:String = ""):void
{
    _requestor.search(a_query, a_type);
}
```

All that's left before we can test it is to change the constructor so that _requestor is created as an SDKRequestor rather than an HTTPRequestor. This is done as follows:

```
public function CustomGraphContainerController(a_graphControlContainer:GraphControlContainer)
{
    super(a_graphControlContainer);
    this._showListCounts = true;
    this._showListFilters = true;
    this._canShowSearchUI = true;

    _requestor = new SDKRequestor();
    addEventListenersToRequestor();
    //we must wait for the Requestor to initialise before we can do anything else with it
    _requestor.addEventListener(Event.COMPLETE, onRequestorInitialize);

    _requestor.initialize();
}
```