
The required form of the `requestComplete()` callback is almost the same as the others, as shown in the next lines of code:

```
private function requestComplete(result:Object, fail:Object):void
{
    if (result != null)
    {
        //was a success.
    }
    else
    {
        //was a failure. See contents of 'fail' object.
    }
}
```

Add that to `SDKRequestor.as`. Notice that this time, instead of a `success` parameter we have a `result` parameter (not that this really matters, but it's a more accurate name), and it is just an `Object`, rather than an instance of `FacebookSession`.

In fact, `result` is in exactly the same form as the `decodedJSON` object that we created ourselves in `HTTPRequestor.onGraphDataLoadComplete()`. This means we can copy and paste the code we wrote earlier to deal with it – so do so:

```
private function requestComplete(result:Object, fail:Object):void
{
    if (result != null)
    {
        var decodedJSON:Object = result;
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
            graphList.ownerID = originalRequest.objectID;
            graphList.connectionType = originalRequest.connectionID;

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

            dispatchEvent(new RequestEvent(RequestEvent.REQUEST_COMPLETED,
                graphObject));
        }
    }
    else
    {
        //was a failure. See contents of 'fail' object.
    }
}
```

You'll also need the following imports:

```
import graph.GraphList;
import graph.GraphObject;
import events.RequestEvent;
```

There's a big problem here, though. We refer to originalRequest in the code, but we have no way of obtaining the original GraphRequest instance!

The solution is to use a coding trick called currying. This allows us to pass extra values to the callback function at the time that we make the request.

> Don't worry if you don't understand how currying works. I'm not going to explain it here, as it's fairly advanced and not really relevant to Facebook – the code that uses it is specific to the Visualizer. For an excellent explanation of currying, see the article by Jackson Dunstan at: http://jacksondunstan.com/articles/338.

![](img/4e826fa36038b86f2087ef8b818d0acf_126_0.png)

Change the request() function to:

```
public function request(a_request:GraphRequest):void
{
    var graphItem:String = "/" + a_request.objectID;
    if (a_request.connectionID)
    {
        graphItem += "/" + a_request.connectionID;
    }
    Facebook.api(graphItem, function(result:Object, fail:Object):void {
        requestComplete.call(this, result, fail, a_request); } );
}
```

Now, modify the signature of requestComplete() to accept an extra parameter:

```
private function requestComplete(result:Object, fail:Object, originalRequest:GraphRequest):void
```

SDKRequestor is now ready for testing. Switch to CustomGraphContainerController.as, and change it to:

```
package controllers
{
    import flash.events.Event;
    import graph.apis.base.ExtendedPermissions;
    import graph.apis.http.HTTPRequestor;
    import graph.apis.sdk.SDKRequestor;
    import graph.GraphRequest;
    import ui.GraphControlContainer;

    public class CustomGraphContainerController extends GCCcontroller
    {
        public function CustomGraphContainerController
            (a_graphControlContainer:GraphControlContainer)
        {
            super(a_graphControlContainer);
            _requestor = new SDKRequestor();
            addEventListenersToRequestor();
            //we must wait for the SDK to initialize before we can do
            //anything else with it
            _requestor.addEventListener(Event.COMPLETE, onSDKInitialize);
        }

        private function onSDKInitialize(a_event:Event):void
        {
            _requestor.attemptToAuthenticate();
        }
    }
}
```

All we have to do now is create an HTML page to contain the SWF. We cannot use the standard `index.html` page we have been using so far, as the SDK requires certain JavaScript functions in order to work.

Adobe has provided some **wrapper** HTML pages to use as containers for our SWFs. One of the pages, `sdkindex.html`, is in the `\bin\adobesdk\` folder, alongside a JavaScript file called `FBJSBridge.js`. Copy both `sdkindex.html` and `FBJSBridge.js` into your `\bin\` directory.

Open `sdkindex.html` in a text editor, and find the following line:

```
embedSWF("Visualizer.swf", "flashContent", "600", "300", "10.0");
```

Replace `Visualizer.swf` with the name of your SWF.

Finally, upload `sdkindex.html`, `FBJSBridge.js` and your SWF to your web host to the same directory as usual, and load `http://host.com/sdkindex.html` in your web browser.

## What just happened?

You just added the official Adobe ActionScript 3 SDK for Facebook Platform to your application as an alternative to the classes that we built earlier in the chapter.

The SDK works in a very similar way to our classes, albeit with a lot more polish; it also uses `URLRequest` objects to retrieve data, and decodes the resulting JSON to a native AS3 object.

In order for the SDK to work, it imports two JS files containing JavaScript functions, which actually power the Facebook login and integration. Those two files are called: `FBJSBridge.js`, which is included in the project; and `all.js`, which is loaded from the Facebook server. As long as your webpage uses SWFObject and imports those JavaScript files, you should be able to use the SDK.

## Have a go hero – requesting extended permissions with the SDK

The `Facebook.login()` method takes a second parameter: `options`, a native AS3 `Object`. This is used to request extended permissions; simply provide an object with a string property called `perms`, which is a comma-separated list of the permissions.

Use this to modify your `SDKRequestor.attemptToAuthenticate()` method so that it can take an arbitrary number of extended permissions as parameters and request them using the `Facebook.login()` method.

## Pop Quiz

- 1. What does "me" do?
  - a. Returns the authenticated user's ID
  - b. Stands in as the authenticated user's ID when used in a Graph URL.
  - c. It represents the ID of a Facebook user called Mike Eddington

- 2. Why didn't we obtain an access token using AS3 alone?
  - a. `URLRequest` objects can't load the Facebook login page
  - b. It's only possible to obtain a token using JavaScript
  - c. We wanted to present the user with a genuine Facebook login page, for trust reasons

- 3. Why shouldn't you ask for all the extended permissions your application might possibly need at once?
  - a. It's likely to scare users away
  - b. Only five permissions can be granted at any one time
  - c. This is a violation of the Facebook terms of service

## Summary

In this chapter, the key words were security, permissions, and authentication.
We learned about:

- How Facebook restricts us from accessing information we're not supposed to
- How users determine which information other users (and apps) can see
- How apps can request permission to see more information from users
- How users and apps work together to authenticate with Facebook

## Key takeaways

- The Graph API will only give certain details about a person if you try to access a Graph Object without proof of authentication.
- An access token is proof of authorization, and is associated with both a user and an application.
- Users have two credentials: e-mail address and password.
- Applications have three credentials: application ID (which is like a username), application secret (which is like a password, and can be changed), and API key (which is used to access the Graph API).
