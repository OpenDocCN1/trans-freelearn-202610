

如您所见，我们添加了将表单提交到 Netlify 所需的两个参数。

3. 让我们继续添加其他字段，如下所示：

```
<form name='propose-event' method='POST' data-netlify='true' >
    <input type='hidden' name='form-name' value='propose-event' />
    <div className='field'>
        <label className='label'>Full name:
            <input className='input' type='text' name='name'/>
        </label>
    </div>
    <div className='field'>
        <label className='label'>Email:
            <input className='input' type='email' name='email'/>
        </label>
    </div>
    ...
    <div className='field'>
        <label className='label'>Message:
            <textarea className='textarea' name='message'></textarea>
        </label>
    </div>
    <div className='field'>
        <button className='button' type='submit'>Send</button>
    </div>
</form>
```

场地（Venue）字段将从我们的站点获取场地信息。让我们添加查询以获取所有场地。

+   4. 在我们的 index.js 页面中，在 `IndexPageQuery` 查询内，我们可以添加一个查询以获取所有场地的 ID 和名称，如下所示：

```
Query IndexPageQuery {
    ...
    venues: allSanityVenue {
        edges {
            node {
                _id
                name
            }
        }
    }
}
...
```

+   5. 将数据映射到一个名为 `venueNodes` 的常量中，如下面的代码片段所示：

```
const venueNodes = (data || {}).venues
    ? mapEdgesToNodes(data.venues)
    : []
```

+   6. 将我们的场地字段添加到表单中，如下所示：

```jsx
<div className="field">
    <label className="label">Venue:
        <select className="select" name="venue">
            {
                venueNodes && venueNodes.map((venue) => (
                    <option id={venue._id}>{venue.name}</option>
                ))
            }
        </select>
    </label>
</div>
```

现在我们的 HTML 表单已经就绪，它应该看起来像这样：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_160_0.png)

图 11.1 – Gatsby 应用程序主页下的表单

我们准备好将其推送到 GitHub。

## 将表单部署到 Netlify

如我们所知，当我们将代码推送到 GitHub 时，它会自动部署到 Netlify，如下面的步骤所示：

+   1. 一旦部署到 Netlify，我们就能看到表单位于我们 Netlify 站点管理面板的“表单”选项卡下，如下面的截图所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_160_1.png)

图 11.2 – 表单选项卡

+   如果你点击 **表单**，你将看到 **提交内容**，如下面的截图所示：

### 已验证的提交

### 没有剩余的已验证提交

此表单的已验证提交已全部被删除或标记为垃圾邮件。

图 11.3 – Netlify 表单页面

让我们添加一个新的提交。

+   转到你在 Netlify 上的 Gatsby 在线站点并填写表单。当你点击 **发送** 后，你会收到以下确认消息：

### 谢谢！

您的表单提交已收到。

← 返回我们的网站

图 11.4 – Netlify 提交成功页面

4. 现在，让我们回到 Netlify 表单管理面板以查看我们的提交，如下面的截图所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_162_0.png)

图 11.5 – Netlify 提交列表

如您所见，我们在 Netlify 上有我们的表单提交。

现在，让我们扩展我们的表单并使用 Sanity API 来获取和插入信息。

## 在 Sanity 中配置令牌表单

当提交新表单时，我们需要在 Sanity 应用程序中添加一个新事件。为了做到这一点，我们创建一个令牌，授予我们的函数访问 Sanity 并创建事件的权限。

我们还需要在我们的事件中添加一个新的 'approved'（已批准）字段，以避免在我们的 Sanity 应用程序上发布不需要的活动。

### 创建 approved 字段

我们将创建一个布尔字段，用于定义活动是否已获批准。

打开位于 `studio/schemas/documents.index.js` 下的事件架构文件，并在 `fields` 数组中添加新字段，如下所示：

```
export default {
  name: 'event',
  type: 'document',
  title: 'Event',
  fields: [
    ...
    {
      name: 'approved',
      type: 'boolean',
      title: 'Approved'
    },
    ...
  ]
}
```

保存文件并重启你的本地应用程序。现在，你应该能在本地 Sanity 应用程序中看到该字段。

要将该字段部署到生产环境，请在终端中进入 `/studio` 文件夹并输入以下命令：

```
sanity graphql deploy
```

### 部署 Sanity 令牌

首先，我们将专门为此表单生成一个新令牌。为此，请进入你的 **Manage Sanity**（管理 Sanity）管理面板 (`https://manage.sanity.io/`) 并点击你的项目。然后，按以下步骤操作：

+   1.  转到 **站点设置 | API** 并向下滚动到 **令牌**。现有令牌被 Gatsby 用于从 Sanity 检索内容。现在，我们需要生成一个新令牌以向 Sanity 写入数据。

2. 点击蓝色的 **添加新令牌** 按钮，如下面的截图所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_164_0.png)

图 11.6 – 令牌

3. 输入标题，然后选择 **权限** 为 **写入**。此权限赋予用户读取、写入和删除数据的权限。现在，点击绿色的 **添加新令牌** 按钮，如下面的截图所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_164_1.png)

图 11.7 – 添加新令牌

生成一个新令牌并提示给你。

4. 复制该令牌，然后回到你的 Netlify 项目并点击 **设置 | 构建与部署 | 环境变量**。

在设置的这一部分中，可以将令牌安全地存储为环境变量，如下面的截图所示：

### 环境变量

为你的构建脚本和附加组件设置环境变量。

SANITY_DEPLOY_STUDIO_TOKEN      skOhxPmPlPUmb2WGJhXVppW...

在文档中了解更多信息关于环境变量 ↗

编辑变量

图 11.8 – Netlify 环境变量设置页面

+   5. 点击 编辑变量 | 新变量。

+   6. 添加你的新令牌，名为 SANITY_STUDIO_PROPOSE_EVENT，并点击保存，如下面的截图所示：

### 环境变量

为你的构建脚本和附加组件设置环境变量。

键
SANITY_DEPLOY_STUDIO_TOKEN

值
skSlzHAEqvWdtWYww4vqM2Lw73qb9dynPyJlek

SANITY_STUDIO_PROPOSE_EVENT

skR9qwePUzIz4nyGCjv43NV1eff84ux3Zdo5X1tb

新变量

在文档中了解更多信息关于环境变量 ↗

保存   取消

图 11.9 – Netlify 环境变量设置页面

6. 一旦 netlify-cli 工具安装完毕，从终端进入 /web 目录并输入以下命令：

```
netlify link
```

7. 出现以下提示时，通过选择之前在 GitHub 创建的项目来选择正确的项目：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_167_0.png)

图 11.10 – 终端

8. 选择 Gatsby 应用程序（即那个以 web 结尾的应用程序）；在我们的案例中是 hands-on-jamstack-web，如下面的截图所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_167_1.png)

图 11.11 – 终端

现在，链接已完成。

9. 一旦链接完成，在 web 目录内输入以下命令：

```
netlify dev
```

此命令将启动你的 Gatsby 项目并设置一个 AWS Lambda 服务器，并在端口 8888 上启动一个服务器。

10. 现在，你可以从 http://localhost:8888 访问你的本地站点，并从 http://localhost:8888/.netlify/functions/hello-world 查看你的函数输出，如下面的截图所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_168_0.png)

图 11.12 – 我们 Gatsby 应用程序上的 Netlify 函数

在本节中，我们在 Gatsby 主页上创建了一个新的 Netlify 表单，在 Sanity 上创建了一个令牌以允许 Netlify 插入一个新的提议事件，并配置了我们的 Netlify 函数文件夹。

在下一节中，我们将创建一个函数，将新的提议事件插入到我们的 Sanity 数据集中。

## 创建 submission-created.js 文件

Netlify 表单提交会触发一个 submission-created 事件。请执行以下步骤来创建 .js 文件：

+   1. 在 web/functions 内部创建一个名为 submission-created.js 的文件。此函数将处理来自 Netlify 表单的提交。
+   2. 接下来，为此函数创建 export .handler，如下所示：

```
exports.handler = async (event, context, callback) => {
}
```

此 handler 接收一个 event 对象，其中包含 path、httpMethod、headers、queryStringparameters、body 和 isBase64Encoded。

context 参数包含诸如用户身份信息等信息。callback 参数类似于 AWS Lambda 函数中的 callback 参数。代码可以在下面的片段中看到：

```
exports.handler = function(event, context, callback) {
    callback(null, {
    statusCode: 200,
    body: "Hello, World"
    });
}
```

Sanity 提供 Client API 通过 API 添加内容。

