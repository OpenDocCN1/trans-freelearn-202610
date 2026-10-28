Let's make it dispatch a COMPLETE event when it's ready, so that we know it's safe to call other methods:

```
private function initComplete(success:Object, fail:Object):void
{
    if (success is FacebookSession)
    {
        dispatchEvent(new Event(Event.COMPLETE))
    }
    else
    {
        //was a failure. See contents of 'fail' object.
    }
}
```

You must import flash.events.Event for this to work.

Once the API is initialized, we can authenticate the user using the Facebook.login() method. The call to this should go in attemptToAuthenticate():

```
public function attemptToAuthenticate(...permissions):void
{
    Facebook.login(loginComplete);
}
```

This will cause a pop-up window to appear, asking the user to log in and allow our application to access the user's profile. If no pop-up appears, check that cookies are enabled; it won't work otherwise.

The loginComplete parameter passed to Facebook.login() is another callback function, of the same form as initComplete(), which is called once the user has authenticated. Add this to SDKRequestor.as:

```
private function loginComplete(success:Object, fail:Object):void
{
    if (success is FacebookSession)
    {
        //was a success.
    }
    else
    {
        //was a failure. See contents of 'fail' object.
    }
}
```