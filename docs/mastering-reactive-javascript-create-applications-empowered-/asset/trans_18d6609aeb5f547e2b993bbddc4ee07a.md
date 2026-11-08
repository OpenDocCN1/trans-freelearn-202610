
之后你应该有如下的文件夹结构：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_275_0.png)

现在我们准备第一次构建我们的应用程序。

## 第一次构建应用程序

为了创建我们的应用程序，我们必须能够提供至少两个静态文件，index.html，即 HTML 页面，以及 bundle.js 文件，其中包含用 JavaScript 编写的应用程序的所有逻辑。

使用 express 提供静态文件极其简单。为此，让我们修改 src 文件夹内的 index.js 文件（src/index.js），我们在其中实现了 express 服务器，并粘贴以下代码：

```
app.use(express.static('static'));
```

这段代码告诉 express 将 static 文件夹中的所有文件作为静态文件提供；现在我们将能够第一次看到我们的页面。为此，使用以下命令运行你的应用程序：

```
node src/index.js
```

不要忘记启动 Redis 服务器，因为我们使用它来通信消息。如果不启动 Redis 服务器，你会看到类似这样的错误：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_276_0.png)

所以，不要忘记运行 Redis 服务器。

启动 Redis 服务器并运行我们的 node 应用程序后，我们就可以在浏览器中第一次看到 `web chat` 应用程序了，因此在浏览器中输入 `http://localhost:3000`，你将看到应用程序正在运行：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_276_1.png)

如果你能看到此屏幕，恭喜！我们离实现我们的 web chat 应用程序又近了一步。现在我们能够为我们的应用程序提供静态文件，我们必须找到一种方法来构建 JavaScript 文件；为此我们需要使用 browserify。

browserify 接收一个文件作为参数，并将跟随并添加该文件的所有依赖项（require），直到将所有内容打包到单个文件中。此文件将包含运行我们的 web chat 应用程序所需的所有代码。由于我们已经安装了 browserify，我们要做的就是使用以下命令生成文件：

```
browserify client/index.js -o static/bundle.js
```

此命令告诉 browserify 从 client 文件夹中的 index.js 文件开始，并收集所有依赖项以在 static 文件夹内生成名为 bundle.js 的文件。

运行此代码后，你应该在 static 文件夹内看到生成的 bundle.js 文件。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_277_0.png)

现在我们将准备 package.json 文件，以便在启动应用程序之前始终测试和构建应用程序。如果你不知道如何确切地更改 package.json 文件也不用担心，因为我会粘贴它的最终结果。

让我们修改项目根目录中的 package.json 文件，以便使用它来构建我们的应用程序。首先，让我们添加服务器的测试。正如你可能记得的——要测试我们的服务器应用程序，我们必须运行以下命令：

```
mocha tests/*test.js
```

因此我们必须将此命令添加到 package.json 中 scripts 属性里的 test 属性中；现在我们可以使用以下命令运行测试：

```
npm test
```

要运行我们的项目，我们使用以下命令：

```
node src/index.js
```

因此，我们可以将此命令添加到 package.json 文件中 scripts 属性里的 start 属性中；现在我们可以使用以下命令启动我们的应用程序：

```
npm start
```

现在让我们在 package.json 上添加一个构建客户端应用程序的命令；让我们在 package.json 文件的 scripts 属性中添加 build-client 属性，包含以下命令：

```
browserify client/index.js -o static/bundle.js
```

现在我们可以使用以下命令构建客户端应用程序：

```
npm run build-client
```

最后，让我们使我们的项目在启动时始终构建和测试。为此，在 scripts 属性中添加 prestart 属性，包含以下命令，以构建和测试我们的应用程序：

```
npm run build-client && npm test
```

如果你正确遵循这些说明，你最终应该得到如下的 package.json 文件：

```
{
  "name": "web_chat",
  "version": "1.0.0",
  "description": "Web chat application",
  "main": "src/index.js",
  "scripts": {
    "prestart": "npm run build-client && npm test",
    "start": "node src/index.js",
    "test": "mocha tests/*test.js",
    "build-client": "browserify client/index.js -o static/bundle.js"
  },
  "author": "Erich Oliveira",
  "license": "ISC",
  "dependencies": {
    "express": "^4.15.2",
    "ioredis": "^2.5.0",
    "rx": "^4.1.0",
    "socket.io": "^1.7.3",
    "socket.io-client": "^1.7.3"
  },
  "devDependencies": {
    "browserify": "^14.1.0",
    "mocha": "^3.2.0"
  }
}
```

现在我们可以使用以下命令运行我们的应用程序：

```
npm start
```

你将在控制台中看到以下输出，显示你的应用程序已打包、测试通过，并且应用程序正在运行：

```
bash-3.2$ npm start
> web_chat@1.0.0 prestart /Users/ericholiveira/dev/workspace/web_chat
> npm run build-client && npm test

> web_chat@1.0.0 build-client /Users/ericholiveira/dev/workspace/web_chat
> browserify client/index.js -o static/bundle.js

> web_chat@1.0.0 test /Users/ericholiveira/dev/workspace/web_chat
> mocha tests/*test.js

Block user Test
  ✓ Must not receive message from blocked user

Command Test
  ✓ Must support roll_a_dice command
  ✓ Must support echo command
  ✓ Must send a message if the action is not supported

Listen to message Test
  ✓ Must listen to messages sent to me
  ✓ Must listen to messages sent to all
  ✓ Must not listen to messages sent to others

Send Message Test
  ✓ Must support sending a message
  ✓ Must support broadcast message

9 passing (34ms)

> web_chat@1.0.0 start /Users/ericholiveira/dev/workspace/web_chat
> node src/index.js

Server listening on port 3000
```

现在我们已经完成了应用程序的构建，我们准备实现客户端，并将从与服务器的通信开始。

## 连接到服务器应用程序

我们将使用 WebSockets 通过 socket.io-client 连接到服务器。在服务器上我们将连接和请求建模为 observable 对象，我们将在客户端也这样做。为此，打开 client 文件夹内的 connection.js 文件，并导入我们将要使用的模块：

```
var Rx = require('rx');
var io = require('socket.io-client');
```

到目前为止，我们只导入了 rx 模块，我们将使用它来创建 observables，以及 socket.io-client，我们将使用它来启动连接。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_280_0.png)

我们不会在客户端使用 ES6，以避免将其转译为 ES5 的额外步骤并使其在所有主要浏览器上工作。

现在我们需要连接到服务器。由于我们在 3000 端口运行，我们可以使用以下代码实现：

```
var socket = io('http://localhost:3000');
```

我们只能在连接建立后开始发送消息；因此，我们需要 observables 来监听 connect 和 disconnect 事件：

```
var connectionObservable = Rx.Observable.fromEvent(socket,'connect');
var disconnectObservable = Rx.Observable.fromEvent(socket,'disconnect');
```

在此文件中我们需要一种向服务器发送数据并从服务器监听数据的方式；因此我们将导出两个函数。

第一个公开的函数用于向服务器发送数据，我们将它命名为 addSender；它的签名如下：

```
addSender(observable);
```

它只接收一个必填参数：

- **observable**: An `observable` object whose events are going to be propagated to the server

它的实现如下：

```
function(observable){
    connectionObservable
        .flatMap(observable)
        .takeUntil(disconnectObservable)
        .map(function(data){
            return JSON.stringify(data);
        })
        .subscribe(function(data){
            socket.emit('request',data);
        });
}
```

实现相当直接；由于我们只能在连接建立后发送消息，我们将从 `connectionObservable` 开始，并使用 `flatMap()` 操作符，这样在连接建立后它将开始传播我们想要发送到服务器的数据。

我们还需要停止向服务器发送消息如果 socket 断开连接，因此我们添加 `takeUntil()` 操作符以在断开连接时停止 observable。

我们将以 JSON 字符串的形式向服务器发送数据；因此我们需要使用 `JSON.stringify` 函数将每个数据映射为 JSON 字符串。

在最后一步，我们订阅创建的 observable 并使用 `socket.emit` 向服务器发送数据。

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_281_0.png)

为了保持代码和示例简单，我们的应用程序中不会处理重连。

我们需要公开的另一个方法必须用于监听来自服务器的传入消息；我们将它命名为 `listen`，它的签名如下：

```
listen(event);
```
