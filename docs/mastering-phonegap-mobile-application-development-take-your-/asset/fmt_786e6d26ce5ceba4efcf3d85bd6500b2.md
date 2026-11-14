
```
try {
    // everything else we do in this section will be in this
    // try section
} catch (RuntimeException $err) {
    // generate a 500 server error
    http_response_code(500);
    // and give some error information
    echo $err->getMessage();
}
?>
```

所有这些代码的作用是确保遇到错误时返回 500 错误代码。我们还会返回错误原因，以便客户端理解上传请求失败的原因。

有了这个基础，我们需要开始检查错误条件。拒绝文件上传有几个原因：

* 请求畸形或损坏
* 未提供文件或提供了过多文件（此脚本一次只处理一个文件）
* 文件过大
* 上传耗时过长
* 文件类型错误
* 服务器保存文件时遇到问题（空间不足、上传路径无效等）

### 注意

本节错误检查代码基于 [http://php.net/manual/en/features.file-upload.php#114004](http://php.net/manual/en/features.file-upload.php#114004)。

考虑到这一点，让我们开始检查这些错误条件。我们可以按如下方式检查请求是否畸形或提供了过多文件：

```
if(!isset($uploadedFileError) || is_array($uploadedFileError)) {
    throw new RuntimeException("Invalid arguments");
}
```

在 PHP 中，所有文件上传都会设置错误条件，*即使文件已成功上传*。我们可以检查该条件是否存在，以验证是否有人试图发送畸形或损坏的请求。如果错误条件不存在，我们可以立即停止处理。如果有多个错误条件（是一个数组），则说明发送了多个文件；我们也可以报错。

接下来，我们检查错误条件本身。它会告诉我们文件是否上传正确、是否未发送文件，或文件是否大于服务器设置允许的大小：

```
switch($uploadedFileError) {
    case UPLOAD_ERR_OK:
        break;
    case UPLOAD_ERR_NO_FILE:
        throw new RuntimeException("No file sent");
        break;
    case UPLOAD_ERR_INI_SIZE:
    case UPLOAD_ERR_FORM_SIZE:
        throw new RuntimeException("File too large");
        break;
    default:
        throw new RuntimeException("Unknown error");
}
```

注意，除了错误为 `UPLOAD_ERR_OK` 的情况外，每个 `case` 都会抛出 `RuntimeException`。后一种情况意味着服务器正确接收了文件，我们可以继续处理。对于任何其他情况，我们都应立即停止并报错。

仅因为我们已根据服务器设置检查了文件大小，并不意味着它不大于我们实际想要接受的大小。此代码片段将上传文件的大小与 `$maximumFileSize` 进行对比：

```
if ($uploadedFileSize > $maximumFileSize) {
    throw new RuntimeException("File too large");
}
```

除非你运行的服务器可以接受所有类型的文件，否则你很可能想要检查文件类型。我们可以只检查 `$uploadedFileType`，但这是客户端发送的信息，可能不准确或不正确。相反，我们还应根据文件系统的信息检查文件类型。此代码片段正是如此：

```
$finfo = finfo_open(FILEINFO_MIME_TYPE);
$actualFileType = finfo_file($finfo, $uploadedFilePath);

if (!in_array($uploadedFileType, $validMimeTypes) ||
    !in_array($actualFileType, $validMimeTypes)) {
    throw new RuntimeException("File format isn't valid");
}
```

前面的代码片段将检查客户端指定的文件类型和服务器认为的文件类型是否在 `$validMimeTypes` 列表中。如果不在，脚本将报错并拒绝保存文件。

这满足了前面提到的大多数错误条件。现在，是时候将文件保存到所需的上传位置了。但首先，我们应该重命名文件使其具有唯一名称，否则可能会覆盖他人上传的文件：

```
$newFileName = "upload_" . uniqid();
```

`uniqid` 是 PHP 中的一个函数，根据当前时间返回字符串。这意味着名称实际上可能不唯一；但假设服务器时间从不更改，这对我们的目的已足够。当然，你可以使用任何重命名方案（关于 `uniqid` 的唯一性详见 [http://stackoverflow.com/questions/4070110/how-unique-is-uniqid](http://stackoverflow.com/questions/4070110/how-unique-is-uniqid)）。

现在我们有了唯一的文件名，可以尝试保存文件：

```
if (move_uploaded_file($uploadedFilePath, $uploadLocation . $newFileName)) {
    echo "File uploaded successfully!";
} else {
    throw new RuntimeException("File failed to upload (" . error_get_last() . ")");
}
```

敏锐的读者会注意到，从技术上讲，如果我们执行到这一步，服务器已经接受了文件；它只是位于 `$uploadedFilePath` 指示的临时位置。`move_uploaded_file` 是 PHP 函数，只是将文件从临时位置移动到更永久的位置。

如果移动失败，我们将抛出另一个错误及失败原因。此时可能是严重问题。也许接受上传的卷空间不足，或我们尝试存储文件的位置不存在。

如果移动成功，我们将返回成功消息。PHP 默认也会返回 200 状态代码。

仅有脚本还不够。你需要一台能运行 PHP 脚本的服务器。这超出了本书范围，但如果你想要一个非常简单的测试环境来测试 PHP 脚本，可以使用 Cloud9 ([https://c9.io](https://c9.io))，它提供云端完整开发环境并支持 PHP。事实上，你可以在我的公共工作区查看此示例（假设你有 Cloud9 账户）：[https://ide.c9.io/kerrishotts/mastering-phonegap-ch8-server](https://ide.c9.io/kerrishotts/mastering-phonegap-ch8-server)。Cloud9 不适用于持续运行的服务器，但非常适合开发用途，特别是公共项目可免费使用（最多一个私有项目也免费）。

现在我们有了一个在服务器上等待上传内容的脚本，可以使用 File Transfer 插件进行上传。正如你所料，我们将调用 `upload` 而不是 `download`。这需要设置一些选项，所以不像调用 `download` 那么简单。

`upload` 接受以下参数：

* `fileToUpload`：指示存储在本地设备上要上传的文件。使用 `cdvfile://` 文件路径。
* `uploadURL`：上传脚本的 URL。应通过 `encodeURI` 传递。
* `uploadSuccess`：上传成功时调用。
* `uploadError`：上传未成功完成时调用。
* `options`：至少指定文件键、名称和 MIME 类型。还可提供参数、标头和其他有用设置。这是 `FileUploadOptions` 的实例。

### 注意

本节代码片段位于代码包的 `snippets/09/ex2-uploading-files` 中。使用交互式代码片段演练场时，选择 **9: Transferring Files** 和示例 **2**。

让我们看一个示例：
