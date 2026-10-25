

## 在 Grid 中创建、更新和删除

直接在 Grid 中编辑记录是另一项常见任务。Kendo UI Grid 允许用户编辑 Grid 中的内容，这类似于编辑 Excel 表格中的单元格。有两种模式，即内联和弹出，可用于编辑 Grid 中的单行。内联编辑将单元格转换为文本输入字段，并提供更新记录的选项。在弹出编辑中，会显示一个包含相同字段的弹出窗口，并允许用户保存所选记录。

除了在 Grid 中编辑行之外，该库还允许你向 Grid 添加和删除记录。这些操作——创建、读取、更新和删除——映射到可处理这些请求的远程服务。Web 开发中常用的一种范式是提供 RESTful Web 服务。RESTful 服务提供单个端点 URL，并根据请求中提到的 HTTP 方法提供可调用的资源。

## 如何操作...

要启用 Grid 行的编辑，请将 editable 属性设置为 inline 或 popup：
editable: 'inline'

这将启用内联编辑。下一步是为每一行添加编辑和删除操作按钮。在列配置中，你可以定义可应用于 Grid 中每一行的命令。有两个内置命令，即 edit 和 delete。

edit 命令通过将行中的字段转换为输入字段来启用行的编辑。另一方面，delete 命令使用以下代码片段删除所选行：

```
columns: [{
field : 'movieName',
title : 'Movie',
}, {
field : 'year',
title : 'Year'
}, {
field : 'rating',
title : 'Rating'
}, {
command: ['edit', 'destroy']
}]
```

在这里，`columns` 配置列出了各个字段并提及了 `edit` 和 `destroy` 命令。这将为 Grid 中的每一行添加 **编辑** 和 **删除** 按钮。这些按钮应绑定到远程服务，以便当用户更新或删除记录时，服务会在数据库中更新相同内容。在早期的示例中，我们研究了使用带有 `transport` 属性的 `DataSource` 对象。在这里，你可以使用相同的属性来指定每个操作的端点，如以下代码片段所示：

```
dataSource: {
transport: {
read: '/test',
create: {
url: '/test',
type: 'POST'
},
update: {
url: '/test',
type: 'PUT'
},
destroy: {
url: '/test',
type: 'DELETE'
}
},
schema: {
model: {
id: "movieName",
fields: {
movieName: { type: "string" },
year: { type: "number" },
rating: { type: "number" }
}
}
}
}
```

在上述代码片段中，transport 对象为各种操作（如 read、create、update 和 delete）定义了端点 URL。create、update 和 delete 操作指定了端点 URL 以及发送请求时要使用的 HTTP 方法。如果你有一个接受请求并通过检查请求中的 HTTP 方法来调用正确资源的 RESTful 服务，则需要指定 HTTP 方法。如果未指定，则默认发送 GET 请求。

## 工作原理...

将 editable 属性设置为 inline 并在 columns 中添加其命令配置后，Grid 中的每一行都有两个按钮，即编辑和删除，如以下屏幕截图所示：

| Movie | Year | Rating | |
| --- | --- | --- | --- |
| The Shawshank Redemption | 1994 | 9.2 | Edit Delete |
| The Godfather | 1972 | 9.2 | Edit Delete |
| The Godfather - Part 2 | 1974 | 9 | Edit Delete |
| Pulp Fiction | 1994 | 8.9 | Edit Delete |
| The Good, the Bad and the Ugly | 1966 | 8.9 | Edit Delete |

当你点击 Edit 时，所选行将切换到编辑模式并显示文本输入字段。

| Movie | Year | Rating | |
| --- | --- | --- | --- |
| The Shawshank Redemption | 1994 | 9.2 | Edit Delete |
| The Godfather | 1972 | 9.2 | Edit Delete |
| The Godfather - Part 2 | 1974 | 9.00 | Update Cancel |
| Pulp Fiction | 1994 | 8.9 | Edit Delete |
| The Good, the Bad and the Ugly | 1966 | 8.9 | Edit Delete |

## Kendo UI Grid

更改值并点击 **Update** 后，将向提到的 URL 发送带有请求体中模型数据的 PUT 请求。在这里，用户也有取消操作的选项。同样，当你点击 **Delete** 按钮时，会显示一个询问用户确认删除操作的警告框。如果用户点击 **OK**，则会向提到的服务发送 `DELETE` 请求，并删除 Grid 中相应的行。当你将 editable 属性更改为 popup 时，点击 **Edit** 按钮后会显示一个弹出窗口。

![](img/2a66507445090790d34dc5588b56b24c_53_0.png)

### 更多内容...

Kendo UI Grid 还允许用户向 Grid 添加记录。Grid 组件可以定义一个 `toolbar` 属性，其值是一个包含要显示的一组操作按钮的数组。在这种情况下，要向 Grid 添加新记录，请将值指定为 `create`：

```javascript
toolbar: ['create']
```

这将在 Grid 顶部添加一个 **添加新记录** 按钮，如以下屏幕截图所示：

![](img/2a66507445090790d34dc5588b56b24c_53_1.png)

点击 **添加新记录** 按钮后，将向 Grid 添加一行。

| Movie | Year | Rating | | |
|---|---|---|---|---|
| | 0.00 | 0.00 | ✓ Update | ⊗ Cancel |
| The Shawshank Redemption | 1994 | 9.2 | ✎ Edit | × Delete |
| The Godfather | 1972 | 9.2 | ✎ Edit | × Delete |
| The Godfather - Part 2 | 1974 | 9 | ✎ Edit | × Delete |
| Pulp Fiction | 1994 | 8.9 | ✎ Edit | × Delete |
| The Good, the Bad and the Ugly | 1966 | 8.9 | ✎ Edit | × Delete |

这是当 `editable` 属性设置为 `inline` 时的情况。如果将其设置为 `popup`，则会显示一个弹出窗口。点击 **Update** 后，将向服务发送一个 `POST` 请求（参考 `transport` 中的 `create`，如 _如何操作..._ 部分中最后一个示例所示），并在请求体中提及模型数据。

## 使用可视化机制提高 Grid 的性能

考虑这样一个场景：服务返回一个需要在 Grid 中显示的大型数据集。例如，假设数据集包含约 10,000 条记录。如前所述，`DataSource` 对象中的每条记录都会创建一个表格行。在这种情况下，有 10,000 条记录，为每条记录创建一个表格行（`tr`）会导致浏览器内存不足。创建 10,000 个 DOM 节点并将其追加到文档树中会消耗大量内存。解决此问题的一种方法是使用虚拟化。在虚拟化中，只创建一组固定的节点，当用户在 Grid 上滚动时，现有节点会更新以显示 `DataSource` 中的下一组记录。这样，我们不仅通过重用 DOM 节点优化了浏览器内存的使用，还在用户滚动浏览大型列表时提供了流畅的体验。

## 如何操作...

可视化的第一步是设置要显示的表格行数。这些表格行将被重用以显示 DataSource 对象中的大量记录。让我们先使用以下代码片段将 DataSource 对象中的 pageSize 属性设置为要显示的行数：

```
dataSource: {
transport: {
read: 'http://localhost/kendo/code/chapter2/remote.json',
},

pageSize: 4,
}
```

下一步是指定 scrollable 对象，并将 virtual 属性设置为 true：

```
scrollable: {
virtual: true
}
```

通过将 virtual 属性设置为 true，将启用数据的可视化。

> 为 pageSize 属性设置的值 4 仅用于演示目的。你应该根据记录数量以及最能匹配用户体验的值将其设置为较大的值。

## 工作原理...

当你将 pageSize 属性设置为 4 并将 virtual 设置为 true 时，将仅显示四行且仅创建四个节点，如以下屏幕截图所示：

| Movie | Year | Rating |
|---|---|---|
| The Shawshank Redemption | 1994 | 9.2 |
| The Godfather | 1972 | 9.2 |
| The Godfather - Part 2 | 1974 | 9 |
| Pulp Fiction | 1994 | 8.9 |

