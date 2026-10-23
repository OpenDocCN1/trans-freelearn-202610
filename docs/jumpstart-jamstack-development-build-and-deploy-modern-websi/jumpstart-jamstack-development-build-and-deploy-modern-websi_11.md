# 8
Gatsby与GraphQL

在本章中，我们将开始通过GraphQL将内容引入Gatsby。GraphQL是学习Gatsby工作原理的一个很好的起点，因为Gatsby使用GraphQL来获取内容。正如我们在第6章“Sanity的GraphQL游乐场”中所学到的，Sanity通过GraphQL应用程序接口（API）暴露其信息，因此Gatsby可以轻松地通过GraphQL使用Sanity的内容。

以下是本章将涵盖的主要主题：

- GatsbyJS中的GraphQL
- GraphiQL，一个GraphQL导航器

### 技术要求

对于本章，需要安装Gatsby应用程序，并需要一个终端程序。此外，还需要一个Web浏览器。

## GatsbyJS中的GraphQL

在本节中，我们将了解GraphQL在Gatsby上下文中是如何使用的。我们还将学习Gatsby使用GraphQL的方式与Sanity的GraphQL API暴露GraphQL的方式之间的主要区别。

Gatsby对GraphQL的使用与Sanity不同，因为它使用图（graph）的概念来构建其GraphQL查询。让我们了解什么是图。

在下图中，图形由节点和边组成：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_112_0.png)

图8.1 – 图

在Gatsby对GraphQL的实现中，就图而言，每个Sanity文档都表示为一个节点。回顾第6章“Sanity的GraphQL游乐场”中用于返回所有事件的GraphQL查询，该查询非常简单，如以下代码片段所示：

```
{
  allEvent {
    name
  }
}
```

在Gatsby中，我们发现返回所有事件对应的查询有两个主要区别，如下所示：

- 使用`allSanityEvent`代替`allEvent`。Sanity的开发者创建并正式维护了`gatsby-source-sanity`插件。他们选择添加此前缀以区分Sanity文档与其他文档，这种技术有时称为命名空间（namespacing）。Gatsby认可这是一种良好实践。显然，在Sanity端使用GraphQL游乐场意味着只获取Sanity的内容，而在Gatsby中，可以同时使用许多不同的内容源。

- 查询需要指定`edges`和`node`部分才能返回每个事件，如以下代码示例所示：

```
query MyQuery {
  allSanityEvent {
    edges {
      node {
        name
      }
    }
  }
}
```

同样，由于GraphQL不仅指定了请求格式，还指定了结果格式，因此结果会以边（edges）数组和每个文档的节点（node）形式封装返回，如下所示：

```
{
  "data": {
    "allSanityEvent": {
```

同样，查询结果也包含边和节点，如以下代码片段所示：

```
{
  "data": {
    "allSanityEvent": {
      "edges": [
        {
          "node": {
            "name": "The Appleseed Cast"
          }
        },
        {
          "node": {
            "name": "Friday Drinks"
          }
        }
      ],
    }
  }
```

![](img/97ab031f38e2174e2f1b51da18dfa1d0_115_0.png)

### 图8.2 – GraphiQL

在左侧，我们可以看到GraphQL资源管理器。它通过向用户显示所有可用模式（schema）的视图，帮助我们自动创建GraphQL查询。因此，通过展开每个模式，会显示过滤和排序的选项，以及可作为查询结果返回的字段（包括它们的嵌套元素）。让我们详细看一下与之前在第6章“Sanity的GraphQL游乐场”中讨论类似的示例。

单击所需的起始查询——例如`allSanityEvent`——就会看到可用于过滤选项的各种实体字段，基本查询将在中间部分创建。当选择各个字段时，GraphiQL会自动将这些字段添加到GraphQL查询中。

接下来，让我们查看以下展开的`allSanityEvent`部分截图：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_116_0.png)

图8.3 – 带示例查询的GraphiQL

当展开`allSanityEvent`部分，然后展开`filter`时，会显示过滤选项。`virtual eq false`根据以下条件过滤结果：只选择非虚拟（eq false）的事件。以下截图显示了左侧导航的详细信息，该导航允许过滤和选择要从查询中返回的字段：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_117_0.png)

图8.4 – 查询选项

然后，在展开`edges`后，从节点字段列表中选择`name`。

查询会自动创建，如下所示：

```
```graphql
query MyQuery {
  allSanityEvent(filter: {virtual: {eq: false}}) {
    edges {
      node {
        name
      }
    }
  }
}
```
```

要执行查询，请点击**Play**按钮。

结果显示在右侧的第三个面板中。另外两个功能是**History**和**Prettify**。**Prettify**用于适当格式化代码。

```
```
name
}
}
}
}
~export default ComponentName
```
```

让我们检查生成的代码。第一行导入React。第二行从gatsby导入graphql，用于执行查询。接下来，我们通过创建匿名函数并将其赋值给常量来定义名为ComponentName的组件，因为该函数不会改变。在这种情况下，ComponentName是一个有效的名称，但意为占位符，应予以更改。

接下来，让我们查看单个事件查询的样式。我们不再使用`allSanityEvent`（它会从Sanity端返回所有事件给Gatsby），而是使用单个`sanityEvent`事件查询，该查询只返回一条记录，接受来自Sanity端的事件的`id`，用下划线表示。以下代码片段展示了这一概念的示例：

```
```
query MyQuery {
 sanityEvent(_id: {eq: "6ddd2730-9aca-46dd-a7fd-850ab306f7fb"}) {
 name
 }
}
```
```

接下来，让我们查看结果。请注意，在以下代码片段中，结果被包裹在一个`data`对象中：

```
```
{
 "data": {
 "sanityEvent": {
 "name": "IT Conference"
 }
 }
}
```
```



同样地，我们可以使用场地名称查询事件，如下面的代码片段所示。这与*第6章“Sanity的GraphQL Playground”*中的示例类似：

```
query MyQuery {
  sanityEvent(venue: {name: {eq: "Will's Pub"}}) {
    name
  }
}
```

同样，结果将如前面代码片段的`name`字段所请求的那样，显示事件的名称，如下所示：

```
"data": {
    "sanityEvent": {
      "name": "The Applesead Cast"
    "venue": {
      "name": "Will's Pub"
    }
  }
}
```

这两个示例清楚地展示了GraphQL如何在Gatsby中使用，提供了一种通过GraphQL查询Sanity来源的简便方法。

## 总结

在本章中，我们了解了Gatsby Sanity来源插件，以及它如何使用GraphQL与Sanity交互并从中导入内容。我们学习了如何在Gatsby环境中使用基于Web的GraphQL应用程序来测试和编写GraphQL查询。我们还学习了GraphQL如何通过其GraphQL API获取Sanity的内容。

在下一章中，我们将学习Gatsby如何从文件系统和**内容管理系统**（CMS）等来源获取额外内容，因为我们距离使用Gatsby构建网站的目标越来越近。
