```
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
```

You'll also need the following imports:

```
import graph.GraphList;
import graph.GraphObject;
import events.RequestEvent;
```

There's a big problem here, though. We refer to originalRequest in the code, but we have no way of obtaining the original GraphRequest instance!

The solution is to use a coding trick called currying. This allows us to pass extra values to the callback function at the time that we make the request.

> Don't worry if you don't understand how currying works. I'm not going to explain it here, as it's fairly advanced and not really relevant to Facebook – the code that uses it is specific to the Visualizer. For an excellent explanation of currying, see the article by Jackson Dunstan at: http://jacks ondunstan.com/articles/338.

![](bbox=[0.22, 0.68, 0.28, 0.74])