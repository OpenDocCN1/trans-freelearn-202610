4. Make similar changes in SDKRequestor.as:

```
public function publish(a_publishObject:PublishObject):void
{
    var urlStub:String = "/" + a_publishObject.ownerID + "/feed";
    var actionObj:Object = [ {
        name:"Visit my Twitter Page",
        link:"http://twitter.com/MichaelJW"
    } ];
    var actionString:String = JSON.encode(actionObj);
    Facebook.api(urlStub, publishComplete,
    {
        message:a_publishObject.message,
        picture:a_publishObject.pictureURL,
        link:a_publishObject.linkURL,
        name:a_publishObject.linkName,
        caption:a_publishObject.caption,
        description:a_publishObject.description,
        actions:actionString
    },
        URLRequestMethod.POST
    );
}
```

5. You will need to import com.adobe.serialization.json.JSON. Test this out:

![](bbox=[0.18, 0.52, 0.82, 0.64])

# What just happened?

You've added an app-specific action to posts coming from your project. You can add only one such action to each post, but of course, it doesn't have to be the same for every single post.

The action is a functional hyperlink; clicking on it would take you to my Twitter page. Facebook adds a URL parameter, ref=nf, to the end of all such links, though, so test them out first—for example, trying to load http://google.com?ref=nf gives you a 404 message.