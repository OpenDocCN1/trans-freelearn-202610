
之前，我们的代码运行的是 `App.Contacts.remove(id)`，其中 `id` 是集合 ID。这是 `Backbone` 自动添加到 `collection` 中每个 `model` 上的，用于在内部进行跟踪。但现在我们已更新应用程序以使用真实的 ID 值，情况变得稍微复杂一些。

因为我们希望 `Contact` 的删除操作能持久化到服务器，所以我们需要使用 `model.destroy()`，而不是 `collection.remove()`。首先，我们尝试从 `collection` 中获取 `contact` 并将其赋值给变量 `contact`。用户可能会尝试删除一个不存在的 `contact`，这会导致错误。然后我们检查 `model` 是否返回，如果返回了，我们就运行 `destroy()`。这会向服务器发送 `DELETE` 命令。

现在让我们看看 `scripts/views/contact.js`：

```
  remove: function() {
    App.Browser.navigate('contacts/remove/' + this.model.get('id'));
    this.model.destroy();
  }
```

正如上一节所述，我们现在想要使用 `models` 的真实 ID，而不是 `Collection` ID。我们不再获取 `model.cid`，而是使用 `model.get('id')` 并将其传递给 URL 供 `router` 处理。

最后让我们看看 `scripts/views/directory.js`

```
      newContact.save();
```

这行代码明确告诉 `Backbone` 获取这个新创建的 `contact` 并将其保存到服务器。遗憾的是，仅将一个 `model` 添加到 `Collection` 中并不足以让 `Backbone` 将该 `collection` 持久化到服务器。不过，这确实给了我们对应用程序更多的控制权。

如果你检查此请求的网络流量，你会看到它在服务器上执行 `POST` 操作。然而，它也会响应一个完整的文档。`Backbone` 随后解析此文档，查找 `id` 属性，然后更新本地 `model`。通常，我们希望数据库为我们生成 `id` 值，而 `Backbone` 正好满足了这一需求。

## 注意

如果你终止 `Node` 应用程序（无论是按 `Ctrl` + `C` 还是关闭窗口），联系人数据库将永远被删除。之所以会这样，是因为 `Node` 只是将数据存储在内存中的一个变量里，程序退出时该变量会被清除。

如果你想永久保存数据，请考虑使用数据库系统，例如 `MySQL`、`MongoDB` 或 `Redis`。数据存储有很多可选方案。例如，`MongoDB` 与 `Node` 配合得很好，幸运的是，对于我们 `JavaScript` 开发者来说，它使用 `JSON` 来表示对象。

更多供你学习 `Backbone` 的链接。

## 官方网站

*   Backbone 文档：[http://backbonejs.org/](http://backbonejs.org/)
*   Backbone GitHub：[http://github.com/documentcloud/backbone/](http://github.com/documentcloud/backbone/)
*   Underscore 文档：[http://underscorejs.org/](http://underscorejs.org/)
*   jQuery 文档：[http://api.jquery.com/](http://api.jquery.com/)
*   Node.js 文档：[http://nodejs.org/api/](http://nodejs.org/api/)
*   本书示例的代码可从 [https://github.com/tlhunter/backbone-book](https://github.com/tlhunter/backbone-book) 获取

## 社区

*   Freenode IRC：`#documentcloud`
*   Backbone 问题：[http://github.com/documentcloud/backbone/issues](http://github.com/documentcloud/backbone/issues)
*   邮件列表：[https://groups.google.com/forum/#!forum/backbonejs](https://groups.google.com/forum/#!forum/backbonejs)

## Twitter

*   关注 Backbone.js 创建者 `@documentcloud`
*   关注本书作者 `@tlhunter`
*   如需更多开源信息，请关注 Packt `@packtopensource`
