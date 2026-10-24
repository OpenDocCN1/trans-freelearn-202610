

6. Twilio 后端将接收到的消息传递给其预期接收者，并向 Lambda 传递确认响应。
7. Lambda 接收到确认后，通过语音告知发送者短语 `Message sent.`。

正如您从系统功能描述中已了解到的，这些是我们 Handsfree Messenger 技能的主要组成部分：

- 实际的 Alexa 技能：Handsfree Messenger
- 位于 AWS 后端的技能 Lambda
- Twilio 后端

如果您将此技能与我们在上一章中创建的技能进行比较，那么由于它拥有更多组成部分，此技能会稍微复杂一些。

Handsfree Messenger 的 VUI 流程如下：

![](img/80ddccf2572659c5b147f23cc6356c5a_69_0.png)

VUI 流程使用 Amazon 首选的模板来描述 Alexa 技能的 VUI 设计。然而，VUI 流程仅限于用户与 Alexa 的交互，因此，也许以图表形式概述功能步骤可以帮助您更好地理解 Handsfree Messenger 技能的工作原理：

![](img/80ddccf2572659c5b147f23cc6356c5a_70_0.png)

上图概述了技能的基本使用方式；我们还可以将步骤压缩成流程图，以明确系统中信息的流向：

![](img/80ddccf2572659c5b147f23cc6356c5a_71_0.png)

我们希望读者现在已经清楚我们的 HandsFree Messenger 技能的工作原理以及其所有组成部分如何交互。

在下一节中，我们将开始配置技能的组成部分之一，即我们的 Twilio 后端。

## 配置 Twilio 消息后端

Twilio 是一个云通信平台，成立于 2007 年，总部位于旧金山。它允许用户/开发者以编程方式拨打/接听电话，并使用网络服务 API 发送/接收短信。本章中，我们仅演示其消息功能；不过，我们要求并期望读者通过添加电话呼叫功能作为家庭作业练习来扩展此技能的功能。

要将 Twilio 配置为我们的消息后端，我们将遵循以下步骤：

- 1. 请在您的网络浏览器中导航到 `www.twilio.com`，如本截图所示：

![](img/80ddccf2572659c5b147f23cc6356c5a_72_0.png)

2. 请点击 **Get a free API key** 或 **Sign up**（两者都会启动新用户的注册流程）以进入此处所示的 **Sign up for free** 页面：

![](img/80ddccf2572659c5b147f23cc6356c5a_73_0.png)

图 3.5：Twilio 注册页面

3. 请填写您的个人详细信息。如前一截图所示，页面底部有一个小问卷，围绕 Twilio 服务的使用构建。请为该问卷选择以下详细信息：

![](img/80ddccf2572659c5b147f23cc6356c5a_74_0.png)

图 3.6：Twilio - 注册时填写个人详细信息

4. 填写完详细信息后，请选择页面底部的 Get Started 按钮。这应将您带到此验证页面：

![](img/80ddccf2572659c5b147f23cc6356c5a_75_0.png)

图 3.7：Twilio - 验证流程

请选择适当的国家/地区并在提供的空间中输入您的电话号码，然后点击 Verify via SMS，之后您应该会在您提供的电话号码上收到一条包含验证码的短信。

5. 点击 **Verify via SMS** 应将您带到下一个屏幕，您可以在其中输入验证码：

![](img/80ddccf2572659c5b147f23cc6356c5a_76_0.png)

图 3.8：Twilio - 验证流程

请在此屏幕上输入验证码并点击 **Submit**。

6. 如果您成功验证了短信验证码，则应看到项目详细信息屏幕，我们将在此处为项目提供名称，如下所示：

![](img/80ddccf2572659c5b147f23cc6356c5a_77_0.png)

图 3.9：Twilio ~ 为您的项目命名

请将技能命名为 `AlexaHandsFreeSkill` 并点击 **Create Project**。

7. 点击 **Create Project** 按钮应将我们带到此仪表板：

![](img/80ddccf2572659c5b147f23cc6356c5a_77_1.png)

图 3.10：Twilio -- 仪表板

仪表板上有一些需要注意的地方：

- 我们正在使用试用账户，对于本技能而言这是可以的。
- 我们需要获取一个 Twilio 号码。

首先，让我们尝试理解为什么我们需要一个 Twilio 号码。

Twilio 的工作方式如下——开发者最初请求一个 Twilio 号码，当他们使用 Twilio API 发送短信或拨打电话时，这些短信和电话都是从此预先请求的号码生成的。每个生成的 Twilio 号码都与您在第 2 步提供的详细信息中提供的国家/地区及其通信法规相关联。

8. 请点击第 7 步所示的仪表板屏幕上的 Get a number 按钮。这应根据您在第 2 步提供的国家/地区详细信息生成一个电话号码：

![](img/80ddccf2572659c5b147f23cc6356c5a_78_0.png)

图 3.11：获取 Twilio 号码

我们的技能将使用生成的号码通过 Twilio API 调用发送短信。

9. 当您在上一步中向用户展示的 Twilio 电话号码对话框中选择 Choose this Number 时，根据国家/地区，用户可能会被要求提供额外详细信息，如以下截图所示：

### Address Required

由于当地法规，您或您的客户必须拥有任何国家的商业或个人地址才能购买 +31 [redacted]

### Enter an Address

FRIENDLY NAME
地址的名称
CUSTOMER NAME
企业或客户名称
ADDRESS 1
街道地址、邮政信箱、套房、单元等。
ADDRESS 2
附加街道地址、邮政信箱、套房、单元等。
CITY
STATE/PROVINCE/REGION
POSTAL CODE
COUNTRY
Netherlands

Go back to your preselected number

Cancel

Save and continue

图 3.12：Twilio：号码的地址要求

请注意，此处显示的对话框取决于用户在入职流程中选择的国家/地区，具体取决于国家/地区，您可能看到也可能看不到此对话框。如果您看到此对话框，请提供必要详细信息并点击对话框底部的 Save and continue 按钮。

10. 点击 Save and continue 按钮后，我们应看到此对话框：

![](img/80ddccf2572659c5b147f23cc6356c5a_80_0.png)

图 3.13：最终获取 Twilio 号码

号码选择过程已完成，我们现在已成功选择了一个用于通过 Twilio API 调用发送短信的电话号码。请点击此对话框中的 Done 按钮，您应再次返回仪表板。

11. 一旦您回到仪表板，底部应有 Get Started with SMS 部分，其中包含 Get Started 按钮：

![](img/80ddccf2572659c5b147f23cc6356c5a_80_1.png)

图 3.14：Twilio - 开始使用可编程短信

在此部分下，请从下拉菜单中选择 Programmable SMS 并点击 Get Started 按钮。

12. 点击 Get Started 按钮后，您应进入 Send a Message 页面：

![](img/80ddccf2572659c5b147f23cc6356c5a_81_0.png)

图 3.15：Twilio - 发送消息功能的测试设置

请注意，我们需要从此页面获取三项重要信息：

- SID：标记在前一截图中
- Auth Token：如果您勾选标记为 Show your Auth Token 的复选框，Auth Token 将变得可见
- Twilio API URL：这在 curl 命令之后提及

请记下 SID 和 Auth Token，因为我们在 Lambda 代码中需要它们。记下这些详细信息后，请关闭此页面。

我们已成功配置 Twilio 后端，这将帮助我们使用 Alexa 技能发送短信。在下一节中，我们将通过 Amazon Developer 控制台配置我们的 Handsfree Messenger 技能。

