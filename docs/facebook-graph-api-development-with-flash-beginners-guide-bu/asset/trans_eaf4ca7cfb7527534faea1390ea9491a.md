

## 刚才发生了什么？

一旦用户点击“允许”（Allow），Facebook 就会记住该决定，你的应用将无需再次请求该权限——就像用户首次授权应用查看其基本信息时一样。

你可以一次请求多个扩展权限；只需用逗号将它们分隔开：

```
https://graph.facebook.com/oauth/authorize?client_id=<your_application_id>&type=user_agent&redirect_uri=<your_callback_url>&scope=read_mailbox,user_photos,user_interests
```

不过，扩展权限不仅仅与你有关。授予 `user_interests` 扩展权限后，应用可以访问用户的兴趣列表（当然，这些兴趣以 Graph 对象列表表示）；此外还有一个 `friends_interests` 扩展权限，允许应用访问用户所有好友的兴趣列表。

实际上，大多数扩展权限都有“仅用户”和“好友”两个版本；查看官方文档可了解所有权限的列表：http://developers.facebook.com/docs/authentication/permissions.

## 动手实践 – 请求扩展权限

要让应用始终请求 `user_interests` 扩展权限，我们只需将 `HTTPRequestor.attemptToAuthenticate()` 修改为：

```
public function attemptToAuthenticate():void as shown in the following code:
{
    ExternalInterface.call("window.open",
    "https://graph.facebook.com/oauth/authorize?
    client_id=<your_application_id>&type=user_agent&
    redirect_uri=<your_callback_url>"
    + "&scope=user_interests",
    "facebookLoginWindow",
    "height=370, width=600");
}
```

这样就能正常运行了；它会弹出一个窗口，询问你是否允许应用访问你的个人资料信息。

如果这就是你所需要的——而且以后也只需要这些——那么可以就此打住。但如果你希望让它更灵活一点呢？

首先，我们让 `attemptToAuthenticate()` 函数接受任意数量的扩展权限作为参数，并将它们以逗号分隔的列表形式追加到 `scope` 参数中：

```
public function attemptToAuthenticate(...permissions):void
{
    var scope:String = "";
    if (permissions.length > 0)
    {
        scope = "&scope=" + permissions.join(",");
    }
    ExternalInterface.call("window.open",
        "https://graph.facebook.com/oauth/authorize?client_id=165373950152944&type=user_agent&redirect_uri=http://gamedev.michaeljameswilliams.com/scrap/visualizer/callback.html"
        + scope,
        "facebookLoginWindow",
        "height=370, width=600");
}
```

![](img/4e826fa36038b86f2087ef8b818d0acf_117_0.png)

> 将 `permissions` 作为函数的唯一参数，意味着我们可以向它传递任意数量的参数，它们会组成一个名为 `permissions` 的数组。（这被称为 `...`（rest）参数，如果你想进一步了解的话。）
>
> 然后，我们可以使用 `Array.join()` 方法将该数组中的所有字符串合并为单个字符串，形式为逗号分隔的列表。

我们必须修改 `IRequestor.as` 中方法的签名，以匹配我们对该函数所做的更改：

```
package graph.apis.base
{
    import flash.events.IEventDispatcher;
    import graph.GraphRequest;

    public interface IRequestor extends IEventDispatcher
    {
        function request(a_request:GraphRequest):void;
        function attemptToAuthenticate(...permissions):void;
    }
}
```

## 让我进去！

现在，我们只需将所需的扩展权限传递给该方法。在 `CustomGraphContainerController.as` 中，修改构造函数：

```
public function CustomGraphContainerController(a_graphControlContainer:GraphControlContainer)
{
    super(a_graphControlContainer);
    _requestor = new HTTPRequestor();
    addEventListenersToRequestor();
    _requestor.attemptToAuthenticate("friends_interests",
        "read_mailbox");
}
```

编译 SWF 并上传到你的主机。在浏览器中测试时，它会要求你授予这两项权限：

![](img/4e826fa36038b86f2087ef8b818d0acf_118_0.png)

Visualizer 项目包含一个 `ExtendedPermissions.as` 类，旨在让这一过程更简单。它为每个扩展权限都定义了一个 `public static const`，并在注释中附带简短的功能说明。

## 动手实践 – 实现 SDK

SDK 源文件已下载并复制到 Visualizer 项目目录中；你可以在 `\src\com\facebook\` 下找到它们。（有关下载最新版 SDK 并将其包含到你自己的项目中的信息，请参阅第 1 章。）

我们将构建另一个 Requestor，不过这次它将由 SDK 驱动，而不是我们自己的 `URLRequest`。首先在 `\src\apis\` 中创建一个名为 `\sdk\` 的新文件夹。然后，在该文件夹中创建一个名为 `SDKRequestor.as` 的新文件。让它像 `HTTPRequestor` 一样继承 `EventDispatcher` 并实现 `IRequestor`。

以下是该类的基本代码：

```
package graph.apis.sdk
{
    import flash.events.EventDispatcher;
    import flash.events.IEventDispatcher;
    import graph.apis.base.IRequestor;
    import graph.GraphRequest;

    public class SDKRequestor extends EventDispatcher
        implements IRequestor
    {

        public function SDKRequestor(target:IEventDispatcher = null)
        {
            super(target);
        }

        public function request(a_request:GraphRequest):void
        {
        }

        public function attemptToAuthenticate(...permissions):void
        {
        }
    }
}
```

接下来，导入 `Facebook` 类。这是用于基于 Web 应用的 API：

```
import com.facebook.graph.Facebook;
```

在使用这个类之前，需要先对其进行初始化。为此，我们必须向它传递应用的 ID 和一个回调函数。请在 `SDKRequestor()` 构造函数中完成：

```
public function SDKRequestor(target:IEventDispatcher = null)
{
    super(target);

    Facebook.init("«your_application_id»", initComplete);
}
```

接下来，我们必须创建 `initComplete()` 函数，它会在初始化过程完成后被调用。请将以下代码添加到 `SDKRequestor.as` 中：

```
private function initComplete(success:Object, fail:Object):void
{
}
```

该函数需要这两个参数——`success` 和 `fail`——否则代码将无法编译。如果初始化没有问题，那么 `success` 将是一个 `com.Facebook.graph.data.FacebookSession` 类型的对象；否则，`success` 将为 `null`，而 `fail` 将包含详细信息。让我们用以下代码来处理：

```
private function initComplete(success:Object, fail:Object):void
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

你还必须导入 `FacebookSession`，如下所示：

```
import com.facebook.graph.data.FacebookSession;
```

我们让它准备就绪时派发一个 `COMPLETE` 事件，这样就能知道调用其他方法是安全的：

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

为此，你必须导入 `flash.events.Event`，否则无法正常工作。

API 初始化完成后，我们可以使用 `Facebook.login()` 方法对用户进行身份验证。对该方法的调用应放在 `attemptToAuthenticate()` 中：

```
public function attemptToAuthenticate(...permissions):void
{
    Facebook.login(loginComplete);
}
```

这将弹出一个窗口，要求用户登录并允许我们的应用访问其个人资料。如果没有弹出窗口，请检查是否启用了 Cookie；否则将无法正常工作。

传给 `Facebook.login()` 的 `loginComplete` 参数是另一个回调函数，其形式与 `initComplete()` 相同，会在用户完成身份验证后被调用。请将其添加到 `SDKRequestor.as` 中：

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

