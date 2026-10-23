

## Gatsby 页面组件

Gatsby 中使用的最简单组件类型之一是页面组件。Gatsby 页面组件仅用于单个、唯一的页面，例如索引（主页）、404 页面或“关于我们”页面。此页面只会生成一次。

### 构建和修改 Gatsby 页面组件

如前所述，页面是唯一的。通过创建一个页面组件并将其放置在 `/web/src/pages` 文件夹中，Gatsby 将自动生成该页面。

Gatsby 中页面组件的内容可以像下面的示例一样简单，它将整个页面内容放在一个字符串中：

```
// Does not require React because there is no JSX
const AboutUsPage = () => (
  `Who we are: .......`
)
export default AboutUsPage
```

下面是编写上述代码的一种更简短方式：

```
const AboutUsPage = () => {
return `Who we are: .......`
}
```

通过使用 JSX 增强此示例，我们可以创建如下示例：

```
//Requires React because this example uses JSX tags.
import React from 'react'

const AboutUsPage = () => (
  <html>
    <body>
      <h1>Who we are:</h1>
        <p>.......</p>
    </body>
  </html>
)
export default AboutUsPage
```

如果我们将文件命名为 `about-us.js`，并将其放置在 `/web/src/pages/` 目录中，那么运行 Gatsby 后将显示以下屏幕截图：

我们是谁：......

图 10.1 – “关于我们”页面的屏幕截图

此 AboutUsPage 示例将在浏览器中渲染为 HTML。

## Gatsby 模板组件

顾名思义，Gatsby 模板组件就是模板。这些组件用作页面的模板，例如文章页面，其中页面的某些部分将填充唯一内容。这些模板会根据需要处理多次，以生成所需的全部内容。

### 构建和修改模板组件

为了说明这一概念，我们将为网站上的活动创建一个模板。我们需要处理的第一个文件是 `/web/src/gatsby-node.js`。此文件包含使用模板显示博客文章的代码。

在 `gatsby-node.js` 文件的底部，相关代码如下：

```
exports.createPages = async ({graphql, actions}) => {
  await createBlogPostPages(graphql, actions)
}
```

现在，我们将添加另一个函数，通过 GraphQL 生成来自 Sanity 的活动：

```
exports.createPages = async ({graphql, actions}) => {
  await createBlogPostPages(graphql, actions)
  await createEventPages(graphql, actions)
}
```

接下来，我们将复制 createBlogPostPages 的内容。整个函数都应该复制。不过，为节省篇幅，函数中间的部分被省略了。以下是一个片段：

```
async function createBlogPostPages (graphql, actions) {
  const {createPage} = actions
  const result = await graphql(`
  .....
createPage({
    path,
    component:
        require.resolve('./src/templates/blog-post.js'),
    context: {id}
    })
})
```

下面是新创建的 createEventPages 函数的结构：

```
async function createEventPages(graphql, actions) {
    const {createPage} = actions
    const result = await graphql(`
    {
        allSanityEvent {
            ...
        }
    }
`)
    ...
    if (result.errors) throw result.errors

    const eventEdges = (result.data.allSanityEvent || {}).edges || []
    ...
    eventEdges
    .forEach((edge, index) => {
        ...
    })
```

现在，让我们检查这段代码。使用 graphql 和 actions 参数创建一个异步函数。该函数始终会返回一个 promise：

```
async function createEventPages(graphql, actions) {
```

从 actions 中提取 createPage 方法，将其声明为常量，或者说对其进行解构：

```
const {createPage} = actions
```

声明常量 result，用于保存 GraphQL 查询的结果。JavaScript 函数会等待查询完成后再继续执行。查询包含在反引号运算符（`）中：

```
const result = await graphql(`
{
```

下面几行与 GraphQL 查询类似，它通过 `gatsby-source-sanity` 从 `allSanityEvent` 返回 Gatsby 的 id 和 `dateAndTime`：

```
allSanityEvent {
  edges {
    node {
      id
      dateAndTime
    }
  }
}
`
```

如果存在任何错误，则抛出异常：

```
if (result.errors) throw result.errors
```

接下来，我们查看下面这行代码：

```
const eventEdges = (result.data.allSanityEvent || {}).edges || []
```

声明一个名为 `eventEdges` 的常量。如果查询的 edges 不为空，则将查询结果中的 edges 赋给它。否则，如果没有结果，则为 `eventEdges` 实例化一个空数组。请记住，GraphQL 查询的结果封装在 data 中。

使用 forEach 对所有 edges 进行迭代或循环，为 GraphQL 查询中的每条 edge 使用一个 edge 变量：

```
eventEdges
.forEach((edge, index) => {
```

从 edge 的 node 对象中解构 `id` 和 `dateAndTime`：

```
const {id, dateAndTime} = edge.node
```

使用 `date-fns` 中的 `format` 函数格式化 `dateAndTime`。该函数包含在 Gatsby 的 `package.json` 文件中，可以将原本会打印为 `2020-04-10T15:30:00.000Z` 的日期转换为更易接受的四位数年份、正斜杠、两位数月份格式（YYYY/MM），并将结果赋给名为 `dateSegment` 的常量：

```
const { format } = require('date-fns')
const dateSegment = format(dateAndTime, 'YYYY/MM')
```

创建实际作为单页面 URL 并在浏览器中显示的路径。这里使用的是 Gatsby 创建的 ID，而不是 Sanity 的 ID；Sanity 的 ID 可以在 GraphQL 查询中通过 `_id` 访问：

```
const path = `/event/${dateSegment}/${id}/`
```

以下是 URL 路径示例：

```
/event/2021/09/dd88fc6c-10c5-572e-8c58-696add54a14a/
```

最后，调用 `createPage` 函数，传入路径，为每个页面指定 `event.js` 模板文件，并将 `id` 作为 `context` 参数传入：

```
createPage({
  path,
  component:
    require.resolve('./src/templates/event.js'),
  context: {id}
})
```

`event.js` 文件的内容将用于 `createPage` 方法的 `component` 属性，并且会被解析，将页面内容与其模板结合起来，从而生成一个单独的页面。

### 构建 event.js 模板

`event.js` 是每个单独活动页面所使用的模板，因此让我们看看它是如何构建的：

1. 首先，我们需要使用一些 import 语句引入必要的功能：

```
import React from 'react'
import {graphql} from 'gatsby'
import Container from '../components/container'
import GraphQLErrorList from '../components/graphql-error-list'
import Event from '../components/event'
import SEO from '../components/seo'
import Layout from '../containers/layout'
import {toPlainText} from '../lib/helpers'
```

2. 接下来，我们需要执行一个 GraphQL 查询，以获取页面的每个元素：

```
export const query = graphql`
  query EventTemplateQuery($id: String!) {
    event: sanityEvent(id: {eq: $id}) {
      name
      _rawBody
      venue {
        name
      }
    }
  }
`
```

要生成活动页面，至少需要活动的名称、`_rawBody` 以及相关场地的名称。

3. 接下来，使用 props（属性）作为参数定义 `EventTemplate`，并最终将其导出：

```
const EventTemplate = props => {
...
}

export default EventTemplate
```

4. 接下来，从 props 参数中解构 data 和 errors 对象。然后，如果存在 data，则实例化 `event` 常量以保存 data 对象中的 event：

```
const {data, errors} = props
const event = data && data.event
```

5. 接下来，在 `SEO` 组件和 `Event` 组件中调用 `Layout` 组件。为避免造成干扰，错误处理代码已被移除，但如果存在错误，这里也会显示错误。布局会生成页面布局和标签（例如页眉和页脚），并为活动提供一个包装器：

```
return (
  <Layout>
    {event && <SEO title={event.name || 'Untitled'} />}
    {event && <Event {...event} />}
  </Layout>
)
```

> 注意
> 请注意，这里使用了 JavaScript ES6 展开语法，将 Event 对象的每个值分别赋给供 Event 组件使用的独立值。

