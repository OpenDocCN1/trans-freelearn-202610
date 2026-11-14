
# Cordova 白名单配置与文件传输插件

## 白名单配置示例

```xml
<access origin="*" />
<allow-intent href="http://*/*" />
<allow-intent href="https://*/*" />
<allow-intent href="tel:*" />
<allow-intent href="sms:*" />
<allow-intent href="mailto:*" />
<allow-intent href="geo:*" />
<platform name="android">
  <allow-intent href="market:*" />
</platform>
<platform name="ios">
  <allow-intent href="itms:*" />
  <allow-intent href="itms-apps:*" />
</platform>
```

## 理解白名单插件

这里实际上有很多内容，特别是如果你不熟悉新的白名单插件的话。让我们来看看其中一些有趣的部分：

*   **`<access>`** 指定我们的应用被允许与之通信的主机。可以有任意数量的此类标签，因此你可以尽可能具体地指定。通配符也可用；在本例中，`*` 允许与所有主机通信。

*   **`<allow-intent>`** 指定你的应用被允许触发的意图。意图是一个 URL，它会在用户设备上安装的另一个应用中打开一个 URL。例如，`http(s)://` 会在用户的网页浏览器中打开，而 `tel://` 会在可用时打开电话拨号器。本节列出的意图相当通用，涵盖了广泛的用例。如果你的应用不需要特定的意图，你应该将其移除。

*   **`<platform>`** 允许我们指示某些设置仅适用于特定平台。例如，`market://` URL 方案仅在 Android 设备上有效，而 `itms://` 和 `itms-apps://` URL 方案仅在 iOS 设备上有效（这些会打开应用商店）。

## 配置对特定主机的访问

在我们的例子中，假设我们需要通信的服务器是 `www.example.com`。我们可以使用 `<access>` 标签：

```xml
<access origin="http://www.example.com" />
```

### 注意：安全 HTTP

我们的示例假设通过 HTTP 进行数据传输，这并不安全。如果你想使用安全的 HTTP 连接，应该改为指定 `https://`。

如果你可能需要访问该主机的任何子域名，可以按如下方式使用通配符：

```xml
<access origin="http://*.example.com" />
```

## 白名单的其他功能

白名单还控制着其他几个功能，你可能会感兴趣（取决于你的需求）。更多信息请参阅 [https://github.com/apache/cordova-plugin-whitelist#network-request-whitelis](https://github.com/apache/cordova-plugin-whitelist#network-request-whitelis)。

## 文件传输插件与 XHR

一直以来都可以利用 `XMLHttpRequest` 从外部服务器发送和接收内容，这意味着*从技术上讲*你可以完全弃用文件传输插件而改用 XHR，即使对于大文件也是如此。

虽然可行，但由于以下几个原因，这并不理想：

*   你会受到跨域限制的约束，这可能意味着需要为目标服务器添加 `CORS` 支持。
*   接收到的数据实际上并未存储在任何地方；你仍需负责将其写入文件。
*   除非你使用抽象层，否则需要编写大量样板代码。

另一方面，文件传输插件具有以下优势：

*   它无需你编写额外代码即可从原生文件系统读取和写入。
*   它不受跨域限制约束；请求由原生代码发起。
*   操作可以在应用进入后台时完成（注意 iOS 对后台操作持续时间有限制：大约十分钟）。

### 何时使用哪种

那么，何时应该使用文件传输插件而不是 XHR？这实际上取决于你打算如何处理数据。

*   **使用 XHR** 如果你要下载一个大型 JSON 文件并打算立即解析：你会立即获得数据，而不必编写代码去从文件系统读取文件。
*   **使用文件传输插件** 如果文件是 SQLite 文件之类。虽然你可以使用 XHR 将响应以二进制格式写入文件系统，但使用文件传输插件要容易得多，也更不容易出错。

简而言之，如果你希望文件以*文件形式*存在于用户设备上，请使用文件传输插件。如果你只需要解析响应中的数据，请使用 XHR。

上传同理：
*   如果你上传的数据是代码中动态创建的，可以使用 XHR 将此数据提交给服务器。
*   如果你需要传输的数据位于文件系统中的文件里，请改用文件传输插件。同样，你可以使用 File 插件读取文件内容然后通过 XHR 发送，但文件传输插件更简单且不易出错。

### 协议限制

重要的是要注意，文件传输插件仅使用 HTTP(S) 传输数据。也就是说，该插件不会尝试进行 FTP 或 SFTP 文件传输（或任何其他协议）。有些插件支持此功能，但超出了本章范围。

## 安装文件传输插件

在使用文件传输插件之前，我们需要先安装它。

### 使用 Cordova CLI

```bash
cordova plugin add cordova-plugin-file-transfer
```

### 使用 `package.json`

```json
...,
"plugins": {
    ...,
    "cordova-plugin-file-transfer"
}, ...
```

然后，我们需要执行 `gulp init` 以使用新插件重新创建 Cordova 项目。

### 注意：代码片段

本节的代码片段位于本书代码包中的 `snippets/09/ex1-downloading-files`。使用交互式片段演练场时，选择 **9: Transferring Files** 和示例 **1**。

## 使用文件传输插件

该插件提供了一个 `FileTransfer` 类，封装了下载和上传功能。在下载或上传任何文件之前，我们必须创建该类的一个新实例：

```javascript
let fileTransfer = new FileTransfer();
```

### 注意：`deviceready` 事件

请记住，直到 `deviceready` 事件触发之前，你无法真正使用 `FileTransfer`。

获得实例后，你可以调用它提供的 `download` 方法。它接受几个参数：

*   **`url`**：你想要下载的文件的 URL。你应该使用 `encodeURI` 确保特殊字符被正确编码，否则可能会遇到错误。
*   **`downloadLocation`**：这是你想要保存文件的位置。这应该是一个 `cdvfile://` 位置，但也可以是绝对路径。
    *   如果文件仅临时保存，请保存到 `cdvfile://localhost/temporary`。
    *   如果文件永久保存，请保存到 `cdvfile://localhost/persistent`。注意，后者在 iOS 上可能会被 iCloud 备份。
*   **`downloadSuccess`**：文件下载成功时调用此函数。它会传递文件存储位置和生成的 HTTP 响应信息。
*   **`downloadError`**：下载过程中发生错误时调用此函数。它会传递一个错误对象，指示下载失败的原因（如果可用）。
*   **`trustAllHosts`**：当为 `true` 时禁用 SSL 证书检查。默认为 `false`，除非有充分理由，否则应避免更改此设置。在生产代码中绝对不要使用 `true`。
*   **`options`**：这允许你指定额外选项，例如授权头。

### 下载示例

```javascript
function downloadError(err) {
  console.log(`Encountered a download error: ${JSON.stringify(err, null, 2)}`);
}

function downloadSuccess(entry) {
  console.log(`Download completed successfully: ${JSON.stringify(entry, null, 2)}`);
}

function go() {
    let assetURL = encodeURI("http://kerrishotts.github.io/Mastering-PhoneGap-Code-Package/index.html");
    let downloadLocation = "cdvfile://localhost/persistent/index.html";

    let fileTransfer = new FileTransfer();
    fileTransfer.download(assetURL, downloadLocation, downloadSuccess, downloadError);
}

document.addEventListener("deviceready", go);
```
