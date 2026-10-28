

4. 在 SDKRequestor.as 中做类似的修改：

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

5. 你需要导入 com.adobe.serialization.json.JSON。测试一下：

![](img/4e826fa36038b86f2087ef8b818d0acf_224_0.png)

## What just happened?

你已向来自你项目的帖子添加了一个应用特定的操作。每个帖子只能添加一个这样的操作，但当然，它不必对每个帖子都相同。

该操作是一个功能性超链接；点击它会带你到我的 Twitter 页面。不过，Facebook 会在所有此类链接末尾添加一个 URL 参数 ref=nf，因此先测试一下——例如，尝试加载 http://google.com?ref=nf 会得到 404 消息。

3. 在 SDKRequestor.as 中添加类似的代码：

```
public function publish(a_publishObject:PublishObject):void
{
    var urlStub:String = "/" + a_publishObject.ownerID + "/feed";
    var actionObj:Object = [ {
        name:"Visit my Twitter Page",
        link:"http://twitter.com/MichaelJW"
    } ];
    var actionString:String = JSON.encode(actionObj);

    var privacyObj:Object = {
        value:"EVERYONE"
    }
    var privacyString:String = JSON.encode(privacyObj);

    Facebook.api(urlStub, publishComplete,
    {
        message:a_publishObject.message,
        picture:a_publishObject.pictureURL,
        link:a_publishObject.linkURL,
        name:a_publishObject.linkName,
        caption:a_publishObject.caption,
        description:a_publishObject.description,
        actions:actionString,
        privacy:privacyString
    },
        URLRequestMethod.POST
    );
}
```

4. 通过将 value 设置为 EVERYONE，我们将使帖子对任何可以看到你 Wall 的人可见，而不仅仅是你的朋友。

还有哪些其他选项？

好吧，value 可以设置为：

- EVERYONE
- ALL_FRIENDS
- NETWORKS_FRIENDS
- FRIENDS_OF_FRIENDS

这些含义都很明显，除了 NETWORKS_FRIENDS（它允许你的朋友加上你网络中的任何人查看帖子）。

value 的另一个可能设置是：CUSTOM。使用它可以让你为 privacyObj 指定另外两个属性——networks 和 friends。

对于 networks 属性，输入应允许查看你帖子的网络的 ID 的逗号分隔列表，如下所示：

```
var privacyObj:Object = {
    value:"EVERYONE",
    networks:"network1,network2,network3"
}
```

要允许所有网络查看帖子，只需将 1 作为值输入即可。

对于 friends 属性，输入以下其中之一：

- EVERYONE
- NETWORKS_FRIENDS
- FRIENDS_OF_FRIENDS
- ALL_FRIENDS
- SELF
- NO_FRIENDS

同样，很清楚每个的作用是什么。NO_FRIENDS 让你仅允许指定网络中的人查看帖子。

friends 的另一个可能设置是：SOME_FRIENDS。这会为你提供可以为 privacyObj 设置的另一对属性——allow 和 deny。这两个属性都接受用户 ID（混合好友列表 ID）的逗号分隔列表；allow 定义谁可以看到帖子，而 deny 定义谁不能。

因此，你的 privacyObj 可能看起来像这样：

```
var privacyObj:Object = {
    value:"CUSTOM",
    networks:"network1,network2,network3",
    friends:"SOME_FRIENDS",
    allow:"bob.smith,joe.q.faikname",
    deny:"bill.the.jerk"
}
```

## What just happened?

正如你所见，你可以精细控制用户 Wall 上帖子的隐私级别。使用“正确”的设置完全取决于你的应用程序及其受众；只需了解可能实现的功能即可。

> 你无法控制用户在其他用户、页面、活动或群组的 Wall 上发布的帖子的隐私设置；这些帖子始终可以被任何可以看到该 Wall 的人看到。

## Source

`source` 参数据说允许你将小型照片库、Flash 对象（如视频剪辑）和 MP3 音乐嵌入到帖子中，以及自定义键值对，这些键值对（虽然在 Facebook 网站上不可见）稍后可以被你的应用程序或任何其他应用程序读取。

不幸的是，在撰写本文时，该功能与 Graph API 的配合并不正确。请查看 `source` 参数可以实现的功能：http://developers.facebook.com/docs/guides/attachments。希望到你阅读这一段时，该功能能得到完全实现并记录在案。

## Deleting Graph Objects

我们已经掌握了添加帖子的方法，但后来如何删除它们呢？

## Time for action – deleting a Post

Facebook 删除帖子的界面是一个在悬停时出现的 Remove 按钮：

![](img/4e826fa36038b86f2087ef8b818d0acf_228_0.png)

Visualizer 项目也有一个 Delete 按钮；让我们看看如何添加它。

## Time for action – deleting Posts using the Visualizer

1. 我们可以通过使用另一个隐藏选项来启用 Visualizer 的 Delete 按钮。在 CustomGraphContainerController.as 的构造函数中，将 this._showDeleteButtons 设置为 true：

```
public function CustomGraphContainerController(a_graphControlContainer: GraphControlContainer)
{
    super(a_graphControlContainer);
    this._showListCounts = true;
    this._showListFilters = true;
    this._canShowSearchUI = true;
    this._publishingCapability = PublishingCapabilities.COMPLETE;
    this._showDeleteButtons = true;
    _requestor = new SDKRequestor();
    addEventListenersToRequestor();
    //we must wait for the Requestor to initialise before we can do
    // anything else with it
    _requestor.addEventListener(Event.COMPLETE, onRequestorInitialize);
    _requestor.initialize();
}
```

2. 下次加载 SWF 时，你将在 List Renderer 中看到这个新按钮：

![](img/4e826fa36038b86f2087ef8b818d0acf_229_0.png)

我们将从（像往常一样）HTTPRequestor 的代码开始，因此请确保你的项目设置为将 _requestor 实例化为 HTTPRequestor 而不是 SDKRequestor。我们可以重用 publish() 的大部分代码；按如下方式修改 HTTPRequestor.as 的 deleteObject() 函数：

```
public function deleteObject(a_objectID:String):void
{
    var loader:URLLoader = new URLLoader();
    var urlRequest:URLRequest = new URLRequest();
    var variables:URLVariables = new URLVariables();

    if (this.accessToken != "")
    {
        variables.access_token = this.accessToken;
    }
    urlRequest.data = variables;

    loader.addEventListener(Event.COMPLETE, onDeleteComplete);
    loader.addEventListener(IOErrorEvent.IO_ERROR, onIOError);
    loader.addEventListener(HTTPStatusEvent.HTTP_STATUS, onHTTPStatusReturned);
    loader.addEventListener(SecurityErrorEvent.SECURITY_ERROR, onSecurityError);
    loader.load(urlRequest);
}
```

4. 请注意，与使用 onPublishComplete() 作为 COMPLETE 事件处理函数不同，我们使用的是 onDeleteComplete()，因此请创建该函数：

```
private function onDeleteComplete(a_event:Event):void
{
    dispatchEvent(new DialogEvent(DialogEvent.DIALOG, "Deleted!"));
}
```

我们需要使用哪个 URL？很简单：我们使用要删除的 Graph 对象的 Graph URL。因此：

```
public function deleteObject(a_objectID:String):void
{
    var loader:URLLoader = new URLLoader();
    var urlRequest:URLRequest = new URLRequest();
    var variables:URLVariables = new URLVariables();

    urlRequest.url = "https://graph.facebook.com/";
    urlRequest.url += a_objectID;

    if (this.accessToken != ""
