

![](img/80ddccf2572659c5b147f23cc6356c5a_126_0.png)

图 4.28：在 AWS 控制台中导航到 IAM

请选择 IAM 以导航到 IAM 仪表板。

- 2. 进入 IAM 仪表板后，请从左侧菜单中选择 Roles：

![](img/80ddccf2572659c5b147f23cc6356c5a_126_1.png)

图 4.29：IAM 角色

在 **Roles** 屏幕上，你应该有一个预先存在的 **lambda_basic_execution** 角色，因为我们一直在使用它作为所有 Lambda 函数。点击它；我们将用一个策略对其进行增强，以便使用此角色访问 DynamoDB。这样，我们就不需要进行任何额外的配置更改（例如，从头创建新角色）。

- 3. 在下一个屏幕中，点击 **Attach policy** 按钮：

![](img/80ddccf2572659c5b147f23cc6356c5a_127_0.png)

图 4.30：附加策略

你将进入策略列表。使用 DynamoDB 搜索以查找访问 DynamoDB 的可用策略列表：

![](img/80ddccf2572659c5b147f23cc6356c5a_128_0.png)

图 4.31：为 DynamoDB 访问选择正确的策略

选择 **AmazonDynamoDBFullAccess** 策略。最后，点击屏幕底部的 **Attach policy** 按钮。

现在，我们的 Lambda 函数应该能够访问 DynamoDB。接下来，我们将在 DynamoDB 中创建一个表来存储启动计数器值。

- 4. 现在，导航到 https://console.aws.amazon.com 再次，并从提供的服务列表中选择 DynamoDB。如果找不到它，只需在提供的搜索框中搜索即可：

![](img/80ddccf2572659c5b147f23cc6356c5a_129_0.png)

图 4.32：导航到 DynamoDB

选择 DynamoDB 以导航到 DynamoDB 仪表板。

- 5. 在 DynamoDB 仪表板上，点击 Create table 按钮以进入 Create DynamoDB table 屏幕：

![](img/80ddccf2572659c5b147f23cc6356c5a_130_0.png)

图 4.33：创建 DynamoDB 表

如前面的屏幕截图所示，提供表名 `factlylaunchcount` 和主键 `count`。完成后点击 Create 按钮。

- 6. 点击 Create 应该在短暂延迟后创建表，并应显示 `factlylaunchcount` 表的详细信息屏幕：

![](img/80ddccf2572659c5b147f23cc6356c5a_130_1.png)

图 4.34：DynamoDB 表配置

表默认配置为每秒 5 次读写，这对于我们的开发目的来说是可以的，但在扩展技能时应进行更新。

我们现在需要添加实际的启动计数器变量，它将存储我们的启动计数。点击前一个屏幕截图所示的 Items 选项卡。

- 7. 在 Items 选项卡下，点击 Create item 按钮以调出 Create item 窗口：

![](img/80ddccf2572659c5b147f23cc6356c5a_131_0.png)

图 4.35：向 DynamoDB 表添加项目/键

在 Create item 窗口中，提供 `count` 主键，其值为 `counter`，如前一个屏幕截图所示。

- 8. 接下来，在同一窗口中，选择 **+** 按钮以插入一个新列到表中，其类型为 Number，如以下屏幕截图所示：

![](img/80ddccf2572659c5b147f23cc6356c5a_132_0.png)

图 4.36：向 DynamoDB 表添加新列

请将此新列命名为 `launchcounter` 并将其初始值设为 0：

![](img/80ddccf2572659c5b147f23cc6356c5a_132_1.png)

图 4.37：在 DynamoDB 表中命名新列

完成后点击 **Create item** 窗口底部的 **Save** 按钮。`factlylaunchcount` 表模式现在应自动更新以反映我们刚刚所做的更改：

![](img/80ddccf2572659c5b147f23cc6356c5a_133_0.png)

图 4.38：DynamoDB 表中反映的更改

我们现在可以开始更新我们的 Lambda 代码，以便它可以持久化启动计数器。导航回我们的 Lambda 代码，以便我们可以更新它以包含启动计数器的持久化。

## 更新 Factly Lambda 代码以引入持久化

我们将通过首先包含 AWS 依赖项来开始修改 Lambda 代码：

- 1. 在 Lambda 代码（`index.js` 文件）非常开头添加以下代码行：

```
var AWS = require('aws-sdk');
var docClient = new AWS.DynamoDB.DocumentClient();
```

这里，我们包含 AWS SDK，然后使用它来获取 `DynamoDB.DocumentClient` 的实例，这将帮助我们在 DynamoDB 中读写数据。

让我们分解前面的代码片段以了解我们在做什么。因此，更早的时候，当我们启动应用程序时，我们通过 getWelcomeResponse 方法向用户提供了一个硬编码的响应。但是，随着持久化的引入，情况有点不同了。我们首先定义一些参数，即 `paramsGet`，以从表中获取数据。这些参数包括以下内容：

- 从中获取数据的 `TableName`
- 基于 `Key` 要获取的特定项目；我们本质上是在说“请从 `factlylaunchcount` 表中获取主键为 `counter` 的项目”

一旦项目被获取，我们就从该项目中提取 `launchcounter` 的值，这发生在以下这一行：

```
var initialLaunchCount = data.Item.launchcounter;
```

接下来，我们将启动计数加 1 并通过以下代码中的调用将其更新到 `factlylaunchcount` 表中：

```
docClient.update(paramsUpdate, function(err, data)
```

在更新调用之后，我们将 `initialLaunchCount` 传递给 `getWelcomeResponse` 函数。

- 3. 接下来，剩下的就是将 `getWelcomeResponse` 函数的签名从此：

```
function getWelcomeResponse(callback)
```

更新为以下代码所示的内容：

```
function getWelcomeResponse(callback, count)
```

我们现在可以从以下代码所示的静态响应更新 `getWelcomeResponse` 函数中的静态响应：

```
speechOutput = "Factly will ask you " + GAME_LENGTH.toString()
            + " questions, try to get as many right as you can. Just say the number of the answer. Let's begin. "
```

我们将其更新为考虑 count 变量的动态响应：

```
speechOutput = "You have played Factly " + count.toString() +
    " times. Factly will ask you " + GAME_LENGTH.toString() +
    " questions, try to get as many right as you can. " +
    "Just say the number of the answer. Let's begin. ";
```

现在，Factly Skill 将能够记住用户玩过多少次，并能够通知用户此信息。

请在以下 URL 找到专门包含持久化的 Lambda 代码：https://github.com/madhurbhargava/AlexaSkillsProjects/blob/master/Chapter4_Factly_Persistence_index.js。

我们的 Lambda 代码现在已完成，现在是测试它的时候了。

## 测试技能

我们的技能现在已完成，我们准备好测试它。技能可以通过实际的 Amazon Echo 设备或通过 Amazon Service Simulator 进行测试。我们将首先通过 Amazon Service Simulator 测试我们的技能，然后通过实际的 Amazon Echo 设备进行测试。

在 Test 页面上，向下滚动到 Service Simulator 并输入 "Alexa, Launch Factly" 文本，如以下屏幕截图所示：

![](img/80ddccf2572659c5b147f23cc6356c5a_136_0.png)

图 4.39：通过 Amazon Service Simulator 测试 Factly Skill，第 1 步

如果你仔细观察 Alexa 的响应，你会发现它已经包含了启动计数，它已从 DynamoDB 中获取，并附带了响应启动短语和第一个问题。

然后，你只需输入一个答案编号（例如，4），测试模拟器就会告诉你答案是否正确，以及下一个问题，如以下屏幕截图所示：

![](img/80ddccf2572659c5b147f23cc6356c5a_137_0.png)

或者，你也可以使用 Amazon Echo 测试技能，前提是 Echo 已连接并设置好：

- 如果你说：“Alexa, launch Factly”，它应该回复：“You have played Factly *n* times. Factly will ask you three questions, try to get as many right as you can. Just say the number of the answer. Let's begin. Question 1. World War I began in which year? 1. 1923; 2. 1938; 3. 1917; 4. 1914.”

