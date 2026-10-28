```
}
Facebook.api(
    urlStub,
    searchComplete,
    { metadata: 1, q:a_query, type:a_type }
);
}
```

We don't need to change anything else, as these searches will still return a Graph List.
Test it out:

![](bbox=[0.25, 0.295, 0.75, 0.61])

Success. Adding the Friend's Posts search will be a little trickier, but not much. First we have to allow the Requestors' search() functions to accept another parameter: User ID.

Open IRequester.as and add this parameter to the function signature:

```
package graph.apis.base
{
    import flash.events.IEventDispatcher;
    import graph.GraphRequest;

    public interface IRequestor extends IEventDispatcher
    {
        function request(a_request:GraphRequest):void;
        function attemptToAuthenticate(...permissions):void;
```