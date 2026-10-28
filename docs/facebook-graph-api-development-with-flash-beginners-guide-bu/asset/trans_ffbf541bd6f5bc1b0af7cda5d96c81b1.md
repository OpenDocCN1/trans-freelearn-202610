

```
{
    public var rendererObject:GraphObjectRenderer;
    public var graphObjectListRenderers:Array = [];

    public function GraphObject()
    {

    }

}
}
```

老实说，我们无需担心这里的任何代码。你只需要知道它是动态的，也就是说我们可以在运行时为它分配新的属性，而不必预先指定它们的名称。所以我们可以这样做：

```
var graphObject:GraphObject = new GraphObject();
graphObject.favoriteColor = "red";
```

当一个 GraphObject 被传递给 CustomGraphContainerController.renderGraphObject() 方法时，该 GraphObject 的每一个属性都会被自动渲染到一个漂亮的列表中。也就是说，除了类中已经定义的那两个属性之外的所有属性！

因此，我们必须在 CustomGraphContainerController.onGraphDataLoadComplete() 内完成的工作是：

- 1. 创建一个 GraphObject 的新实例。
- 2. 将 decodedJSON 的所有属性复制到这个新的 GraphObject 中。
- 3. 将 GraphObject 传递给 renderGraphObject()。
- 4. 实现该操作的代码如下：

```
private function onGraphDataLoadComplete(a_event:Event):void
{
    var loader:URLLoader = a_event.target as URLLoader;
    //obtain whatever data was loaded, and trace it
    var graphData:String = loader.data;
    var decodedJSON:Object = JSON.decode(graphData);

    var graphObject:GraphObject = new GraphObject();
    //copy all the properties from decodedJSON to graphObject
    for (var key:String in decodedJSON)
```

```
{
    graphObject[key] = decodedJSON[key];
}
this.renderGraphObject(graphObject);
}
```

5. 编译并测试。SWF 显示在下面的截图中：

![](img/4e826fa36038b86f2087ef8b818d0acf_49_0.png)

你可以多次点击“放大”按钮，使渲染器更大更清晰，如上图所示。你的渲染器可能以不同于图示的顺序显示字段；Facebook 会以任意顺序返回字段。

## 刚才发生了什么？

舞台上出现的窗口就是我所称的渲染器——具体来说，是一个 Graph Object 渲染器。你可以通过标题栏拖动它，内容可以滚动，也可以通过点击右上角的按钮来关闭它。

因此，你已经成功地从 Facebook 的 Graph API 获取数据，并将它显示在 SWF 中。你的 SWF 非常灵活；将 request.url 改为指向另一个 Facebook 对象的 Graph URL，你就会看到它显示在渲染器中。

GraphObject 中的大部分数据会以简单的“key: value”格式显示在窗口内的文本区域中。Page 的 name 字段显示在窗口的标题栏中，如果 Page 具有 picture 字段（我们可以从 JSON 中看到 PacktPub 确实有），则图像会被下载，并通过 Loader 显示在渲染器中。

> 与 URLLoader 一样，flash.display.Loader 类会下载给定 URLRequest 所指向的对象，并在就绪时分派一个 COMPLETE 事件。与 URLLoader 不同的是，Loader 用于下载图像和 SWF，而且该事件实际上是由其子对象之一 contentLoaderInfo 分派的。此外，Loader 扩展了 DisplayObject，并在下载完成后呈现图像的外观。
>
> Flash 的安全模型会阻止图像数据被与其所在域不同的 SWF 访问，除非图像所在域中存在允许该访问的跨域策略文件。幸运的是，Facebook 的跨域策略文件非常宽松，允许所有域进行此类访问。

所以，事实上，这仅仅是以图形方式表示 Graph API 中的 Page 对象的一种方式。

## 理解连接

你可能会想：“这固然不错，但它并没有显示与 Page 关联的所有数据，是吗？墙帖和照片在哪里呢？”

## 动手实践——在浏览器中查找连接

Facebook 将墙帖、照片、视频甚至状态都视为 Graph API 中的独立对象，而不是将它们全部塞进一个 Page 对象中。例如，下面是一个表示 Packt Publishing 单条 Post 的对象：

```
{
  "id": "204603129458_127056137323572",
  "from": {
    "name": "Packt Publishing",
    "category": "Products_other",
    "id": "204603129458"
  },
  "message": "The Amazon SimpleDB Developer Guide has been published! Get your copy now! http://bit.ly/b1FQUG",
  "picture": "http://external.ak.fbcdn.net/safe_image.php?d=c4a7887cb52dd8f93e439aaec13c034b&w=130&h=130&url=https%3A%2F%2Fwww.packtpub.com%2Fsites%2Fdefault%2Ffiles%2Fimagecache%2Fproductvw%2F2F7344EN_MockupCover%2520Template.jpg",
  "link": "http://bit.ly/b1FQUG"
}
```

```
"name": "Amazon SimpleDB Developer Guide | Packt Publishing Technical & IT Book Store",
"caption": "bit.ly",
"description": "Gain in-depth understanding of Amazon SimpleDB with PHP, Java, and Python examples, and run optimized database-backed applications on Amazon\\'s Web Services cloud",
"icon": "http://static.ak.fbcdn.net/rsrc.php/zB010/hash/9yvl7ltw.gif",
"type": "link",
"created_time": "2010-06-04T12:39:44+0000",
"updated_time": "2010-06-04T12:39:44+0000",
"likes": 1
}
```

这个对象现在已经过期，无法再通过 Graph API 获取，但正如你所猜测的，它之前可以在 https://graph.facebook.com/204603129458_127056137323572 访问到。它采用与 Page 对象相同的格式——尽管字段略有不同——因此我们的 Graph Object 渲染器可以正常渲染它。

当然，除非我们知道与 Packt Publishing 关联的每个 Post 的 ID，否则这毫无用处，而且我们无从得知去哪里找这些 ID。真的是这样吗？

我之前说过，Graph API 被设计为自文档化的。我们可以通过在任意 Graph URL 末尾添加一个 metadata=1 标志来请求关于任何 Graph Object 的额外“元”信息。请在浏览器中查看：https://graph.facebook.com/PacktPub?metadata=1。JSON 中出现了一个新属性 type：

```
"type": "page"
```

这很有用；正如我所说，Posts 和 Pages（实际上所有 Graph Objects）都采用相同的格式，因此这为我们提供了一种区分它们的方法。

不过，更直接有趣的是新的 metadata 对象。它包含一个对象 connections 和一个数组 fields。我们先来看看 fields：

```
"fields": [
{
"name": "id",
"description": "The page's ID"
},
{
"name": "name",
"description": "The page's name"
},
{
"name": "picture",
"description": "The pages profile picture"
```

```
},
{
  "name": "category",
  "description": "The page's category"
},
{
  "name": "fan_count",
  "description": "\\/* The number of fans the page has"
}
]
```

这个列表说明了 Graph Object 主体中每个字段的含义。在撰写本文时，这仍然是一个相当新的功能，因此当你加载时，列表可能会更完整。

connections 对象如下：

```
"connections": {
  "feed": "https://graph.facebook.com/packtpub/feed",
  "posts": "https://graph.facebook.com/packtpub/posts",
  "tagged": "https://graph.facebook.com/packtpub/tagged",
  "statuses": "https://graph.facebook.com/packtpub/statuses",
  "links": "https://graph.facebook.com/packtpub/links",
  "notes": "https://graph.facebook.com/packtpub/notes",
  "photos": "https://graph.facebook.com/packtpub/photos",
  "albums": "https://graph.facebook.com/packtpub/albums",
  "events": "https://graph.facebook.com/packtpub/events",
  "videos": "https://graph.facebook.com/packtpub/videos"
}
```

浏览上面列表中的某个 URL：http://graph.facebook.com/packtpub/posts。它返回一个 JSON，包含一个名为 data 的数组和一个名为 paging 的对象。data 数组包含多个 Post 对象；我们将在本书后面讨论 paging。

## 刚才发生了什么？

metadata=1 参数告诉 Graph API 显示当前对象的所有元数据，在本例中，包括该对象的类型、对象属性描述的数组，以及包含与此 Page 连接的对象的列表的所有 URL。

Graph API 名称正源于这种布局。在日常用法中，“graph”指的是下图所示的那种图表：

