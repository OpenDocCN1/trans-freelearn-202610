

```json
"name": {
    "value": "4"
}
```

确认 JSON 配置后，返回到 **技能构建者清单**。

- 4. 从 **技能构建者清单** 中导航到 **Endpoint** 屏幕，从 Endpoint 屏幕复制技能 ID，并将其保存到某个地方。我们稍后会用到它。

## 配置 Lambda 函数

一旦 **交互模型** 创建完成，我们暂时保持配置屏幕不变，并将重点放在创建 Lambda 上：

- 1. 打开一个新的浏览器标签页并导航到 AWS 控制台，其网页 URL 在以下信息框中提及：

![](img/80ddccf2572659c5b147f23cc6356c5a_111_0.png)

AWS 控制台可在 https://console.aws.amazon.com 找到。请注意，如果尚未创建，您可能需要创建一个新账户。

为 Lambda 函数命名为 `factlySkill` 并按以下截图所示填写详细信息：

![](img/80ddccf2572659c5b147f23cc6356c5a_112_0.png)

图 4.22：命名 Lambda

填写完详细信息后点击 Create function 按钮。

- 2. 在下一个屏幕中，在 **Designer** 部分下，您需要添加一个触发器来调用 Lambda 函数：

![](img/80ddccf2572659c5b147f23cc6356c5a_112_1.png)

图 4.23：配置 Lambda

从左侧的触发器列表中，选择 **Alexa Skills Kit**，因为 Alexa 将调用我们的 Lambda 函数并向其提供输入：

![](img/80ddccf2572659c5b147f23cc6356c5a_113_0.png)

添加触发器后，下一步将是添加我们在上一节中复制的 Skill ID。在以下截图所示的部分中添加 Skill ID：

![](img/80ddccf2572659c5b147f23cc6356c5a_113_1.png)

添加 **Skill ID** 可确保 Lambda 仅响应由已验证技能生成的请求。添加 **Skill ID** 后，点击屏幕底部的 **Add** 按钮，然后点击屏幕右上角的 **Save** 按钮。

- 3. 点击 **Save** 按钮后，选择 **Designer** 部分下的 `factlySkill` 框，如以下截图所示：

![](img/80ddccf2572659c5b147f23cc6356c5a_114_0.png)

图 4.26：完成 Lambda 配置

这将在底部打开一个代码编辑器，我们将在其中添加 Lambda 的代码。

## 编写 Lambda 函数

我们将在开发者门户本身的 `index.js` 文件中内联编辑代码：

- 1. 首先，我们将定义一个 JSON 数组，其中将包含测验技能提出的问题。此数组可以定义在 `index.js` 文件的顶部，甚至在 `exports.handler` 之前：

```javascript
//questions is an array of json objects
var questions = [
    {
        "World War I began in which year?": [
            "1923",
            "1938",
            "1917",
            "1914"
        ]
    },
    {
        "Adolf Hitler was born in which country?": [
            "France",
            "Austria",
            "Hungary",
            "Germany"
        ]
    },
    {
        "The battle of Hastings was fought in which country?":
        [
            "France",
            "Russia",
            "England",
            "Norway"
        ]
    }
];

var correctAnswerSlots = [3, 1, 2];
```

如前面的代码所示，除了 questions 数组之外，我们还定义了另一个名为 `correctAnswerSlots` 的数组，其中包含 `questions` 数组中每个问题的正确答案的索引。

困惑吗？我们可以用一个例子来阐述。可以从 `correctAnswerSlots` 数组推断出第一个问题 World War I began in which year? 的答案是 1914，它位于索引 3（即第四个选项）。因此，我们在 `correctAnswerSlots` 数组的第一个位置放置了 3。

- 2. 现在，我们可以继续在 `exports.handler` 中实现事件的处理。我们的 Lambda 将处理两种基本请求：
    - `LaunchRequest`：处理技能的启动
    - `IntentRequest`：处理各种 Intent（在我们的例子中，我们只有两个 intent，第一个是 Answer intent，表示 Factly 提出的问题的答案，以及 `AMAZON.StopIntent`，表示退出技能）

我们将首先对技能收到的每个请求进行区分。为此，声明以下处理器：

```javascript
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

这就是我们区分 Launch 和 Intent 事件请求的方式。

- 3. 接下来，我们将定义一些辅助函数，帮助我们创建语音响应：

```javascript
// ------ Helper functions ------

function buildSpeechletResponse(output, repromptText, shouldEndSession) {
    return {
        outputSpeech: {
            type: "PlainText",
            text: output
        },
        reprompt: {
            outputSpeech: {
                type: "PlainText",
                text: repromptText
            }
        },
        shouldEndSession: shouldEndSession
    };
}

function buildResponse(sessionAttributes, speechletResponse) {
    return {
        version: "1.0",
        sessionAttributes: sessionAttributes,
        response: speechletResponse
    };
}
```

这些函数帮助 Lambda 在接收到特定 intent/输入时返回供 Alexa 朗读的响应。

`buildResponse` 函数将生成供 Alexa 朗读的纯文本响应。

`buildSpeechletResponse` 函数将指定 Alexa 最终会以纯文本形式朗读的输出。它还接受另一个变量 `shouldEndSession`，该变量指定 Alexa 在响应后是否应结束会话。

- 4. 配置好辅助函数后，我们可以更新 `LaunchRequest` 和 `IntentRequest` 情况的处理，使 Alexa 朗读相关的响应，如以下代码所示：

```javascript
exports.handler = function (event, context) {
    try {

        if (event.request.type === "LaunchRequest") {
            onLaunch(event.request,
                event.session,
                function callback(sessionAttributes, speechletResponse) {
                    context.succeed(buildResponse(sessionAttributes,
                        speechletResponse));
                });
        } else if (event.request.type === "IntentRequest") {
            //TBD
        }
    } catch (e) {
        context.fail("Exception: " + e);
    }
};
```

当 Lambda 接收到 `LaunchRequest` 时，它将调用 `onLaunch` 方法以返回欢迎消息：

```javascript
function onLaunch(launchRequest, session, callback) {
    getWelcomeResponse(callback);
}
```

我们将在下一步实现 `getWelcomeResponse` 方法。

请通过点击屏幕右上角的 **Save** 按钮保存我们的进度。

- 5. `getWelcomeResponse` 方法负责制定启动响应并向用户提出第一个问题：

```javascript
var ANSWER_COUNT = 4;
var GAME_LENGTH = 3;

function getWelcomeResponse(callback) {
    var sessionAttributes = {},
        speechOutput = "Factly will ask you " +
            GAME_LENGTH.toString()
            + " questions, try to get as many right as you can. Just say the number of the answer. Let's begin. ",
        shouldEndSession = false,

        gameQuestions = populateGameQuestions(),
        currentQuestionIndex = 0,
        correctAnswerIndex =
            correctAnswerSlots[currentQuestionIndex],
        answers = populateAnswers(gameQuestions, 0),

        spokenQuestion =
            Object.keys(questions[gameQuestions[currentQuestionIndex]])[0],
        repromptText = "Question 1. " + spokenQuestion + " ",

        i;

    for (i = 0; i < ANSWER_COUNT; i++) {
        repromptText += (i + 1).toString() + ". " + answers[i] +
            ". "
    }
    speechOutput += repromptText;
    sessionAttributes = {
        "speechOutput": repromptText,
        "repromptText": repromptText,
        "currentQuestionIndex": currentQuestionIndex,
        "correctAnswerIndex": correctAnswerIndex + 1,
        "questions": gameQuestions,
        "score": 0
    };
    callback(sessionAttributes,
        buildSpeechletResponse(speechOutput, repromptText,
            shouldEndSession));
}
```

