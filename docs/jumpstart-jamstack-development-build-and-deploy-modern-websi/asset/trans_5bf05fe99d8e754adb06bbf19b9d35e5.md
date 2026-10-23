
3.  在我们的 `submission-created.js` 文件中，让我们在文件顶部添加客户端，如下所示：

```
const sanityClient = require('@sanity/client')
exports.handler = function(event, context, callback) {
    callback(null, {
        statusCode: 200,
        body: "Hello, World"
    });
}
```

4.  要初始化客户端，我们需要提供 Sanity 项目 ID、数据集名称和访问令牌。在我们的 Sanity 应用主页中，我们的本地环境位于 http://localhost:3333。从你的 Sanity 管理控制台获取项目 ID 和使用的数据集。

5.  我们已经在上一节中创建了访问令牌，现在可以像下面这样复用它：

```
const sanityClient = require('@sanity/client')
const client = sanityClient({
    projectId: 'my_project_id',
    dataset: 'my_dataset',
    token: process.env.SANITY_STUDIO_ADD_EVENT
})
exports.handler = function(event, context, callback) {
    callback(null, {
        statusCode: 200,
        body: "Hello, World"
    });
}
```

6.  现在，我们可以使用 `create(doc)` 方法向 Sanity 添加新事件。

7.  客户端方法需要一个表示文档的普通 JavaScript 对象。它必须包含 `_type` 属性。

让我们开始构建文档。我们的事件的字段如下：

-   标题
-   日期
-   事件 URL
-   地点
-   虚拟
-   描述

8.  在我们的 handler 内部，我们必须获取事件的 body，如下所示：

```
exports.handler = async function(event, context,
        callback) {
    const { payload } = JSON.parse(event.body)
...
}
```

9.  现在，我们可以映射 payload 中的字段，像这样：

```
const doc = {
    _type: 'event',
    name: payload.data.eventTitle,
    dateTime: payload.data.date,
    virtual: payload.data.virtual === 'on',
    eventUrl: payload.data.eventUrl,
    venue: {
        _ref: payload.data.venue,
        _type: 'reference'
    },
    body: [{
        _type: 'block',
        'children': [{
            _type: 'span',
            'text': payload.data.message
        }]
    }],
    approved: false
}
```

10.  现在我们的文档已经准备好了，我们可以将它传递给 `create(doc)` 函数，如下所示：

```
const sanityClient = require('@sanity/client')
const client = sanityClient({
  projectId: 'my_project_id',
  dataset: 'my_dataset',
  token: process.env.SANITY_STUDIO_ADD_EVENT
})

exports.handler = async function(event, context, callback) {
  const { payload } = JSON.parse(event.body)
  const doc = {
    ...
  }

  // Create document
  await client.create(doc)
    .then(res => {
      callback(null, {statusCode: 200})
    })
}
```

完成了。现在，将代码添加到 Git 并推送到 master 分支。部署完成后，我们可以在生产环境中进行测试。

在你的在线 Gatsby 应用程序中，填写表单并按下发送，如图 11.1 所示。一个新的提交将被添加到 Netlify Forms，内容将从 Gatsby 通过 Netlify 函数添加到 Sanity。

### 总结

在本章中，我们学习了如何使用 Netlify Forms 来使创建和管理表单提交变得顺畅。我们使用 Netlify 函数的力量，扩展了 Netlify 表单，并在提交时添加了我们自己的操作。最后，我们使用了 Sanity API，通过 Netlify 函数从 Netlify 表单创建新的事件内容。

在下一章中，我们将创建一个 Alexa 技能，从我们的 Sanity 数据集中读取事件。

## 12 API – Alexa 技能

Alexa 是亚马逊的虚拟助手，可以为个人执行任务或服务，它使用自然语言处理和机器学习将语音输入转换为可执行命令。Alexa 可以提供多种服务，从音乐播放到创建待办事项列表、播放有声读物，以及提供天气、交通和新闻。Alexa 还能够控制许多智能设备，允许你为家庭构建自动化系统。亚马逊用户可以通过 Alexa 技能商店安装技能（功能，类似于应用程序），以扩展 Alexa 的能力。

在本章中，我们将构建 Jumpstart Jamstack Alexa 技能。用户将能够询问接下来的五个事件。通过这个示例，你将能够通过检索过去的事件或所有事件来扩展现有技能，并且你还可以为其他任何应用创建新技能。

我们将涵盖以下主题：

-   Alexa 技能生命周期
-   创建技能

### 技术要求

要开发 Alexa 技能，你必须通过 https://developer.amazon.com/alexa 创建账户来访问 Alexa 开发者控制台。此外，你需要在系统上安装 Sanity Studio。

本章的代码可在 `https://github.com/PacktPublishing/Jumpstart-Jamstack-Development/tree/chapter_twelve` 获取。

### Alexa 技能生命周期

用户通过说出触发词 *Alexa*，后面跟技能名称和要执行的意图（命令）来激活特定技能。例如，*Alexa，问 jumpstart jamstack 告诉我接下来的活动。*

Amazon Echo 设备将请求发送到 Alexa 控制台，控制台将通过联系我们的 Sanity 应用来处理请求，并将结果发送回 Amazon Echo 设备，向用户提示消息。

要构建 Alexa 技能，我们将使用 Alexa 开发者控制台。Alexa 技能的两个组件是技能界面和技能服务。

#### 技能界面

**技能界面** 是亚马逊提供的软件，用户可以通过一个美观干净的 UI 来开发 Alexa 技能。我们将使用技能界面来创建和修改技能的每个组件。

#### 技能服务

**技能服务** 是 Alexa 技能的业务逻辑所在。代码将执行意图，并向界面返回一个 JSON 响应，界面会给用户一个可理解的自然语言句子作为响应。

在本章中，我们的服务将在 AWS Lambda 函数内的 Node.js 中执行。

既然我们知道了技能界面和技能服务是什么，我们将继续为 Alexa 创建技能。

### 创建技能

如前所述，我们将通过 **Alexa 开发者控制台** 创建一个技能，该技能将从我们的活动网站检索五个即将举办的活动。

3. 点击标有 Create Skill 的蓝色按钮：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_176_0.png)

图 12.3 – Amazon Alexa Skills 概览

4. 选择一个技能名称，这也将是你的调用名称。对于这个示例，我们将写 jumpstart jamstack：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_176_1.png)

图 12.4 – Amazon Alexa Skills 创建页面

在 Alexa 范式中，模型是定义技能使用方式的意图和话语的集合。在这个示例中，我们将使用 **自定义** 模型。

5. 如前所述，我们将使用 Alexa 托管的宿主，因此使用默认选中的 **Alexa 托管（Node.js）**：

> **2. 选择一种托管技能后端资源的方法**
>
> 你可以自己配置后端资源，或者让 Alexa 为你托管。如果你决定让 Alexa 托管你的技能，你将可以使用我们的代码编辑器，从而可以从开发者控制台直接部署代码到 AWS Lambda。

![](img/97ab031f38e2174e2f1b51da18dfa1d0_177_0.png)

![](img/97ab031f38e2174e2f1b51da18dfa1d0_177_1.png)

在右上角，如果你愿意，可以更改托管技能的区域。我将其保留为默认的 **美国东部（弗吉尼亚北部）**。

6. 然后，点击右上角的 **创建技能** 蓝色按钮来创建技能：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_177_2.png)

![](img/97ab031f38e2174e2f1b51da18dfa1d0_177_3.png)

![](img/97ab031f38e2174e2f1b51da18dfa1d0_179_0.png)

图 12.8 – Alexa 技能界面顶部菜单

由于这是一个示例技能，本章我们将探讨 **构建**、**代码** 和 **测试**。**分发**、**认证** 和 **分析** 选项卡在技能添加到市场时使用。

### 构建

**构建** 部分是我们可以在其中设置调用名称、配置交互模型、设置槽位和设置端点的位置：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_179_1.png)

图 12.9 – Alexa 技能界面构建菜单

