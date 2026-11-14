

## 文件上传基础示例

```
function uploadError(err) {
    console.log(`Encountered an upload error: ${JSON.stringify(err, null, 2)}`);
}

function uploadSuccess(r) {
    console.log(`Upload completed successfully: ${JSON.stringify(r, null, 2)}`);
}

function doUpload() {
    let fileToUpload = 
                    "cdvfile://localhost/persistent/index.html";
    let uploadURL = encodeURI(
                            "https://your.server.com/upload.php");

    let options = new FileUploadOptions();
    options.fileKey = "file";
    options.fileName = "index.html";
    options.mimeType = "text/html";

    let fileTransfer = new FileTransfer();
    fileTransfer.upload(fileToUpload, uploadURL, uploadSuccess, 
                        uploadError, options);
}
```

这里指定的 `options` 是最低要求。我们必须指定文件键、文件名和文件类型。后两者更接近建议：我们的脚本无论如何都会重命名文件（以确保没有命名冲突），也不假设我们关于 MIME 类型的陈述是真实的。

注意我们为 `options.fileKey` 指定了文件。这与我们设想的输入元素上的 `name="file"` 属性匹配，也是 PHP 脚本用来索引 `$_FILES` 的方式。如果这不正确，上传将会失败。

如果上传遇到错误，将调用 `uploadError`。它将具有与传递给 `downloadError` 的错误对象相同的结构。

如果上传成功完成，将调用 `uploadSuccess`，并传入一个包含上传数据的对象。你不必对它做太多处理；它只是让你检查服务器发送的响应。在 Android 上，你还可以检查发送的字节数，但 iOS 不支持此功能，因此你无法用它来判断浏览器是否发送了你预期的字节数。

你可以指定其他有用的选项：

*   `httpMethod`：更改上传文件所用的方法。默认为 `POST`，但如果需要可以改为 `PUT`。
*   `chunkedMode`：默认情况下，文件分块上传，这允许原生代码优化文件传输方式（尤其是大文件）。如果需要禁用分块上传，将其设为 `false`。
*   `headers`：你可能需要发送认证信息或其他头部。可以通过在 `headers` 对象中发送该信息来实现。

下面是一个稍微复杂的示例，使用了前面提到的选项：

```
let options = new FileUploadOptions();
options.fileKey = "file";
options.fileName = "index.html";
options.mimeType = "text/html";
options.chunkedMode = false;
options.httpMethod = "PUT";
options.headers = {
    "Authorization": `Basic ${btoa(`${username}:${password}`)}`
}

let fileTransfer = new FileTransfer();
fileTransfer.upload(fileToUpload, uploadURL, uploadSuccess, 
                    uploadError, options);
```

### 注意

从技术上讲，`upload` 支持 `trustAllHosts` 作为第六个参数。其工作方式与下载时的 `trustAllHosts` 相同，但除非你真的必须接受自签名证书，否则应避免将其设为 `true`。在生产代码中绝不应这样做。

## 上传/下载进度监听

只要下载或上传的是小文件，用户不太可能注意到操作耗时。但一旦传输中等大小的文件，耗时就会变得明显。因此，你需要一种方式向用户传达进度。

*如何* 实现完全取决于你。你可以只显示一个加载指示器，也可以显示某种进度条。但在此之前，我们必须监听操作的进度更新。

### 注意

本节代码片段位于代码包的 `snippets/09/ex3-progress`。使用交互式片段演练场时，选择 **9: Transferring Files** 和示例 **3**。

注册接收进度更新非常简单。创建 `FileTransfer` 实例后，可以为 `onprogress` 属性分配一个处理程序，如下所示：

```
function reportProgress(progressEvent) {
  // 我们将在下面添加此函数的内容。
}

let fileTransfer = new FileTransfer();
fileTransfer.onprogress = reportProgress;
```

`reportProgress` 接收的 `progressEvent` 有一些有用的属性：

*   `lengthComputable`：如果为 `true`，则可以计算操作的长度。并非所有服务器都返回下载大小，所以这不总是可用。
*   `total`：如果可以计算长度，这是操作的总大小。
*   `loaded`：已下载（或上传）的数据量。

如果 `lengthComputable` 为 true，则可以通过用 `loaded` 除以 `total` 来确定操作完成的百分比：

```
let pctComplete;
if (progressEvent.lengthComputable) {
    pctComplete = progressEvent.loaded / progressEvent.total;
}
```

### 注意

此时，`pctComplete` 是 0 到 1 之间的数字。如果想要典型的 0-100 范围，可以乘以 100。

假设 DOM 中有一个 `progress` 元素，在现代设备上会渲染进度条。我们可以按如下方式更新它：

```
if (progressEvent.lengthComputable) {
    document.getElementById("progressBar").setAttribute("value",
        (progressEvent.loaded / progressEvent.total));
}
```

如果无法计算操作的完成百分比，由你决定做什么。你可以显示一个不确定进度条（虽然 iOS 7 不支持 `progress` 元素的不确定性）。你可以显示额外信息告诉用户已下载了多少数据；虽然这可能给不了他们太多信息，因为他们也不太可能知道应该传输多少数据。总之，目标是让他们确信操作正在进行，即使不知道还要多久。你可以随时间缓慢增加进度条（Web 浏览器等待服务器响应时常这样做），或显示安抚性消息，如“请稍候——我们仍在下载数据”。

但你不能保证一定会收到进度更新，所以不要围绕它构建关键逻辑。如果文件已在浏览器缓存中、传输极快或发生错误，可能没有时间发送进度事件。

## 终止文件传输

只要可能，用户都应能终止文件传输，即使你不认为传输会花很长时间。你不知道他们的网络状况，即使是很短的文件，在网络条件极差时也可能比任何人预期的都要长。话虽如此，你不必立即提供选项；你可能想等待几秒钟，给传输留出完成时间，然后再提供中止操作的方法，以确保用户不会不小心按下屏幕上出现的任何按钮。

中止传输很简单：`FileTransfer` 的任何实例都有一个 `abort` 方法，它将停止操作并调用错误处理程序。错误代码始终为 `FileTransferError.ABORT_ERR`，因此你的错误处理程序可以正确通知应用的其他部分关于终止的情况。

### 注意

本节代码片段位于代码包的 `snippets/09/ex4-abort`。使用交互式片段演练场时，选择 **9: Transferring Files** 和示例 **4**。

下面是一个示例。在这种情况下，DOM 中有 `<button id="cancelDownload">`。点击时，传输被中止：

