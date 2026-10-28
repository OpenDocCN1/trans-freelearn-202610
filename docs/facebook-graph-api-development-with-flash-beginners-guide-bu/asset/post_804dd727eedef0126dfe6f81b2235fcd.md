
As you can see, there's a new **Search** button in the top-left corner, which makes the new **Search** window appear in the main stage area. You'll recognise the first six radio buttons as corresponding to allowed values for the `type` parameter; don't worry about **My News Feed** or **Friend's Posts** for now.

When the **Search** button inside the window is clicked, it calls a function called `search()` inside the class that `CustomGraphContainerController.as` extends, passing it three values:

- Query
- Type
- User ID (which contains the value in the lower textbox – again, we'll come to this later)

This function is completely empty, so clicking that button doesn't do anything at the moment; we'll have to override `search()` and make it work!

1. In `CustomGraphContainerController.as`, add the following function:

```
override public function search(a_query:String = "", a_type:String = "", a_userID:String = ""):void
{

}
```

2. Now to add the actual search code. Let’s start with the HTTP Requestor. The process is very similar to requesting a Graph Object or Graph List directly – in fact, it's even simpler. Open `HTTPRequestor.as` and create a new function as so:

```
public function search(a_query:String = "", a_type:String = ""):void
{
    var loader:URLLoader = new URLLoader();
    var urlRequest:URLRequest = new URLRequest();
    var variables:URLVariables = new URLVariables();

    urlRequest.url = "https://graph.facebook.com/search";

    variables.metadata = 1;
    if (accessToken != "")
    {
        variables.access_token = accessToken;
    }
    variables.q = a_query;    //not variables.query!
    variables.type = a_type;
```

### What just happened?

Our Visualizer can now search for Graph objects based on a query that you enter. Since the results are returned in the form of a Graph List, our application will then grab all the info about those objects – and this means we can see their connections, too.

### Time for action – searching via the SDK

It's easy to make our search work via the SDK, too. Start by opening SDKRequestor.as and adding a new function called search(), just as we did with the HTTPRequestor:

```
public function search(a_query:String = "", a_type:String = ""):void
{
}
```

As with a request, we pass three parameters to the Facebook.api() method:

1. /search (a string containing the key part of the Graph URL)
2. A reference to a callback function
3. An object containing the parameters (q and type – remember not to use query as the name of the parameter!)

So, add this call to your search() function as shown in the following code:

```
public function search(a_query:String = "", a_type:String = ""):void
{
    Facebook.api(
        "/search",
        searchComplete,
        { metadata: 1, q:a_query, type:a_type }
    );
}
```

There was no need to use currying this time, as we don't need to maintain a reference to the original GraphRequest object.

That's it for the search() function. We still need to write the callback for when the data is loaded though, so create a new function, searchComplete(), as shown in the following lines of code:

```
private function searchComplete(success:Object, fail:Object):void
{
}
```

Like the other SDK callbacks, this function must accept two parameters: result and fail.

Since the search results are returned in the form of a Graph List, we can re-use most of the code from the requestComplete() function. Modify searchComplete() as shown in the following lines of code:

```
private function searchComplete(result:Object, fail:Object):void
{
    if (result != null)
    {
        var decodedJSON:Object = result;
        var graphList:GraphList = new GraphList();
        var childGraphObject:GraphObject;
        for each (var childObject:Object in decodedJSON)
        {
            childGraphObject = new GraphObject();
            for (var childKey:String in childObject)
            {
                childGraphObject[childKey] = childObject[childKey];
            }
            graphList.addToList(childGraphObject);
        }
        graphList.paging = decodedJSON.paging;

        dispatchEvent(new RequestEvent(RequestEvent.REQUEST_COMPLETED, graphList));
    }
    else
    {
        //was a failure. See contents of 'fail' object.
    }
}
```

Just as with the HTTP Requestor, check Chapter 2 and Chapter 3 if you want a reminder of how this all works.

Now that both Requestors have a search() method; we can add that method to the IRequestor interface. Modify IRequestor as like so:

```
package graph.apis.base
{
    import flash.events.IEventDispatcher;
    import graph.GraphRequest;

    public interface IRequestor extends IEventDispatcher
    {
        function request(a_request:GraphRequest):void;
        function attemptToAuthenticate(...permissions):void;
        function initialize():void;
        function search(a_query:String = "", a_type:String = ""):void;
    }
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

### Search Me

Save, compile, and upload the SWF. Load it in your browser – remember to use sdkindex.html.

![](img/4e826fa36038b86f2087ef8b818d0acf_184_0.png)

Great, it works!

### What just happened?

We just added the same search functionality as before, but this time we made it work via the SDK. Bear in mind that this means that the access token is automatically used, if the user is authenticated.

### Have a go hero – setting the locale

Now that we can search for posts, how about restricting the search to posts written by people with a specified locale?

You could hardcode the locale to use within the search() method, or (if you're comfortable with manipulating XML) you could make your project pick one of the locales at random, from http://www.facebook.com/translations/FacebookLocales.xml.

## Searching feeds and wall posts

The Graph URL search may be limited compared to Facebook's Full Search, but there are two types of search that can only be done via the Graph API:

- Searching for specific items in your news feed
- Searching for things that a specific friend has posted

### Time for action – searching your news feed

Your news feed is the list you see when you log in to the Facebook website; all the status updates, links, videos, and photos that your friends (and other Graph websites to which you are subscribed, such as pages you "like") have posted.

We've seen that we can access this feed through your profile's home connection:

```
https://graph.facebook.com/me/home?access_token=«access_token»
```

which returns a Graph List, as you'd expect.

We can search within this feed by specifying a value for the q parameter:

```
https://graph.facebook.com/me/home?q=test&access_token=«access_token»
```

```
"data": [
  {
    "id": "151673724649_415202504649",
    "from": {
      "name": "Activetuts",
      "category": "Products_other",
      "id": "151673724649"
    },
    "message": "Test Your Observation Skills With an AS3 Difference Game \u2013 Active Premium",
    "link": "http://feedproxy.google.com/~r/Flashtuts/~3/i8-bZF-Mvhc/",
    "description": "Today, we have another Active Premium tutorial exclusively available to Premium members. If you want to take your ActionScript skills to the next level, then we have an awesome tutorial for you, courtesy of Stephan Meesters",
    "type": "link",
    "created_time": "2010-07-28T11:07:12+0000",
    "updated_time": "2010-07-28T11:07:12+0000"
  }
],
"paging": {
```
