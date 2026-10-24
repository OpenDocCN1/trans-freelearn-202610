

buildResponse 函数将生成要由 Alexa 朗读的直白响应。

buildSpeechletResponse 函数将以纯文本形式指定 Alexa 最终要朗读的输出。它还接受另一个变量 shouldEndSession，该变量指定 Alexa 在响应结束后是否应结束会话。

定义 fbGet 函数，它将接受一个 key 参数，并通过 REST 调用从给定的主机获取数据：

```
function fbGet(key){
    return new Promise((resolve, reject) => {
        var options = {
            hostname: firebaseHost,
            port: 443,
            path: key + ".json",
            method: 'GET'
        };
        var req = https.request(options, function (res) {
            res.setEncoding('utf8');
            var body = '';
            res.on('data', function(chunk) {
                body += chunk;
            });
            res.on('end', function() {
                resolve(JSON.parse(body))
            });
        });
        req.end();
        req.on('error', reject);
    });
}
```

前面的函数将接受保存温度数据的键参数（在我们的例子中，这个键是 Temperature），并从 Firebase 主机获取数据。接收到的数据是 JSON 格式，因此需要解析。

- 3. 添加此方法后，我们需要从 exports.handler 中调用它。请用对 fbGet 的调用更新 exports.handler，如下面的代码所示：

```
exports.handler = (event, context, callback) => {
    try {
        if (event.request.type === "IntentRequest") {
            if ("OneShotFetchTemperatureIntent" ===
                event.request.intent.name) {
                fbGet("/Temperature").then(res => {
                    context.succeed(buildResponse(buildSpeechletResponse("The temperature is " + res + " degrees", true)));
                })
            } else {
                throw "Invalid intent";
            }
        }
    }
    catch(e) {
        context.fail("Exception: " + e);
    }
};
```

获取成功后，我们会创建一个语音响应并将其传回，供 Alexa 朗读。这样就完成了我们的 Lambda 实现。

- 4. 我们需要将 Lambda 链接到我们的技能，为此，我们需要 Lambda 的 Amazon 资源名称（ARN）。ARN 位于 Lambda 函数/代码编辑器页面的右上角：

![](img/80ddccf2572659c5b147f23cc6356c5a_215_0.png)

图 6.34：复制 ARN

复制 ARN。复制完成后，请切换到之前配置技能交互模型的浏览器标签页：

![](img/80ddccf2572659c5b147f23cc6356c5a_216_0.png)

图 6.35：使用 ARN 更新端点 – 步骤 1

如上图所示，单击“技能构建器清单”下的“端点”按钮，这将导航到端点屏幕，如下面的屏幕截图所示：

![](img/80ddccf2572659c5b147f23cc6356c5a_216_1.png)

图 6.36：使用 ARN 更新端点 – 步骤 2

选择 **AWS Lambda ARN**，在 **默认区域** 选项下，添加我们从上一屏幕复制的 ARN，然后点击屏幕左上角的 **保存端点** 按钮。

点击 **保存端点** 按钮后，请再次点击 **构建** 标签页以返回到仪表板。

- 5. 在仪表板上，**技能构建器清单** 中的所有项目现在都应显示为绿色，如下面的屏幕截图所示：

![](img/80ddccf2572659c5b147f23cc6356c5a_217_0.png)

这表示我们的技能现在已准备好进行测试。请点击上方屏幕截图中显示的 **测试** 标签页。

## 测试与调试技能

**测试** 标签页应会打开 Alexa 模拟器：

![](img/80ddccf2572659c5b147f23cc6356c5a_218_0.png)

Alexa 模拟器大大减少了对实体 Amazon Echo 的需求，因为你只需使用模拟器就可以轻松测试 Ambience Manager 技能。

请在文本框中输入 `Alexa ask ambience manager to get temperature`，如下面的屏幕截图所示，然后按 *Enter* 键：

![](img/80ddccf2572659c5b147f23cc6356c5a_218_1.png)

Alexa 应该会用正确的响应“The temperature is...”进行回复，如果你的电脑音量已打开，你甚至可以听到 Alexa 说出该响应。

我们的技能现在已测试完毕，可以随意把玩了。你可以在信息框中提到的链接处找到本章 Lambda 的代码。

请在此处找到 Lambda 的代码：https://github.com/madhurbhargava/AlexaSkillsProjects/blob/master/Chapter_6_Smart_Home_Index.js.

但是，如果发生错误，技能的行为不符合我们的预期，该怎么办？

对于 Alexa 技能而言，一个有效的调试工具是 Amazon CloudWatch。CloudWatch 可以显示启动技能时发生的完整日志详细信息，以及它如何处理各种意图。如果你的 Lambda 代码中包含日志语句（Log Statements），你还可以在 CloudWatch 中通过这些日志语句的输出来检查程序流程。

可以从 AWS 控制台访问 CloudWatch。

要访问 CloudWatch，可前往 https://console.aws.amazon.com.

要检查技能的执行状态，请在 AWS 控制台的 AWS 服务列表中选择 CloudWatch，如下面的屏幕截图所示：

![](img/80ddccf2572659c5b147f23cc6356c5a_220_0.png)

图 6.40：启动 CloudWatch

这应该会将你带到 CloudWatch 仪表板，如下面的屏幕截图所示，该仪表板除了日志之外还提供许多功能：

![](img/80ddccf2572659c5b147f23cc6356c5a_220_1.png)

图 6.41：CloudWatch 仪表板

不过，我们感兴趣的是日志，因此请单击“日志”选项：

![](img/80ddccf2572659c5b147f23cc6356c5a_221_0.png)

图 6.42：从 CloudWatch 仪表板启动日志

这里会显示你拥有的且有可用日志的技能列表。单击你选择的技能，会显示可用日志列表，并按时间顺序排序。你可以单击其中任意一个日志，查看详细的日志记录，其中包含你的技能已响应的完整事件列表：

![](img/80ddccf2572659c5b147f23cc6356c5a_221_1.png)

图 6.43：CloudWatch 中的日志列表

## 总结

我们希望你喜欢创建 Ambience Manager 技能的过程。这个技能包含许多并非 Alexa 专属的组成部分，例如 Firebase、环境传感器和移动应用。我们还介绍了 One-Shot 启动以及通过 CloudWatch 进行技能调试。

与之前的技能一样，Ambience Manager 技能仍有改进空间，例如处理湿度数据和引入额外的意图。

我们把改进留作练习，让用户可以进一步磨练技能。

我们也希望你现在对 Alexa 技能的知识更有信心，并且迫不及待地想看到你接下来会创建什么。

在最后一章中，我们将探索基于语音的助手的未来。

## 7. 基于语音的个人助手的未来

> “要么做，要么不做。没有尝试。”

> ——尤达大师

我们已经完成了六章，在这六章中，我们只专注于 Alexa。在本章，也是最后一章中，我们仍将关注 Alexa 及其一些最受欢迎的技能，但会与它的各种竞争对手进行比较。我们还将讨论包括 Alexa 在内的各种语音计算技术的未来。在本章中，我们将介绍以下主题：

- 流行的 Alexa 技能
- 比较各种基于语音的助手
- 基于语音的助手的未来

我们讨论这些主题是为了巩固目前所学到的信息，并集思广益，探讨语音计算的未来将会是什么样子。

### 流行的 Alexa 技能

在撰写本书时，美国市场上可用的 Alexa 技能总数已超过 30,000 个，而美国也是 Amazon Alexa 最大的市场，平均每天有 50 个新技能发布。每个技能都属于特定类别，Alexa 技能商店目前让开发者和用户可以从 23 个不同的大类中进行选择，如下面的屏幕截图所示：

![](img/80ddccf2572659c5b147f23cc6356c5a_224_0.png)

图 7.1：Alexa 技能——类别分布

在撰写本书时（2018 年年中），开发者中最受欢迎的类别（拥有技能最多的类别）是：

- **游戏与问答：** 拥有超过 6,000 项技能
- **新闻：** 拥有超过 3,000 项技能
- **音乐与音频：** 拥有超过 3,000 项技能
- **教育与参考：** 拥有超过 3,000 项技能
- **生活方式：** 拥有超过 2,000 项技能

