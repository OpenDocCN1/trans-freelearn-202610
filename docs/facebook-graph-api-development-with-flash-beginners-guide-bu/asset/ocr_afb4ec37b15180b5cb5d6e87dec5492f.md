# Let Me In!

To see how much information your friends can access about you through applications, log in to the Facebook website, click on Account | Privacy Settings, select Edit your settings for using applications, games and websites, and view the settings for Information accessible through your friends:

![](bbox=[0.195, 0.22, 0.81, 0.53])

# That's cheating!

Okay, fine, generating an access token in the browser and then pasting it directly into the source code is not a great way of handling authentication. We need to obtain an access token in our application itself.

# Time for action – authenticating through the application

When authenticating in the browser, we visit https://graph.facebook.com/oauth/authorize?client_id=<your_application_id>&redirect_uri=http://google.com&type=user_agent, so let's start by loading that URL in our code.

Start by creating a new function, attemptToAuthenticate(), in HTTPRequestor.as:

```
public function attemptToAuthenticate():void
{
    var loader:URLLoader = new URLLoader();
    var urlRequest:URLRequest = new URLRequest();
```