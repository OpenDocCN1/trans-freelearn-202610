
```
this.sendMessage({
    from:'server',
    to:payload.from,
    content:'The action '+payload.action +' is not supported'});
}
```

我们的服务的实现很简单：我们只需在 action 属性上使用一个 switch 块来决定发送哪条消息。第一个 case 是 roll_a_dice，在这种情况下我们发送一条消息，其内容以 Result is 开头并包含 1 到 6 之间的随机数字。

第二个 case 用于 echo 命令，我们只需发送一条包含接收到的相同内容的消息。

如果 action 属性不满足其他任何 case，我们就进入 default，只发送一条说明命令不受支持的消息。

现在我们将为此实现运行测试：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_262_0.png)

我们的测试现在按预期通过了。

这是最后一个需要实现的服务。因此，现在我们已经实现了所有服务和测试，可以使用以下命令一起执行所有测试：

```
mocha tests/*_test.js
```

我们可以在控制台看到，所有测试都通过了：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_263_0.png)

## Handling WebSocket connections

我们将使用 socket.io 库来处理 WebSocket 连接。这是 Node.js 社区中使用最广泛的 WebSocket 库。

现在，让我们开始 WebSocket 实现。打开你的 index.js 文件并将以下代码放入其中：

```
let Rx = require('rx');
let dataSource = require('./data_source');
let services = require('./services')(dataSource);
```

目前，我们只是导入了响应式扩展、我们的数据源并创建了服务。现在我们需要启动服务器：

```
let app = require('express')();
let http = require('http').Server(app);
let io = require('socket.io')(http);
http.listen(3000, ()=>console.log('Server listening on port 3000'));
```

在这里，我们创建一个 HTTP 服务器，将其传递给 socket.io 库，并使其在 3000 端口监听。现在我们必须等待连接：为此我们创建一个 Observable 来监听连接事件：

```
Rx.Observable.fromEvent(io,'connection')
```

我们为其创建一个订阅：

```
Rx.Observable.fromEvent(io,'connection')
.subscribe(function(client) {
});
```

现在我们必须实现订阅，并在订阅中监听来自该客户端的事件。客户端将以字符串形式发送 JSON，因此我们需要将其解析回 JSON。我们还需要调用正确的服务；每次我们的服务返回一个新的 Observable 时，我们必须切换到该 Observable。最后，当客户端断开连接时，我们必须停止我们的 Observable：

```
function(client) {
    let currentObservable = null;
    Rx.Observable.fromEvent(client, 'request')
        .map((payload)=>JSON.parse(payload))
        .flatMapLatest((payload)=> {
            let serviceObservable = services[payload.service](payload,
currentObservable);
            if(serviceObservable){
                currentObservable = serviceObservable;
            }
            return currentObservable;
        })
        .takeUntil(Rx.Observable.fromEvent(client,'disconnect'))
        .subscribe(data => client.emit('message',data));
}
```

我们需要做的第一件事是创建一个变量来存储当前正在监听的 Observable；我们需要它是因为 blockUser() 服务必须将其作为参数接收。然后我们为客户端的所有类型为 request 的事件创建一个 Observable，并使用 map() 运算符将我们的 payload 映射为 JSON 对象。

现在我们有一个 Observable，它会发出客户端发送的 JSON 对象。下一步是调用相应的服务。由于我们知道我们的服务可能会返回一个 Observable，在这种情况下我们必须使用它，因此我们需要将其存储在 currentObservable 变量中；我们使用 flatMapLatest 而不是 flatMap，因为我们只想保留最后返回的 Observable。

我们还必须在客户端断开连接时结束我们的 Observable，我们使用 takeUntil() 运算符来实现这一点，如果客户端发送 disconnect 事件则结束 Observable。

最后，我们要做的就是将消息发送回我们的用户，我们通过在 observable 中添加一个订阅来实现这一点。

所以下面是我们 index.js 文件的最终代码：

```
let Rx = require('rx');
let dataSource = require('./data_source');
let services = require('./services')(dataSource);
let app = require('express')();
let http = require('http').Server(app);
let io = require('socket.io')(http);
http.listen(3000, ()=>console.log('Server listening on port 3000'));

Rx.Observable.fromEvent(io,'connection')
.subscribe(function(client) {
  let observable = null;
  Rx.Observable.fromEvent(client, 'request')
    .map((payload)=>JSON.parse(payload))
    .flatMapLatest((payload)=> {
      let serviceObservable =
        services[payload.service](payload,observable);
      if(serviceObservable){
        observable = serviceObservable;
      }
      return observable;
    })
    .takeUntil(Rx.Observable.fromEvent(client,'disconnect'))
    .subscribe(data => client.emit('message',data));
});
```

这就是使用 RxJS 库的函数式响应式编程实现网络聊天所需的所有内容。在我们的实现中，一切都在同一个服务器上进行。让我们看看如何添加更多服务器并使它们相互通信。

## Scaling the server

目前，如果我们决定部署多个服务器，我们的实现将无法使用。

想象一下用户 A 连接到服务器 A，用户 B 连接到服务器 B 的场景；用户 A 和用户 B 无法聊天，因为他们在不同的服务器上。

为了使来自不同服务器的连接用户能够通信，我们需要找到一种方法让我们的服务器相互通信，我们可以使用 Redis 数据库来实现这一点。除了其他出色的功能外，Redis 还支持称为发布/订阅模式的功能。有了这种模式，服务器可以发布一条消息，所有订阅以监听该消息的服务器都会收到它。

有了这个功能，Redis 是解决我们服务器通信问题的完美数据库，因此让我们看看如何连接到它以接收消息，以及如何更改我们的数据存储以支持它。

## Connecting to Redis

要使用 Redis 数据库，我们需要一个库来连接它，Node 最常用的库称为 ioredis。该库可在 npm 上获得，并支持 Redis 的所有功能。

要安装 ioredis，只需在项目中执行以下命令：

```
npm install ioredis@2.5.0 --save
```

正如我们在上一节中所讨论的，我们将使用 Redis 作为我们网络聊天实现中所有消息的发布/订阅代理。

在使用 Redis 之前，你需要在机器上安装它，你可以按照官方网站上的说明进行安装。

此图显示了在具有多个 Node.js 服务器的环境中用户发送消息时发生的情况：

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_267_0.png)

此图显示了一个用户（名为 User A），它连接到一台服务器（名为 Server A），该用户随后向 Server A 发送消息，该服务器随后在 Redis 中发布此消息，所有服务器都订阅以监听 Redis 服务器上的消息，然后 Redis 将消息发送给所有服务器中的每一个，最后将消息发送回所有已连接用户。

这种架构是我们如何使多个服务器通信的简单实现，这并不打算成为解决此问题的最佳架构。

## Using Redis as the DataSource for messages

现在我们必须更改我们的 DataSource 以使用 Redis 而不是原始实现中的基本 subject。为此，只需更改 data_source.js 文件，使用以下实现代替：

```
let Rx = require('rx');
let Redis = require('ioredis');

let subscriber = new Redis();
let publisher = new Redis();
const TOPIC = 'web_chat';
subscriber.subscribe(TOPIC);
let messagesObservable =
Rx.Observable.fromEvent(subscriber,'message',(channel,message)=>JSON.parse(
message));
module.exports = {
  push(data){
    publisher.publish(TOPIC,JSON.stringify(data));
  },
  get(){
    return messagesObservable;
  }
};
```

在这段代码中，我们首先导入 RxJS 和 ioredis 库；然后我们与 Redis 服务器建立两个连接，一个用于发布，另一个用于监听消息。由于我们在本地机器上运行 Redis，因此无需向构造函数传递任何参数。在下一行，我们为消息将被发布到的 TOPIC 创建一个名称。此名称可以是任何内容，但所有服务器必须使用相同的 TOPIC。你也不需要手动创建此 TOPIC，因为 Redis 会自动为你创建。我们最后订阅此 TOPIC 并创建一个 Observable 来监听该订阅的消息。我们需要做的最后一件事是实现导出的方法（push() 和 get()）：在 push() 中我们只需将其发布到 Redis，在 get() 中我们只需返回创建的 Observable。

如果我们想使用 Redis 在它们之间通信来将我们的应用程序分发到多个服务器上，这就是我们需要做的全部内容。
