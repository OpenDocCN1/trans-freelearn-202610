GraphRequest is a simple class; its constructor allows you to use two parameters to specify what you'd like to retrieve from the Graph API:

+   - objectID, the name of any Graph Object.
+   - connectionID, the name of any connection of that Graph Object.

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
        private var __requests:Dictionary = new Dictionary();

        public function HTTPRequestor(target:IEventDispatcher = null)
        {
```