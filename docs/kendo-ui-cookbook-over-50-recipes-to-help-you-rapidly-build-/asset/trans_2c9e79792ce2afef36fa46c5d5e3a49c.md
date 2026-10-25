

## How it works...

如前所述，在初始化 Editor 小部件时指定工具配置，默认工具将被配置中指定的工具替换，如下图所示：

![](img/2a66507445090790d34dc5588b56b24c_79_0.png)

## There's more...

某些工具，例如 fontName 和 fontSize，会在下拉列表中列出可能的值。默认情况下，这些下拉列表中会列出一系列字体和字体大小。此列表可以在工具配置中自定义：

```
$("#editor").kendoEditor({
 tools: [
 {
 name: "fontName",
 items: [{
 text: "Default",
 value: "Arial, Verdana, sans-serif"
 }, {
 text: "Monospace",
 value: "monospace"
 }]
 }, {
 name: "fontSize",
 items: [{
 text: "Small",
 value: "12px"
 }, {
 text: "Medium",
 value: "16px"
 }, {
 text: "Large",
 value: "20px"
 }
 ]
 }
 ]
});
```

在前面的代码片段中，tools 配置包含一个对象数组。每个对象都有一个 name 属性，用于指定要使用的工具的名称，以及一个 items 数组，用于列出要在下拉列表中显示的值。考虑 fontName 工具使用的 items 数组。它包含一组对象，每个对象都有两个属性，即 text 和 value。text 属性指定要显示的文本，value 属性指定当用户从列表中选择相同值时要应用的值。在这种情况下，如果用户从列表中选择 Default，则将应用 Arial, Verdana, sans-serif 字体。

![](img/2a66507445090790d34dc5588b56b24c_80_0.png)

在许多情况下，您可能希望创建自己的自定义命令，以与 Editor 界面进行交互。例如，一个向 Editor 插入文本的自定义命令。除了用户可以添加到工具集中的各种工具之外，该库还允许您定义与 Editor 交互的自定义命令：

```
$("#editor").kendoEditor({
    tools: [{
        name: "custom",
        exec: function(e) {
            var editor = $(this).data('kendoEditor');
            editor.exec("inserthtml",
                {value: "Thanks,<br>Sagar Ganatra."});
        }
    }]
});
```

## Kendo UI Editor

在这里，工具的名称是 `custom`，它包含一个名为 `exec` 的函数。当用户单击自定义工具时，将执行 `exec` 函数，如下图所示。该函数首先获取 Editor 小部件的引用，然后在 Editor 实例上调用 `exec` 函数。该函数接受两个参数，即命令名称和一个包含要执行命令的一组参数的对象。在前面的示例中，使用 `inserthtml` 命令向 Editor 小部件插入文本。要插入的文本作为第二个参数指定。

![](img/2a66507445090790d34dc5588b56b24c_81_0.png)

要使用的命令名称应为 Editor 命令之一。

## Using the image browser tool to insert images into the Editor

Editor 工具集中可用的工具之一是图片浏览器。图片浏览器工具允许您将图片插入到 Editor 中。它通过显示对话框来询问用户图片 URL 和可选的替代文本，如下图所示：

![](img/2a66507445090790d34dc5588b56b24c_81_1.png)

然而，当今构建的许多丰富应用程序允许用户在存储库中浏览图片、上传图片，以及从同一存储库中删除图片。Editor 中使用的图片浏览器工具可以自定义以允许这些操作。在本示例中，我们将了解如何使用图片浏览器工具来处理此类场景。

## Getting ready

在本示例中，我们将首先了解如何从存储库读取图片并在图片浏览器中显示缩略图。这需要一个服务器端应用程序来响应请求，该请求返回一个 JSON 结构，其中提供元数据，例如图片名称和文件大小。

此外，要显示一组缩略图，还需要一个返回请求的缩略图的服务。请求的格式和回退选项在本示例中进行了说明。

## How to do it...

如前所述，我们需要一个返回包含图片元数据的 JSON 结构的服务。此数据应以以下代码片段所示的格式发送：

```
[
    {
        "name": "OperaHouse1.jpg",
        "type": "f",
        "size": 73289
    },
    {
        "name": "OperaHouse2.jpg",
        "type": "f",
        "size": 15289
    },
    {
        "name": "HarborBridge.jpg",
        "type": "f",
        "size": 6286
    }
]
```

在这里，结构是一组对象，每个对象指定图片名称（`name` 属性）和图片大小（`size` 属性）。`type` 属性包含值 `f`，表示它代表文件类型。在服务器上创建目录的场景中，`type` 属性应具有值 `d`。文件大小应以字节为单位。但是，在显示图片元数据时，它将转换为千字节。

## Kendo UI Editor

下一步是指定传输 URL 以读取前面提到的结构，并从存储库获取缩略图：

```
$("#editor").kendoEditor({
    tools: [ 'insertImage' ],
    imageBrowser: {
        transport: {
            read: "/app/ImageService/Read",
            thumbnailUrl: "/app/ImageService/ThumbNailService",
            imageUrl: "/app/ImageService/Image?path={0}"
        }
    }
});
```

在这里，imageBrowser 配置选项指定传输 URL，用于读取包含元数据的 JSON 结构（read 属性）、返回缩略图的服务 URL（thumbnailUrl 属性）以及返回要插入到 Editor 中的图片的服务（imageUrl 属性）。

## How it works...

当您指定 imageBrowser 配置时，默认对话框将被覆盖，并显示如下图所示的图片浏览器模态窗口：

![](img/2a66507445090790d34dc5588b56b24c_83_0.png)

当您单击图片浏览器工具时，首先会从 read 属性的值中提到的 URL 获取图片元数据。对于 JSON 结构中的每个对象，都会引用缩略图服务来获取图片的缩略图。在我们的示例中，图片名称为 HarborBridge.jpg，发送到缩略图服务的请求将是 http://<domain_name>/app/ImageService/ThumbNailService?path=HarborBridge.jpg。

在这里，查询参数 path 包含要检索其缩略图的图片名称。由于 JSON 结构中有三个对象，因此将向缩略图服务发送三个请求。

前面的截图中显示的缩略图尺寸为 80px X 60px，并显示图片名称及其大小。图片浏览器工具还有一个搜索输入字段，用于过滤浏览器中显示的缩略图列表。如果您有大量缩略图列表并正在按名称查找图片，这将特别有用。

从列表中选择缩略图并单击 Insert 按钮后，将从 imageUrl 中提到的服务获取相应的图片。请注意 imageUrl 中 {0} 的用法：

/app/ImageService/Image?path={0}

在这里，{0} 将被替换为所选文件名。获取的图片随后将插入到 Editor 中。

## There's more...

图片浏览器工具还允许您上传图片，然后将其插入到 Editor 中。要将图片上传到服务器，请指定上传服务 URL：

```
imageBrowser: {
transport: {
read: "/app/ImageService/Read",
thumbnailUrl: "/app/ImageService/ThumbNailService",
imageUrl: "/app/ImageService/Image?path={0}",
uploadUrl: "/app/ImageService/Upload"
}
}
```

在这里，uploadUrl 属性指定将图片上传到服务器的服务。当指定 uploadUrl 属性时，图片浏览器中将出现一个 **Upload** 按钮。

![](img/2a66507445090790d34dc5588b56b24c_85_0.png)

单击 **Upload** 按钮（前面的截图中已突出显示）后，系统会提示用户指定文件位置。将向上传指定图片的服务发送一个 POST 请求。

图片上传到服务器后，将查询缩略图服务以获取同一图片的缩略图。上传服务应确保上传的图片已调整大小，并在缩略图所在的目录中可用。

在某些场景中，您可能希望在服务器上创建目录并上传图片到该目录。要创建目录，请指定 create 属性，其值为可创建目录的服务 URL：

```
imageBrowser: {
    transport: {
        read: "/app/ImageService/Read",
```

