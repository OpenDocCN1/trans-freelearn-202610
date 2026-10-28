

```
{
    variables.access_token = this.accessToken;
}
urlRequest.data = variables;

loader.addEventListener(Event.COMPLETE, onDeleteComplete);
loader.addEventListener(IOErrorEvent.IO_ERROR, onIOError);
loader.addEventListener(HTTPStatusEvent.HTTP_STATUS,
    onHTTPStatusReturned);
loader.addEventListener(SecurityErrorEvent.SECURITY_ERROR,
    onSecurityError);
loader.load(urlRequest);
}
```

5. 那么HTTP请求方法呢？有一种叫做DELETE的方法，它绝对不是“安全”方法，而且正是我们想要的。不幸的是，出于安全考虑，我们只能在AIR中使用它。下面是代码，以防你以后编写AIR应用程序：

```
public function deleteObject(a_objectID:String):void
{
    var loader:URLLoader = new URLLoader();
    var urlRequest:URLRequest = new URLRequest();
    var variables:URLVariables = new URLVariables();

    urlRequest.url = "https://graph.facebook.com/";
    urlRequest.url += a_objectID;
    urlRequest.method = URLRequestMethod.DELETE;

    if (this.accessToken != "")
    {
        variables.access_token = this.accessToken;
    }
    urlRequest.data = variables;

    loader.addEventListener(Event.COMPLETE, onDeleteComplete);
    loader.addEventListener(IOErrorEvent.IO_ERROR, onIOError);
    loader.addEventListener(HTTPStatusEvent.HTTP_STATUS,
        onHTTPStatusReturned);
    loader.addEventListener(SecurityErrorEvent.SECURITY_ERROR,
        onSecurityError);
    loader.load(urlRequest);
}
```

不过，如果你是在制作常规的Flash Player SWF，URLRequestMethod类甚至不会有DELETE常量。

对于基于常规SWF的应用程序，情况并没有复杂多少。我们只有两种请求方法可选——GET和POST。因为GET是安全方法，我们不能用它来删除服务器上的对象，所以必须使用POST：

```
public function deleteObject(a_objectID:String):void
{
var loader:URLLoader = new URLLoader();
var urlRequest:URLRequest = new URLRequest();
var variables:URLVariables = new URLVariables();

urlRequest.url = "https://graph.facebook.com/";
urlRequest.url += a_objectID;
urlRequest.method = URLRequestMethod.POST;

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

……但我们可以轻松地通过传递一个新参数——method=delete——来告诉Facebook将其视为DELETE请求：

```
public function deleteObject(a_objectID:String):void
{
var loader:URLLoader = new URLLoader();
var urlRequest:URLRequest = new URLRequest();
var variables:URLVariables = new URLVariables();

urlRequest.url = "https://graph.facebook.com/";
urlRequest.url += a_objectID;
urlRequest.method = URLRequestMethod.POST;

if (this.accessToken != "")
{
    variables.access_token = this.accessToken;
}
variables.method = "delete";
urlRequest.data = variables;

loader.addEventListener(Event.COMPLETE, onDeleteComplete);
loader.addEventListener(IOErrorEvent.IO_ERROR, onIOError);
loader.addEventListener(HTTPStatusEvent.HTTP_STATUS,
    onHTTPStatusReturned);
loader.addEventListener(SecurityErrorEvent.SECURITY_ERROR,
    onSecurityError);
loader.load(urlRequest);
}
```

这个方法和AIR版本一样有效，而且只多花我们一行代码。

要使用SDK删除对象，我们可以使用`Facebook.api()`，但有一个快捷方法：`Facebook.deleteObject()`。它接受两个参数——要删除的Graph Object的ID和一个回调函数。这使得所需代码非常简单；将其添加到SDKRequestor.as中：

```
public function deleteObject(a_objectID:String):void
{
    Facebook.deleteObject(a_objectID, deleteComplete);
}

private function deleteComplete(result:Object, fail:Object):void
{
    if (result != null)
    {
        dispatchEvent(new DialogEvent(DialogEvent.DIALOG, "Deleted!"));
    }
    else
    {
        dispatchEvent(new DialogEvent(DialogEvent.DIALOG,
            "Deletion failed. Details: " + String(fail)));
    }
}
```

编译、上传，然后通过删除你墙上的一条帖子来测试新的更改。

## 刚才发生了什么？

你应该已经看到一条墙上的帖子从Facebook上被删除了。

> 最新消息：在本书即将出版之前，Facebook似乎改变了Graph API，使得墙上的帖子只能由最初用于发布该帖子的应用程序删除。以前，可以删除已登录用户墙上的任何帖子，也可以删除该用户在他人墙上发布的任何帖子。在撰写本文时，官方Graph API文档并未提及此限制。因此，如果你在删除帖子时遇到问题，请先通过Visualizer创建一个帖子，然后再尝试删除它。

这再次只需要`publish_stream`扩展权限；到目前为止，你应该已经清楚这个权限有多强大了。你可以使用相同的代码删除其他类型的Graph Object，而不仅仅是帖子。如果你的应用程序有权限修改或创建对象，那么它也有权限删除它。

如果你尝试通过Facebook网站删除帖子，它会要求确认：

![](img/4e826fa36038b86f2087ef8b818d0acf_235_0.png)

这是个好主意，值得在你自己的项目界面中使用。Visualizer不会要求确认，而且即使删除后也不会从List Renderer中移除帖子——尽管这确实让你看到如果尝试删除一个已经被删除的帖子会发生什么……

## 发布其他类型的Graph Object

所有可以创建的Graph Object类型都可以按照与墙帖子相同的基本步骤来创建：

- 使用`POST HTTP`请求方法
- 使用`URLVariables`对象设置参数
- 确保你拥有访问令牌和所需的权限

Graph List Renderer的Publish按钮总是会创建一个窗口，其中的输入与正在渲染的Graph List相对应，而这个窗口又总是将其值传递给`CustomGraphContainerController`的`publish()`函数（在一个`PublishObject`实例内部），因此你可以试验不同的类型。

每当有人评论该帖子时，用户还会收到电子邮件和通知。这就像一个微型订阅，也是一种认可的标志。

请求的URL是：

```
https://graph.facebook.com/<post_id>/likes
```

没有任何参数。

点赞需要`publish_stream`权限。

## 那么“点赞”其他Graph Object呢？

在第2章中，我们了解了“点赞”连接如何将人们与电视节目、电影、音乐、兴趣和活动联系起来；“点赞”连接构成了Graph Web中的许多分支。然而，Graph API不允许我们“点赞”帖子以外的任何东西（即使尝试点赞评论也会导致HTTP状态码500）。

Facebook表示，他们不打算允许开发者为其他任何东西创建“点赞”。他们的目标似乎是确保任何用户都不会点击非官方的“点赞”按钮（或者至少，不会在Facebook未批准的环境中点击“点赞”按钮；Yelp.com有自己设计的“点赞”按钮，因为他们是Facebook的合作伙伴网站）。

所以，遗憾的是，无法通过Graph API修改用户的电视节目、电影、兴趣或活动。

## 删除点赞

点赞没有自己的ID，所以我们不能通过尝试删除 https://graph.facebook.com/<like_id> 来删除点赞。相反，我们必须使用这个URL：https://graph.facebook.com/<graph_object_id>/likes。

如你所知，这是连接的Graph URL，如果我们试图在浏览器中加载它，它会返回一个Graph List。然而，当我们尝试使用“删除Graph Objects”一节中描述的过程删除该URL时，它会使授权用户“取消赞”该Graph Object。

