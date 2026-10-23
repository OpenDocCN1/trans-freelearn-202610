

## 调用

调用名称是我们与技能交互时需要使用的关键词。我们的调用名称是 `jumpstart jamstack`，因此我们可以说：Alexa，打开 jumpstart jamstack：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_180_0.png)

图 12.10 – Alexa 技能界面：调用选项卡

## 交互模型

在交互模型中，我们可以设置意图（intent）和话语（utterance）。任何 Alexa 技能都会创建以下四个默认的内置意图：

- `CancelIntent`
- `HelpIntent`
- `StopIntent`
- `NavigateHomeIntent`

`CancelIntent` 和 `StopIntent` 类似，当你想停止交互时会被调用。例如，当 Alexa 正在为你朗读新闻时，你可以说“Alexa，停止”，Alexa 就会停止朗读新闻；或者当你通过 Alexa 听音乐时，想停止播放时，就会调用 `StopIntent` 和 `CancelIntent`。

当你想了解更多关于某个技能以及有哪些可用话语时，会使用 `HelpIntent`。

当你使用一个多层级技能并想返回该技能的顶层菜单时，会使用 `NavigateHomeIntent`。

在我们的意图列表中，还有默认的 HelloWorldIntent，它包含七个话语。现在让我们添加 GetUpcomingEventIntent：

1. 点击意图列表上方的蓝色“添加意图”（Add Intent）按钮：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_181_0.png)

2. 在我们的 **创建自定义意图** 部分，将意图名称添加为 GetUpcomingEventsIntent。然后，点击蓝色的 **创建自定义意图** 按钮：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_182_0.png)

Amazon 还提供了一组可用于特定技能的意图，例如天气、视频和音频。

既然我们已经创建了自定义意图，就可以开始添加话语了。

用户可能会问：*Alexa，询问 jumpstart jamstack 即将发生的事件*，或者 *Alexa，让 jumpstart jamstack 告诉我即将发生的事件*。

我们可以添加任意数量的话语。对于这个示例，让我们添加以下话语：

- 列出即将发生的事件
- 告诉我即将发生的事件
- 有哪些即将发生的事件

要添加话语，请执行以下步骤：

1. 在 **示例话语** 文本框中写入话语，然后按下文本框右侧的 **+** 按钮。如果不点击 **+** 按钮，话语就不会被添加：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_183_0.png)

图 12.13 – Alexa 技能界面：意图页面

现在你的意图中应该已经添加了三个话语。

2. 点击 **意图** 表单上方的 **保存模型** 按钮，然后点击 **保存模型** 按钮旁边的 **构建模型** 按钮：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_183_1.png)

图 12.14 – Alexa 技能界面：意图页面

现在，我们的意图已添加，模型也已构建，可以在我们的 Alexa 服务中使用。

在本节中，我们创建了一个新意图来检索事件，并重新构建了模型以激活新意图。在下一节中，我们将查看意图历史。

## 意图历史

点击左侧菜单中的“意图历史”（Intent History）链接，你可以查看最近 30 天内对你的技能进行的所有频繁意图请求。你可以查看“未解决的话语”（Unresolved Utterances）和“已解决的话语”（Resolved Utterances）。从这里，你可以改进你的意图话语：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_184_0.png)

图 12.15 – Alexa 技能“意图历史”页面

## 话语冲突

点击左侧菜单中的“话语冲突”（Utterance Conflicts），你可以查看意图之间是否存在话语冲突：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_184_1.png)

图 12.16 – Alexa 技能“话语冲突”页面

## JSON 编辑器

点击左侧菜单中的“JSON 编辑器”（JSON Editor），你也可以通过 JSON 代码创建意图。当你想要快速从另一个技能导入现有意图时，这非常有用：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_185_0.png)

## 槽位类型

槽位（Slot）可以接收通过话语传递的变量。默认情况下，Amazon 提供了许多槽位类型，例如数字（number）、城市（city）和国家（country），Alexa 可以识别这些类型。你也可以创建自定义槽位类型，并方便地处理它。

槽位类型的一个例子可以在天气技能中找到，当你查询某个特定城市的天气时。

在你的 WeatherByCity 意图中，你不需要为世界上的每个城市添加话语，你可以将其作为变量传递：

### 槽位类型

![](img/97ab031f38e2174e2f1b51da18dfa1d0_186_0.png)

图 12.18 – Alexa 技能“槽位类型”页面

## 端点

**端点** 将接收 JSON 负载请求。由于我们使用的是 AWS Lambda 函数，这里可以保留默认设置。

如果你想在 AWS 之外的其他地方托管 Alexa 服务，可以选择 **HTTPS** 选项：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_187_0.png)

图 12.19 – Alexa 技能“端点”页面

## 代码

现在我们已经探索了“构建”（Build）部分，让我们跳到 Alexa 服务部分，也就是我们的 Node.js 代码：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_188_0.png)

图 12.20 – Alexa 技能“代码”页面

如你所见，该界面提供了一个代码编辑器，我们可以在其中实现我们的 Alexa 服务。

在我们的 `index.js` 文件中，我们可以为意图创建处理程序。我们添加的任何新意图都可以在这里实现。Alexa 提供了一个 API 来处理请求，它被称为 ask-sdk-core API。请求位于 handlerInput JSON 对象内部。

处理程序将处理我们的意图，并由此对 Sanity 数据集发出 Sanity 调用。

在我们 `index.js` 文件的顶部，我们引入 ask-sdk-core：

```
const Alexa = require('ask-sdk-core');
```

处理程序包含两个函数：`canHandle()` 函数通过检查请求类型来判断该处理程序是否能处理此请求；`handle()` 函数将处理请求并返回响应：

```
const MyCustomHandler = {
    canHandle(handlerInput) {
        return Alexa.getRequestType(handlerInput.requestEnvelope) === 'MyRequestType' && Alexa.getIntentName(handlerInput.requestEnvelope) === 'MyIntent';
    }
};
```

在我们的 `canHandle()` 函数中，我们使用 `ask-sdk-core` API 获取请求类型和请求名称。

如果两个条件都为真，则意味着该处理程序可以处理这个请求，因此我们实现 `handle()` 函数：

```
const MyCustomHandler = {
    canHandle(handlerInput) {
        return Alexa.getRequestType(handlerInput.requestEnvelope) === 'MyRequestType' && Alexa.getIntentName(handlerInput.requestEnvelope) === 'MyIntent';
    },
    handle(handlerInput) {
        const speakOutput = 'This is my custom text response';
        return handlerInput.responseBuilder
            .speak(speakOutput)
            .getResponse();
    }
};
```

在这个简单的示例中，我们只是返回一个文本。我们使用 `responseBuilder()` 函数构建响应，将文本传递给 `speak()` 函数，然后由 `getResponse()` 函数构建最终响应。

我们在文件中遇到的第一个处理程序是 LaunchRequestHandler。当你使用命令 *Alexa，打开 jumpstart jamstack* 打开技能时，会调用该处理程序：

```javascript
const LaunchRequestHandler = {
    canHandle(handlerInput) {
        return Alexa.getRequestType(handlerInput.requestEnvelope) === 'LaunchRequest';
    },
    handle(handlerInput) {
        const speakOutput = 'Welcome, you can say Hello or Help. Which would you like to try?';
        return handlerInput.responseBuilder
            .speak(speakOutput)
            .reprompt(speakOutput)
            .getResponse();
    }
};
```

它将返回消息：*欢迎，你可以说 Hello 或 Help。你想尝试哪一个？*

让我们更改这条消息，使其与我们的技能相关。将其替换为：欢迎使用 jumpstart jamstack 技能，你可以说“告诉我即将发生的事件”或“帮助”。你想尝试哪一个？

因此，我们的 LaunchRequest 处理程序如下：

```javascript
const LaunchRequestHandler = {
    canHandle(handlerInput) {
        return Alexa.getRequestType(handlerInput.requestEnvelope) === 'LaunchRequest';
    },
    handle(handlerInput) {
        const speakOutput = 'Welcome to the jumpstart jamstack skill, you can say tell me the upcoming events or Help. Which would you like to try?';

        return handlerInput.responseBuilder
            .speak(speakOutput)
            .reprompt(speakOutput)
            .getResponse();
```

