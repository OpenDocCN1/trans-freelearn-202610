

这个例子非常简单：它从 GitHub 下载一个文件（实际上是本书代码包的 HTML 页面），并将其保存到设备的本地文件系统中，文件名为 `index.html`。文件名可以是你喜欢的任何名称；它不必与原始名称匹配（尽管你应该努力保持相同的扩展名）。此外，它可以保存到子目录中；如果该目录不存在，将会自动创建。

如果发生错误，将调用 `downloadError` 函数。我们的错误处理非常简单；它只是记录 `error` 对象。在生产环境中，你需要做更多的工作来弄清楚问题所在，以及是否有办法绕过它（或向用户显示友好的错误消息）。传入的 `err` 对象包含以下可能对你有用的信息：

*   `code` 表示错误代码。它可以是以下之一：

    *   `FileTransferError.FILE_NOT_FOUND_ERR`：无法在该下载位置保存文件。
    *   `FileTransferError.INVALID_URL_ERR`：源 URL 无效（你是否忘记使用 `encodeURI`？）。
    *   `FileTransferError.CONNECTION_ERR`：无法建立连接。有网络连接吗？主机可达吗？如果你使用 SSL，请确保证书未过期，且由受信任的证书颁发机构签名。
    *   `FileTransferError.ABORT_ERR`：传输被应用中止。
    *   `FileTransferError.NOT_MODIFIED_ERR`：自上次请求以来源文件未被修改。如果请求返回 HTTP 304 代码，就会发生这种情况。

*   `source` 是插件尝试下载的源 URL。
*   `target` 是下载目标位置。
*   `http_status` 表示来自服务器的响应代码。这可能是各种代码。但通常以下是典型代码：

    *   `200` (`OK`)：请求成功完成。此时，错误发生在保存文件时，而非服务器端。
    *   `304` (`Not Modified`)：请求成功完成，但自上次请求以来文档未更改。
    *   `401` (`Not Authorized`)：请求未能完成，可能是因为你未通过服务器认证。
    *   `403` (`Forbidden`)：请求失败，因为你无权访问该资源。你可能需要请求用户提供凭据。
    *   `404` (`Not Found`)：请求失败，因为未找到资源。
    *   `500` (`Internal Server Error`)：服务器端出现严重错误。错误消息可能包含额外的有用信息，也可能不包含。

*   `body` 是来自服务器的响应。如果你收到的状态码不是 200，你可能想在这里检查更多信息。

正如你所见，这里有很多错误处理的机会。至少，你会想向用户显示一条无法下载文件的通知；但你可以利用传入的 `error` 对象中提供的信息，为用户定制消息。

如果下载成功完成，将调用 `downloadSuccess` 函数。同样，我们的处理程序非常简单；它只是记录 `file` 条目（表示文件在文件系统中的存储位置）。在生产应用中，此时你将继续处理该文件；调用此方法时，你可以确信文件已存在于文件系统中。如果你需要使用 File API 进行额外处理，可以通过传入的 `file` 条目来实现。

如果你需要为受保护的资源提供凭据，可以按如下方式操作：

```javascript
let fileTransfer = new FileTransfer();
let username = "testuser";
let password = "password";
let options = {
    headers: {
        "Authorization": `Basic ${btoa(`${username}:${password}`)}`
    }
}
fileTransfer.download(assetURL, downloadLocation, downloadSuccess, downloadError, false, options);
```

### 提示

如果你要发送凭据，请确保使用 SSL 来保护通信安全。

## 上传文件

在将文件上传到服务器之前，我们需要服务器上有能够接收和处理文件的程序。至少，此过程会将文件写入服务器上的特定位置，但通常还有更多工作要做。通常，会检查请求以确保没有可疑行为（例如文件过大或类型错误）。然后，文件几乎总是会被重命名以防止命名冲突。之后，就由你决定了：你可以对文件运行病毒扫描、调整大小（如果是图片）、上传到后端数据库、发送到另一台服务器，等等。

### 注意

本节的代码片段位于本书代码包的 `snippets/09/ex2-uploading-files` 中。使用交互式代码片段演练场时，请选择 **9: Transferring Files** 和示例 **2**。

我们迄今为止使用的 File Transfer 插件使用 multipart HTTP `POST` 请求上传数据，这非常有用。这意味着我们可以使用与从网站用户请求文件相同的方法，这通常涉及 HTML 表单上的 `<input type="file" />` 控件和一个处理结果的 PHP 脚本。在我们的例子中，我们不需要 HTML 表单；是我们的应用在执行上传，而不是用户，所以我们可以直接调用 PHP 脚本。

### 注意

如果你已经有了这样的系统，可以跳过本节。另外请注意，虽然本节使用 PHP，但这些概念适用于任何服务端语言。

让我们开始构建 PHP 脚本。我们将其命名为 `upload.php`。在顶部，我们将放置稍后要用到的一些配置设置：

```php
<?php
$maximumFileSize = 5 * 1024 * 1024;
$uploadLocation = "../upload/";
$validMimeTypes = array("text/plain", "text/html", "image/png", "image/jpeg");
```

虽然你可以在 `php.ini` 设置中限制服务器上的最大文件大小，但最好不要假设这些设置就是你想要的。因此，在上传脚本中检查文件大小也是个好主意。但要记住，这里在 `$maximumFileSize` 中指定的任何大小都不会覆盖你的 `php.ini` 设置，所以你需要确保 `post_max_size` 和 `upload_max_filesize` 设置足够大，能够接受 `$maximumFileSize` 字节的文件。

上传位置完全取决于你和你的服务器配置。有效的 MIME 类型也由你决定；这些将允许你过滤掉非预期格式的文件。当然，既然我们的应用将是上传文件的客户端，理想情况下应用永远不会发送错误类型的文件，但多检查一下总无妨。此外，你的应用可能不是上传脚本的唯一客户端，所以你需要处理应用可能不是发送数据的一方的情况。

PHP 自动将有关正在上传文件的信息放入一个名为 `$_FILES` 的全局变量中。我们可以使用它来提取所需信息：

```php
$uploadedFile = $_FILES['file'];
$uploadedFileError = $uploadedFile['error'];
$uploadedFileSize = $uploadedFile['size'];
$uploadedFileType = $uploadedFile['type'];
$uploadedFilePath = $uploadedFile['tmp_name'];
```

一个重要的注意事项是，`$_FILES` 上的 `file` 索引是文件键。如果你有一个 HTML 表单，它将匹配你的文件输入元素的 `name`。我们将其称为“文件键”：

```html
<form enctype="multipart/form-data" method="POST">
    <input type="file" name="file" />
</form>
```

接下来，我们将用 `try…catch` 语句包装我们所做的一切。如果出现问题，这将让我们返回适当的错误（否则我们可能会在错误条件下返回 200）：

