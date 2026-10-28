

`requestComplete()` 回调所需的格式几乎与其它回调相同，如下面几行代码所示：

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

将它添加到 `SDKRequestor.as` 中。注意这次我们使用的是 `result` 参数而不是 `success` 参数（这其实并不重要，但名字更准确），而且它只是一个 `Object`，而不是 `FacebookSession` 的实例。

事实上，`result` 的形式与我们在 `HTTPRequestor.onGraphDataLoadComplete()` 中自己创建的 `decodedJSON` 对象完全相同。这意味着我们可以复制粘贴之前处理它的代码——就这么做：

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

你还需要以下导入：

```
import graph.GraphList;
import graph.GraphObject;
import events.RequestEvent;
```

这里有一个大问题。我们在代码中引用了 originalRequest，但我们没有任何办法获取原始的 GraphRequest 实例！

解决方案是使用一种叫做柯里化的编程技巧。这允许我们在发出请求时向回调函数传递额外的值。

> 如果你不明白柯里化是如何工作的，不用担心。我不会在这里解释它，因为它比较高级，而且与 Facebook 关系不大——使用它的代码是特定于 Visualizer 的。关于柯里化的优秀解释，请参阅 Jackson Dunstan 的文章：http://jacksondunstan.com/articles/338。

![](img/4e826fa36038b86f2087ef8b818d0acf_126_0.png)

将 `request()` 函数改为：

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

现在，修改 `requestComplete()` 的签名以接受一个额外参数：

```
private function requestComplete(result:Object, fail:Object, originalRequest:GraphRequest):void
```

SDKRequestor 现在可以进行测试了。切换到 CustomGraphContainerController.as，并将其改为：

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

现在我们要做的就是创建一个包含 SWF 的 HTML 页面。我们不能使用到目前为止一直使用的标准 `index.html` 页面，因为 SDK 需要某些 JavaScript 函数才能工作。

Adobe 提供了一些 **wrapper** HTML 页面作为我们 SWF 的容器。其中一个页面 `sdkindex.html` 位于 `\bin\adobesdk\` 文件夹中，旁边有一个名为 `FBJSBridge.js` 的 JavaScript 文件。将 `sdkindex.html` 和 `FBJSBridge.js` 都复制到你的 `\bin\` 目录中。

在文本编辑器中打开 `sdkindex.html`，并找到以下行：

```
embedSWF("Visualizer.swf", "flashContent", "600", "300", "10.0");
```

将 `Visualizer.swf` 替换为你的 SWF 名称。

最后，将 `sdkindex.html`、`FBJSBridge.js` 和你的 SWF 上传到你的网络主机上的同一目录，如往常一样，并在网络浏览器中加载 `http://host.com/sdkindex.html`。

## What just happened?

你刚刚将官方的 Adobe ActionScript 3 SDK for Facebook Platform 添加到你的应用程序中，作为本章前面构建的类的替代方案。

SDK 的工作方式与我们的类非常相似，只是更加精致；它也使用 `URLRequest` 对象来检索数据，并将结果 JSON 解码为原生的 AS3 对象。

为了使 SDK 正常工作，它导入了两个包含 JavaScript 函数的 JS 文件，这些函数实际上驱动了 Facebook 登录和集成。这两个文件称为：`FBJSBridge.js`，包含在项目中；以及 `all.js`，从 Facebook 服务器加载。只要你的网页使用 SWFObject 并导入这些 JavaScript 文件，你就应该能够使用 SDK。

## Have a go hero – requesting extended permissions with the SDK

`Facebook.login()` 方法接受第二个参数：`options`，一个原生的 AS3 `Object`。它用于请求扩展权限；只需提供一个具有字符串属性 `perms` 的对象，该属性是权限的逗号分隔列表。

使用它来修改你的 `SDKRequestor.attemptToAuthenticate()` 方法，使其可以接受任意数量的扩展权限作为参数并使用 `Facebook.login()` 方法请求它们。

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

在本章中，关键词是安全、权限和身份验证。
我们学习了：

- Facebook 如何限制我们访问不应该访问的信息
- 用户如何决定哪些信息可以被其他用户（和应用）看到
- 应用如何请求查看用户更多信息的权限
- 用户和应用如何共同使用 Facebook 进行身份验证

## Key takeaways

- 如果你尝试在没有身份验证证明的情况下访问 Graph 对象，Graph API 只会提供关于某人的某些细节。
- 访问令牌是授权证明，与用户和应用程序相关联。
- 用户有两个凭证：电子邮件地址和密码。
- 应用程序有三个凭证：应用程序 ID（类似于用户名）、应用程序密钥（类似于密码，可以更改）和 API 密钥（用于访问 Graph API）。

