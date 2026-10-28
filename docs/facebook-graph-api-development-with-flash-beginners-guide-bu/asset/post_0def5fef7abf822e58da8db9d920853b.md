
4. Compile, upload, and execute the Visualizer, and try to post on your own Wall again.

   It still won't work, but you'll see a dialog like this:

   ![](img/4e826fa36038b86f2087ef8b818d0acf_209_0.png)

## What just happened?

The dialog says HTTP status: 403. As mentioned earlier, this means that we are not allowed to access the resource that we were trying to access—that is, we're not allowed to post to the Wall. In other words, we don't have permission... or rather, we haven't been granted the extended permission required.

> It's possible that you'll see an HTTP status code response of 500, instead. That's okay—it's a fairly generic error message which means "there was a problem," in this case. The cause is likely still the lack of the extended permission.
>
> Of course, if you had granted your application the permission needed earlier in the book, you won't have seen any problems at all!

## Time for action – granting the required permission

1. Before this can work, we must grant the application the `publish_stream` extended permission, so alter the `onRequestorInitialize()` function within `CustomGraphContainerController.as`, like so:

   ```
   private function onRequestorInitialize(a_event:Event):void
   {
       _requestor.attemptToAuthenticate(
           ExtendedPermissions.READ_STREAM,
           ExtendedPermissions.PUBLISH_STREAM);
   }
   ```

2. Compile, upload, and try it again. You'll see the Request for permission pop up when you load the page:

   ![](img/4e826fa36038b86f2087ef8b818d0acf_210_0.png)

3. Click on Allow, then open your Feed List Renderer, and click on Publish. Enter some text in the box:

   ![](img/4e826fa36038b86f2087ef8b818d0acf_210_1.png)

   ...and click on Post. Check your Wall on the Facebook website:

   ![](img/4e826fa36038b86f2087ef8b818d0acf_210_2.png)

Did it work?

When publishing, we need to pass a fourth variable:

- The HTTP request method to use

From our work with the HTTP Requestor, you already know that this needs to be POST. So, modify the `publish()` method of `SDKRequestor.as`, like so:

```
public function publish(a_publishObject:PublishObject):void
{
    var urlStub:String = "/me/feed";
    Facebook.api(urlStub, publishComplete,
        { message:a_publishObject.message },
        URLRequestMethod.POST
    );
}
```

Make sure that you import `flash.net.URLRequestMethod`.

Next, you’ll need to create the `publishComplete()` function to be used as a callback:

```
private function publishComplete(result:Object, fail:Object):void
{

}
```

We don't need to add event listeners for HTTP statuses or security errors when using the SDK; just check whether the result is null; if so, more details should be in the fail object:

```
private function publishComplete(result:Object, fail:Object):void
{
    if (result != null)
    {
        dispatchEvent(new DialogEvent(DialogEvent.DIALOG,
            "Publish complete!"));
    }
    else
    {
        dispatchEvent(new DialogEvent(DialogEvent.DIALOG,
            "Publish failed. Details: " + String(fail)));
    }
}
```

Make sure that you import `events.DialogEvent` if you want to use it.

We'll concentrate on `message`, `picture`, `link`, `name`, `caption`, and `description` first, and find out what they do by publishing a rich post that uses them.

The Visualizer can be configured to display a window with input fields for all six of these parameters, instead of just `message`. To do so, find this line in `CustomGraphContainerController.as`:

```
this._publishingCapability = PublishingCapabilities.BASIC;
```

...and replace it with this:

```
this._publishingCapability = PublishingCapabilities.COMPLETE;
```

When you click on the **Publish** button on your `feed` List Renderer, you'll see the new window:

![](img/4e826fa36038b86f2087ef8b818d0acf_215_0.png)

The **Post** button still passes an instance of `PublishObject` to the same function, `publish()`, in the class that `CustomGraphContainerController` extends. The difference from before is that the `PublishObject` has more properties set.

Specifically, it can have any of these properties:

- `message`
- `pictureURL`: Corresponds to `picture`
- `linkURL`: Corresponds to `link`

Similarly, edit the `publish()` function in `SDKRequestor.as`:

```
public function publish(a_publishObject:PublishObject):void
{
    var urlStub:String = "/me/feed";
    Facebook.api(urlStub, publishComplete,
    {
        message:a_publishObject.message,
        picture:a_publishObject.pictureURL,
        link:a_publishObject.linkURL,
        name:a_publishObject.linkName,
        caption:a_publishObject.caption,
        description:a_publishObject.description
    },
    URLRequestMethod.POST
    );
}
```

Test the Visualizer on your host. Here's some test data from Wikipedia and Wikimedia Commons:

- message: Hello Britain!
- picture: http://upload.wikimedia.org/wikipedia/commons/thumb/a/ae/Flag_of_the_United_Kingdom.svg/200px-Flag_of_the_United_Kingdom.svg.png
- link: http://en.wikipedia.org/wiki/Union_Flag
- name: Union Flag
- caption: en.wikipedia.org
- description: The Union Flag, also known as the Union Jack, is the flag of the United Kingdom

This is the result:

![](img/4e826fa36038b86f2087ef8b818d0acf_217_0.png)

![](img/4e826fa36038b86f2087ef8b818d0acf_219_0.png)

A note on images: That small image in the post is still hosted on Wikimedia Commons, but is accessed via a proxy PHP hosted on Facebook's server—http://platform.ak.fbcdn.net/ in this case. The crossdomain.xml on that server is as follows:

```
<cross-domain-policy>
    <site-control permitted-cross-domain-policies="master-only"/>
    <allow-access-from domain="*"/>
</cross-domain-policy>
```

This means that your application can access the BitmapData of the image directly without any sandbox security issues, no matter where the original image was hosted.

The actual Graph Object created looks like this:

```
{
  "id": "«redacted»",
  "from": {
    "name": "Michael James Williams",
    "id": "«redacted»"
  },
  "message": "Hello Britain!",
  "picture": "http://platform.ak.fbcdn.net/www/app_full_proxy.php?app=«redacted»&v=1&size=z&cksum=«redacted»&src=http%3A%2F%2Fupload.wikimedia.org%2Fwikipedia%2Fcommons%2Fthumb%2Fa%2Fae%2FFlag_of_the_United_Kingdom.svg%2F200px-Flag_of_the_United_Kingdom.svg.png",
  "link": "http://en.wikipedia.org/wiki/Union_Flag",
  "name": "Union Flag",
  "caption": "en.wikipedia.org",
  "description": "The Union Flag, also known as the Union Jack, is the flag of the United Kingdom.",
  "icon": "http://graph.facebook.com/images/icons/hidden.gif",
  "actions": [
    {
      "name": "Comment",
      "link": "http://www.facebook.com/«redacted»/posts/«redacted»"
    },
    {
      "name": "Like",
      "link": "http://www.facebook.com/«redacted»/posts/«redacted»"
    }
  ],
  "privacy": {
    "description": "Friends only",
```

Now look again at the Wall post:

![](img/4e826fa36038b86f2087ef8b818d0acf_223_0.png)

The actions are the links underneath the post that other Facebook users can click.

We can add our own user-defined action, such as "Visit my website" or "Plant some crops", by passing it in a JSON to the actions parameter.

## Time for action – literally

1. Open `HTTPRequestor.as` and find the `publish()` function. Create a native AS3 object within the function like so:

   ```
   var actionObj:Object = [ {
       name:"Visit my Twitter Page",
       link:"http://twitter.com/MichaelJW"
   } ];
   ```

   Square brackets define an array, so we could type this as an Array rather than an Object, but it doesn't really matter.

2. Next, in the same function, use the JSON library that we've used before to encode this object into a JSON string:

   ```
   var actionString:String = JSON.encode(actionObj);
   ```

3. Now, add this String as a property of the URLVariables object, called `actions`:

   ```
   variables.message = a_publishObject.message;
   variables.picture = a_publishObject.pictureURL;
   variables.link = a_publishObject.linkURL;
   variables.name = a_publishObject.linkName;
   variables.caption = a_publishObject.caption;
   variables.description = a_publishObject.description;
   variables.actions = actionString;
   urlRequest.data = variables;
   ```
