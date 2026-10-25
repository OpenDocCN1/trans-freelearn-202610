## Kendo UI 文件上传器

在本章中，我们将介绍以下配方：

- 使用文件上传器将文件上传到服务器
- 异步将文件上传到服务器
- 监听文件上传事件

### 简介

Kendo UI 库提供了一个文件上传器小部件，可用于将多个文件上传到服务器。在 Chrome、Safari、Firefox 和 Opera 等现代浏览器上，可以使用 HTML5 File API 异步上传文件。HTML5 对 XmlHttpRequest 对象的增强功能（例如进度跟踪）也可用。在旧版浏览器上，它会回退并同步上传文件。在本章中，我们将了解文件上传器小部件可用于将文件上传到服务器的各种方式，展示文件上传过程的进度以及可用于自定义小部件的选项。

### 使用文件上传器将文件上传到服务器

在本配方中，我们将了解如何使用文件上传器小部件将多个文件同步上传到服务器。

#### 如何操作...

让我们创建一个用于上传文件的表单，如下所示：

```html
<form method="post"
 action="/fileUploadService"
 style="width:500px">

<input name="files" id="fileUpload" type="file" />
<input type="submit" value="Submit" class="k-button" />
</form>
```

下一步是通过在文件输入类型上调用 kendoUpload 函数来初始化文件上传器小部件：

```javascript
$('#fileUpload').kendoUpload();
```

这将初始化文件输入类型为上传器小部件。然后可以使用此上传器小部件选择文件并将其上传到服务器。你可以选择多个文件，列表中也会显示相同内容。

当你点击 **Submit** 按钮时，表单将提交以上传列表中的文件。这是一个 `multipart/form-data` 提交，文件数据位于请求负载中。服务器将能够接受表单提交并将用户路由到下一页。

#### 工作原理...

当你初始化文件上传器小部件时，你会看到两个按钮，**Select files...** 和 **Submit**，如下图所示：

![](img/2a66507445090790d34dc5588b56b24c_103_0.png)

请注意文件输入类型的外观和感觉——有一个按钮，并且这在所有浏览器中都是一致的。该按钮允许你选择多个文件。

为了本示例的目的，让我们选择文件两次，即点击 **Select files...** 按钮两次并选择一个文件：

![](img/2a66507445090790d34dc5588b56b24c_104_0.png)

在这里，你会注意到列表中显示了两个文件。当你点击 **Submit** 按钮时，带有请求负载中文件的 POST 请求将发送到文件上传服务。请注意，列表中还有一个叉号 (x) 按钮，可用于从列表中删除文件。

#### 更多...

有几个配置选项可用于自定义文件上传器小部件。

#### 禁用多文件上传

默认情况下，文件上传器允许用户将多个文件上传到服务器。但是，在许多场景中，只允许上传一个文件是可取的。要实现此功能，请在初始化小部件时将 multiple 属性设置为 false：

```javascript
$('#fileUpload').kendoUpload({
    multiple: false
});
```

这将只允许选择一个文件。当你再次点击 **Select files...** 按钮并选择另一个文件时，列表将反映最近选择的文件。

#### 使用模板自定义列表项的显示

用于显示所选文件的列表可以通过为列表中的项提供模板来自定义。模板中有三个可用变量：name、size 和 extension。

```html
<script id="fileListTemplate" type="text/x-kendo-template">
<div>
    <span>Name: #=name#</span>
    <span>Size: #=size# bytes</span>
    <span>Extension: #=files[0].extension#</span>
</div>
</script>
```

现在，在初始化文件上传器时，请包含 `template` 属性：

```javascript
$('#fileUpload').kendoUpload({
    template: kendo.template($('#fileListTemplate').html())
});
```

现在，当你选择文件时，你会看到列表中包含名称、大小和扩展名等元数据。

![](img/2a66507445090790d34dc5588b56b24c_105_0.png)

### 异步将文件上传到服务器

在本配方中，我们将了解如何异步将文件上传到服务器。此方法使用 HTML5 File API 将文件上传到服务器。在上传文件时，上传器小部件中会显示文件上传进度。

#### 开始使用

本配方将需要服务器端处理程序来存储或删除文件。

#### 如何操作...

让我们使用相同的表单将文件上传到服务器。要异步上传文件，请指定 `async` 配置：

```javascript
$('#fileUpload').kendoUpload({
    async: {
        'saveUrl': '/fileUploadService/save',
        'removeUrl': '/fileUploadService/remove',
        'autoUpload': true
    }
});
```

这里，async 配置指定了三个属性：saveUrl、removeUrl 和 autoUpload。saveUrl 属性指定将上传文件的服务 URL。removeUrl 属性指定将处理上传文件删除的服务 URL。布尔型 autoUpload 属性指定文件是在用户选择文件时上传，还是在用户点击 Submit 按钮时上传。在本示例中，此设置为 true，这意味着当用户选择文件时，文件将上传到服务器。

当你使用文件上传器小部件选择多个文件时，每个文件都会异步上传。客户端发送带有请求负载中文件数据的 POST 请求。它使用 XmlHttpRequest 对象向 saveUrl 发送异步 POST 请求。

#### 工作原理...

当你使用文件上传器小部件选择多个文件时，会为每个选定的文件向 saveUrl 属性中提到的服务发送 POST 请求，如下图所示：

![](img/2a66507445090790d34dc5588b56b24c_106_0.png)

这里，已选择三个文件，每个文件都在异步上传。在上传文件时，小部件会显示上传过程的进度。文件上传器小部件使用 XmlHttpRequest 对象将文件上传到服务器。随着 HTML5 中 XmlHttpRequest 对象的增强，文件上传器小部件使用进度回调处理程序来跟踪正在上传的文件的进度。

另外，请注意，在上传所选文件时，右上角会显示状态“Uploading”。当列表中的所有文件都上传完毕后，状态将更改为 Done。

![](img/2a66507445090790d34dc5588b56b24c_106_1.png)

上传这些文件后，用户可以通过点击叉号 (x) 按钮来删除它们。这将向 `removeUrl` 属性中提到的服务发送 POST 请求。

如果文件上传出错，上传器小部件会通过将背景颜色标记为红色来指示这一点。它还为你提供了一个重试文件上传的选项。

![](img/2a66507445090790d34dc5588b56b24c_107_0.png)

点击 **retry** 时，会向 `saveUrl` 属性中指定的 URL 发送 POST 请求。

当你尝试在文件上传后从列表中删除文件时，会向 `removeUrl` 中提到的服务 URL 发送 POST 请求。如果你想使用不同的 HTTP 方法来删除上传的文件，请在 `async` 配置中设置 `removeVerb` 属性：

```javascript
$('#fileUpload').kendoUpload({
async: {
'saveUrl': '/fileUploadService/save',
'removeUrl': '/fileUploadService/remove',
'autoUpload': true,
'removeVerb': 'DELETE'
}
});
```

这里，`removeVerb` 属性设置为 `DELETE`，当用户尝试删除文件时，会发送 `DELETE` 请求而不是 POST 请求。

#### 更多...

文件上传器小部件可以本地化，以在小部件的各个位置显示不同的字符串。此外，在现代浏览器中，用户可以将文件拖放到文件上传器小部件上。





### 本地化文件上传器小部件

要本地化文件上传器小部件，请按如下方式指定本地化选项：

```javascript
$('#fileUpload').kendoUpload({
async: {
'saveUrl': '/fileUploadService/save',
'removeUrl': '/fileUploadService/remove',
'autoUpload': true
},
localization: {
select: 'Choose files...',
remove: 'Remove file',
retry: 'Retry upload',
headerStatusUploading: 'Uploading files...',
headerStatusUploaded: 'Complete'
}
});
```

在此，"Select Files..." 标签将被替换为 "Choose files..."；remove 和 retry 的提示将分别为 "Remove file" 和 "Retry upload"。在文件上传期间，将显示 "Uploading files..." 消息，一旦上传完成，将显示 "Complete" 消息。

### 使用拖放上传文件

文件上传器小部件支持将文件拖放到文件上传器小部件上。此功能在所有支持 HTML5 File API 的现代浏览器中均受支持。只有当小部件配置为异步上传文件时，此方法才可用。

当您尝试将文件拖放到上传器小部件上时，它会显示 **drop files here to upload** 消息。

![](img/2a66507445090790d34dc5588b56b24c_108_0.png)

放置文件后，文件将被添加到列表中，并异步上传到服务器。

### 监听文件上传事件

在 HTML5 出现之前，XmlHttpRequest 可用的唯一事件监听器是 `readystatechange` 事件监听器。在 HTML5 中，已向 `XmlHttpRequest` 接口添加了多个事件监听器：loadstart、progress、load、abort、error、timeout 和 loadend。在本示例中，我们将了解可以附加到文件上传器小部件的各种事件监听器以及这些事件执行的顺序。

### 操作方法

文件上传器小部件会显示正在上传的文件的进度，并在所有文件上传完成时显示状态为已完成。但是，在许多情况下，您可能希望附加监听器并监听各种事件：

```javascript
$('#fileUpload').kendoUpload({
    async: {
        'saveUrl': '/fileUploadService/save',
        'removeUrl': '/fileUploadService/remove',
        'autoUpload': true
    },
    cancel: function () {
        console.log('Cancel Event.');
    },
    complete: function () {
        console.log('Complete Event.');
    },
    error: function () {
        console.log('Error uploading file.');
    },
    progress: function (e) {
        console.log('Uploading file ' + e.percentComplete);
    },
    remove: function () {
        console.log('File removed.');
    },
    select: function () {
        console.log('File selected.');
    },
    success: function () {
        console.log('Upload successful.');
    },
    upload: function () {
        console.log('Upload started.');
    }
});
```

在前面的代码片段中，指定了各种事件处理程序：cancel、complete、error、success、progress、remove、select 和 upload。

### 工作原理

当您尝试使用文件上传器小部件上传文件时，各种回调处理程序将按以下顺序执行：

- 1. 当您选择一个文件或多个文件时，将执行 `select` 事件处理程序。
- 2. 当上传器小部件开始上传文件时，将执行 `upload` 事件处理程序。
- 3. 在文件上传期间，`progress` 事件将被连续调用。请注意，事件对象具有一个 `percentComplete` 属性，用于指示上传到服务器的文件大小。
- 4. 文件上传完成后，将执行 `success` 处理程序。
- 5. 如果正在上传多个文件，并且所有选定的文件都已上传，则将执行 `complete` 处理程序。
- 6. 如果用户在文件上传进行期间尝试取消上传，则将执行 `cancel` 事件。
- 7. 文件上传完成后，如果用户尝试通过单击叉号 (x) 图标从上传器小部件中删除已上传的文件，则将执行 `remove` 处理程序。
- 8. 当 `remove` 和 `cancel` 事件执行完成后，也会执行 `complete` 处理程序。
