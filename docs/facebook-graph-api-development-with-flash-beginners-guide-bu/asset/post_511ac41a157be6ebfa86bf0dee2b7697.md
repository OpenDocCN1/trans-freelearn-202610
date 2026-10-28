
GraphRequest is a simple class; its constructor allows you to use two parameters to specify what you'd like to retrieve from the Graph API:

- objectID, the name of any Graph Object.
- connectionID, the name of any connection of that Graph Object.

So, to request the Packt Publishing Page, you would use this GraphRequest:

```
newGraphRequest("PacktPub");
```

and to request the list of Posts from the Packt Publishing Page, you'd use this:

```
newGraphRequest("PacktPub", "posts");
```

The class is already written; it's in \src\com\graph\apis\http\HTTPRequestor.as. Take a look! There are a few changes from the code we wrote in CustomGraphContainerController.as, but these all have comments to explain them:

```
package graph.apis.http
{
    import events.DialogEvent;
    import events.RequestEvent;
    import flash.events.Event;
    import flash.events.EventDispatcher;
    import flash.events.HTTPStatusEvent;
    import flash.events.IEventDispatcher;
    import flash.events.IOErrorEvent;
    import flash.net.URLLoader;
    import flash.net.URLRequest;
    import flash.net.URLVariables;
    import flash.utils.Dictionary;
    import graph.apis.base.IRequestor;
    import graph.BaseGraphItem;
    import graph.GraphList;
    import graph.GraphObject;
    import graph.GraphRequest;
    import com.adobe.serialization.json.JSON;

    //the class needs to dispatch events (see later in code for why)
    public class HTTPRequestor extends EventDispatcher implements IRequestor
    {
        //this is used to figure out which GraphRequest created each
        //loader
        private var _requests:Dictionary = new Dictionary();

        public function HTTPRequestor(target:IEventDispatcher = null)
        {
            //this is needed because the class extends EventDispatcher
            super(target);
        }

        public function request(a_request:GraphRequest):void
        {
            var loader:URLLoader = new URLLoader();
            var urlRequest:URLRequest = new URLRequest();
            var variables:URLVariables = new URLVariables();

            //We construct a URL from the parameters of the GraphRequest
            urlRequest.url = "https://graph.facebook.com/" + a_request.objectID;
            if (a_request.connectionID)
            {
                urlRequest.url += "/" + a_request.connectionID;
            }

            variables.metadata = 1;
            urlRequest.data = variables;

            //this is used to figure out which GraphRequest created the loader later
            _requests[loader] = a_request;

            loader.addEventListener(Event.COMPLETE, onGraphDataLoadComplete);
            loader.load(urlRequest);
        }

        private function onGraphDataLoadComplete(a_event:Event):void
        {
            var loader:URLLoader = a_event.target as URLLoader;
            var graphData:String = loader.data;
            var decodedJSON:Object = JSON.decode(graphData);

            //we find the original GraphRequest used to start the loader
            var originalRequest:GraphRequest = _requests[loader] as GraphRequest;

            if (decodedJSON.data)
            {
                var graphList:GraphList = new GraphList();
                var childGraphObject:GraphObject;
                for each (var childObject:Object in decodedJSON.data)
                {
                    childGraphObject = new GraphObject();
                    for (var childKey:String in childObject)
                    {
                        childGraphObject[childKey] = childObject[childKey];
                    }
                    graphList.addToList(childGraphObject);
                }
                graphList.paging = decodedJSON.paging;

                //we use the properties of the original GraphRequest to add
                //some extra data to the GraphList itself
                graphList.ownerID = originalRequest.objectID;
                graphList.connectionType = originalRequest.connectionID;

                //since this class does not have a renderGraphList() method,
                //we dispatch an event, which CustomGraphContainerController
                //will listen for, and call its own renderGraphList() method
                dispatchEvent(new RequestEvent(RequestEvent.REQUEST_COMPLETED,
                    graphList));
            }
            else
            {
                var graphObject:GraphObject = new GraphObject();
                for (var key:String in decodedJSON)
                {
                    graphObject[key] = decodedJSON[key];
                }

                //since this class does not have a renderGraphList() method,
                //we dispatch an event, which CustomGraphContainerController
                //will listen for, and call its own renderGraphList() method
                dispatchEvent(new RequestEvent(RequestEvent.REQUEST_COMPLETED,
                    graphObject));
            }
        }
    }
}
```

Compile and run your SWF, then expand the **Connections** box and click on "posts":

![](img/4e826fa36038b86f2087ef8b818d0acf_67_0.png)

Great! The Graph List Renderer appears, with a black line to the Page to indicate that there is a connection between them. What about the other connections? Try clicking on **statuses**.

**Error #2044: Unhandled ioError.text=Error #2032: Stream Error. URL:**
**https://graph.facebook.com/204603129458/statuses?metadata=1**

Oops.

## What just happened?

If you load the troublesome URL in your browser (https://graph.facebook.com/packtpub/statuses), you'll see the following message:

```
{
  "error": {
    "type": "OAuthAccessTokenException",
    "message": "An access token is required to request this resource."
  }
}
```

This error is due to not being logged in to Facebook through your SWF. We'll look at how to solve this in the next chapter.

For now, you can get around the error by adding an IO_ERROR event listener to the URLLoader. In HTTPRequestor.as, modify request():

```
public function request(a_request:GraphRequest):void
{
    var loader:URLLoader = new URLLoader();
    var urlRequest:URLRequest = new URLRequest();
    var variables:URLVariables = new URLVariables();

    //We construct a URL from the parameters of the
    //GraphRequest
    urlRequest.url = "https://graph.facebook.com/" +
        a_request.objectID;
    if (a_request.connectionID)
    {
        urlRequest.url += "/" + a_request.connectionID;
    }

    variables.metadata = 1;
    urlRequest.data = variables;

    //this is used to figure out which GraphRequest
    //created the loader later
    _requests[loader] = a_request;

    loader.addEventListener(Event.COMPLETE,
        onGraphDataLoadComplete);
    loader.addEventListener(IOErrorEvent.IO_ERROR,
        onIOError);
    loader.load(urlRequest);
}
```

![](img/4e826fa36038b86f2087ef8b818d0acf_68_0.png)

You will need to import flash.events.IOErrorEvent. Now, in the same class, create a simple event handler function to trace the error:

```
private function onIOError(a_event:IOErrorEvent):void
{
    trace(a_event.text);
}
```

![](img/4e826fa36038b86f2087ef8b818d0acf_68_1.png)

This way, you can see the error in your output window, but it won't crash the SWF. Note: a try-catch block will not work for this kind of error.

```
"updated_time": "2010-03-18T14:46:50+0000"
},
{
  "id": "307932939458",
  "from": {
    "name": "Packt Publishing",
    "category": "Products_other",
    "id": "204603129458"
  },
  "name": "Books",
  "description": "Packt Books",
  "link": "http://www.facebook.com/
    album.php?aid=180619&id=204603129458",
  "count": 32,
  "type": "normal",
  "created_time": "2010-02-04T12:32:17+0000",
  "updated_time": "2010-03-18T16:08:42+0000"
}
],
"paging": {
  "previous": "https://graph.facebook.com/204603129458/
    albums?metadata=1&limit=25&since=2010-09-30T10%3A13%3A53%2B0000",
  "next": "https://graph.facebook.com/204603129458/
    albums?metadata=1&limit=25&until=2010-02-04T12%3A32%3A16%2B0000"
}
}
```

## Time for action – loading photos from an album

However, as we've established, each object inside data is a Graph Object in its own right. Let's take a look at the Packt Books album, with id 307932939458, by browsing to https://graph.facebook.com/307932939458?metadata=1:

```
{
  "id": "307932939458",
  "from": {
    "name": "Packt Publishing",
    "category": "Products_other",
    "id": "204603129458"
  },
  "name": "Books",
  "description": "Packt Books",
  "link": "http://www.facebook.com/
    album.php?aid=180619&id=204603129458",
  "count": 32,
  "type": "album",
  "created_time": "2010-02-04T12:32:17+0000",
  "updated_time": "2010-03-18T16:08:42+0000",
  "metadata": {
```
