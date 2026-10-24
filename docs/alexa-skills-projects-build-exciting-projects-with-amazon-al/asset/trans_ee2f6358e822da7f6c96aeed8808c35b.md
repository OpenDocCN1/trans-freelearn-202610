
## 配置 Handsfree Messenger 技能

在准备好 Twilio 后端之后，我们现在需要配置 Alexa 技能。为此，我们将通过 Amazon 开发者门户进行配置。

我们假设读者已经了解并注册了 Amazon 开发者门户，并且已经掌握了 Amazon Alexa 技能开发者门户的基本入门信息。我们在上一章中已经介绍了 Amazon 开发者门户的账户创建和其他基本细节，因此这里不再重复。

我们将从技能创建开始：

+   1. 在**创建新技能**屏幕上，请为技能命名：

![](img/80ddccf2572659c5b147f23cc6356c5a_82_0.png)

请保存这些更改，并使用屏幕右上角的**下一步**按钮进入下一个屏幕。

+   2. 在此步骤中，我们将为技能选择一个模型。

我们已经在第 2 章 *Hello World, Alexa!* 中介绍了选择模型的细节，因此这里不再赘述。请为技能选择**自定义**模型，并在创建技能后进入下一个屏幕，即技能构建器检查清单。

我们从第 2 章 *Hello World, Alexa!* 中已经知道，交互模型由意图、槽位和示例语句组成。Handsfree Messenger 技能的交互模型包含所有这些元素，我们将在接下来的步骤中通过完成**技能构建器检查清单**来定义它们。

+   3. 作为**技能构建器检查清单**的第一步，我们将为技能提供一个**调用名称**。我们的技能的**调用名称**将是 `handsfree messenger`。这是用户用来调用我们技能的名称。提供调用名称后，请保存模型。

+   4. 接下来，我们需要为技能配置一个意图。我们的技能只包含一个意图 `SendMessage`。我们定义该意图如下：

![](img/80ddccf2572659c5b147f23cc6356c5a_83_0.png)

请点击**创建自定义意图**按钮来创建意图。

但这还不是全部；我们还需要在意图架构中包含一个槽位，以便将消息文本作为参数传递给 Lambda。

## 第五章 树和二叉树

### 5.1 树的逻辑结构

树是n（n≥0）个结点的有限集合，在任意一棵非空树中：

+   1. 有且仅有一个特定的称为根的结点；

+   2. 当n>1时，其余结点可分为m（m>0）个互不相交的有限集合，其中每一个集合本身又是一棵树，并且称为根结点的子树。

树的逻辑结构具有以下特点：

+   （1）树的根结点没有前驱结点，除根结点之外的所有结点有且只有一个前驱结点。

+   （2）树中所有结点可以有零个或多个后继结点。

![](img/80ddccf2572659c5b147f23cc6356c5a_85_0.png)

图5-1 树结构示意图

## 编写 Lambda 函数

一旦交互模型创建完成，我们将保持此屏幕不变，专注于创建 Lambda。

+   1. 请打开一个新的浏览器标签页，并导航到 AWS 控制台，其网址如下：

AWS 控制台位于 https://console.aws.amazon.com。请注意，如果尚未创建账户，您可能需要创建一个新账户。

我们已经在第 2 章 *Hello World, Alexa!* 的“Hello Alexa – 构建 Hello World Alexa 技能”标题下的步骤 8-12 中介绍了基本 Lambda 函数的创建，因此这里不再重复这些步骤。请按照这些步骤操作，将 Lambda 函数命名为 `handsFreeMessengerSkill`，并将技能 ID 与 Lambda 关联，如第 2 章 *Hello World, Alexa!* 的“Hello Alexa – 构建 Hello World Alexa 技能”标题下所述。

由于我们的技能非常简单，我们的 Lambda 将处理两个基本请求：

- **启动请求**：用于处理技能启动
- **意图请求**：用于处理各种意图（在我们的例子中，只有一个 `SendMessage`，用于发送实际的短信）

我们将直接在开发者门户中的 `index.js` 文件中内联编辑代码。我们首先将技能接收到的每个请求进行分离。为此，请声明以下处理程序：

```
exports.handler = function (event, context) {
    try {
        if (event.request.type === "LaunchRequest") {
        } else if (event.request.type === "IntentRequest") {
        }
    } catch (e) {
        context.fail("Exception: " + e);
    }
};
```

这就是我们分离启动和意图事件请求的方式。

2. 接下来，我们将定义一些辅助函数，帮助我们创建语音响应：

```
//------Helper Methods------//

function buildSpeechletResponse(output, shouldEndSession) {
    return {
        outputSpeech: {
            type: "PlainText",
            text: output
        },
        shouldEndSession: shouldEndSession
    };
}

function buildResponse(speechletResponse) {
    return {
        version: "1.0",
        response: speechletResponse
    };
}
```

这些函数帮助 Lambda 在收到特定意图/输入时返回 Alexa 要说的响应。

`buildResponse` 函数将生成最终由 Alexa 说出的简明响应。

`buildSpeechletResponse` 函数将以纯文本形式指定 Alexa 最终要说的输出。它还接受另一个变量 `shouldEndSession`，该变量指定 Alexa 在响应后是否应结束会话。

3. 配置好辅助函数后，我们可以更新 `LaunchRequest` 和 `IntentRequest` 情况的处理，让 Alexa 说出相应的响应，如下所示：

```
exports.handler = function (event, context) {
    try {
        if (event.request.type === "LaunchRequest") {
            onLaunch(function callback(speechletResponse) {
                context.succeed(buildResponse(speechletResponse));
            });
        } else if (event.request.type === "IntentRequest") {
            onIntent(event.request,
                function callback(speechletResponse) {
```

```javascript
context.succeed(buildResponse(speechletResponse));
});
}
} catch (e) {
    context.fail("Exception: " + e);
}
};
```

现在，当我们的 `Lambda` 收到 `LaunchRequest` 时，它将调用 `onLaunch` 方法发送欢迎消息，我们将在下一步中实现该方法。

此外，如果收到 `IntentRequest`，它可以调用 `onIntent` 方法，该方法会进一步根据意图名称进行筛选（尽管目前不需要筛选，因为我们只有一个自定义意图）。

点击屏幕右上角的**保存**来保存进度。

+   4. 我们现在将实现上一步讨论的 `onLaunch` 和 `onIntent` 方法：

```
```javascript
function onLaunch(callback) {
    var output = "Welcome to HandsFree Messenger.";
    var endSession = false;
    callback(buildSpeechletResponse(output, endSession));
}

function onIntent(intentRequest, callback) {
    var intent = intentRequest.intent,
        intentName = intentRequest.intent.name;
    if("SendMessage" === intentName){
        var text = intent.slots.Text.value;
        var recipient = '+31XXXXXXXXXX';
        SendMessage(recipient, text, callback);
    } else {
        throw "Intent not recognized";
    }
}
```
```

`onLaunch` 方法很简单——它创建一条欢迎消息，并使用我们之前定义的辅助方法从该消息创建语音响应后将其发送回去。

`onIntent` 方法相对复杂一些——它根据意图名称筛选意图，如果意图是 `SendMessage`，则从槽位值中提取短信文本，然后通过 `SendMessage` 函数将其转发到接收者的硬编码手机号码。

我们将在接下来的步骤中定义 `SendMessage` 函数。

+   5. `SendMessage` 函数是整个技能的核心。它负责将消息传递给 Twilio 后端。然而，在定义 `SendMessage` 函数之前，我们需要声明一些与 Twilio API 交互时所需的常量。

如果您还记得，我们在上一节的最后一步记下了 SID 和 Auth Token；现在是时候声明它们了。我们将在 `SendMessage` 函数中使用这些常量。请按如下所示声明这些变量：

```
var sid = 'ACXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX';
var token = '4bXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX';
var sender = '+31XXXXXXXXXX';

var https = require('https');
var queryString = require('querystring');
```

请在 AWS 开发者门户的 `index.js` 文件最顶部定义这些常量。定义完成后，开发者门户上的 `index.js` 文件应类似于下图所示：

![](img/80ddccf2572659c5b147f23cc6356c5a_89_0.png)

图 3.20：编写 Lambda 代码

