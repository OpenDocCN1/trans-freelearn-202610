

现在，让我们学习如何注册考试。

## 考试注册

UiPath Certified Professional 考试在全球 Pearson VUE 考试中心的安全受监考环境中提供，也可以通过 OnVUE 在您的私人位置进行远程受监考考试。

要安排考试，请访问 https://home.pearsonvue.com/uipath 进行注册、支付考试费用，并安排考试的日期和时间。如果您有考试代金券，可以在结账流程前输入代金券编号。

现在您应该已简要了解如何注册考试。下一步是了解如何在成功通过考试后管理您的认证凭证。

## 管理您的认证

您的所有考试和认证凭证详细信息都将保存在 UiPath Certification Manager (CertMetrics) 网站上，您可以在其中查看和下载电子证书和数字徽章。CertMetrics 位于 https://www.certmetrics.com/uipath。出于跟踪目的，每个电子证书都将包含一个唯一验证码。如有需要，您的雇主可以通过在 UiPath Certification Manager 验证页面 (https://www.certmetrics.com/uipath/public/verification.aspx) 上输入验证码来验证和确认您的凭证。

## 摘要

在本章中，您学习了如何准备、注册并参加 UiPath RPA Associate 考试。这包括考试详情、考试目标受众、考试部分和主题，以及您将遇到的问题类型。最后，您获得了其他考试详情，并学习了如何在成功通过考试后管理您的凭证。

如果您需要关于 UiPath Certified Professional 计划、其计划政策、考生协议以及常见问题解答 (FAQs) 的更多信息，请访问 https://www.uipath.com/learning/certification。

祝您顺利通过 UiPath RPA Associate 考试并获得 UiRPA 凭证！

在下一章中，您将有机会通过完成本书的第一个模拟考试来练习考试。

## 模拟考试 1

这是模拟考试 1。它用于测试您的知识，并让您了解实际考试的情况。此模拟考试不包含实际考试题目。如果您感觉已准备就绪，请不要查看答案直接参加此考试。如果在此考试中得分达到 70% 或以上，您可以确定自己已准备好参加实际考试，并且通过考试的机会非常大。

还在等什么？去试试吧：

1. 您可以使用 UiPath Assistant 中的 Sign In 功能配置哪种类型的机器人？
   - A. 无人值守机器人
   - B. 高密度机器人
   - C. 有值守机器人
   - D. 无人值守和有值守机器人

2. 您可以使用哪个属性来确保活动在失败后仍继续执行？
   - A. DelayAfter 属性
   - B. WaitForReady 属性
   - C. TimeoutMS 属性
   - D. ContinueOnError 属性

3. 哪个组合键可以让您从活动的属性字段自动创建变量？
   - A. Ctrl + A
   - B. Ctrl + K
   - C. Ctrl + N
   - D. Ctrl + P

4. 下列哪项最能定义部分选择器？
   - A. 它包含识别 UI 元素所需的所有元素，包括根节点。
   - B. 它由桌面记录器生成，且不包含根节点。
   - C. 它包含识别 UI 元素所需的所有元素，不包括根节点。
   - D. 它由基础记录器生成，且不包含根节点。

5. 一个名为 countries 的字符串数组已使用以下内容初始化：{"India", "Dubai", "Singapore", "Nepal", "Sweden", "Maldives"}。

变量面板如下：

| 名称 | 变量类型 | 作用域 | 默认值 |
|---|---|---|---|
| countries | String[] | Sequence | {"India","Dubai","Singapore","Nepal","Sweden","Maldives"} |

Main.xaml 工作流如下：

![](img/84b06ad2832c53c5f677f8ce4c80f184_519_0.png)

根据屏幕截图，**输出** 面板中由 **写入行** 活动打印的国家名称是哪些？

- A. India
  Dubai
  Singapore
  Nepal

- B. India

- C. India
  Dubai
  Singapore

- D. India
  Dubai
  Nepal
  Maldives

6. 一名开发者创建了一个包含以下内容的自动化流程：

![](img/84b06ad2832c53c5f677f8ce4c80f184_520_0.png)

以下是 **构建数据表** 活动的详细信息 – 数据表存储在名为 dt__emp 的变量中：

![](img/84b06ad2832c53c5f677f8ce4c80f184_520_1.png)

以下是筛选数据表活动的详细信息：

![](img/84b06ad2832c53c5f677f8ce4c80f184_521_0.png)

以下是写入范围活动的属性面板：

![](img/84b06ad2832c53c5f677f8ce4c80f184_521_1.png)

根据屏幕截图中显示的信息，写入范围活动的输出是什么？

- A.

| EmpName | EmpNo | EmpDept |
|---|---|---|
| Bob | 115 | IT |
| Alice | 125 | HR |
| Albert | 124 | IT |

- B.

| EmpName | EmpNo | EmpDept |
|---|---|---|
| Bob | 115 | IT |
| Alice | 125 | HR |
| Albert | 124 | IT |
| Jane | 112 | PS |
| Albert | 124 | IT |

- C.

| EmpName | EmpNo | EmpDept |
|---|---|---|
| Alice | 125 | HR |
| Albert | 124 | IT |

7. 您被指派构建自动化流程。它将从 Microsoft Outlook 帐户中读取未读电子邮件，其中主题行应始终为 How you doin'?。应将哪个筛选器表达式传递给 Get Outlook Mail Messages 活动的 Filter 属性？

- A. `[Subject] = 'How you doin'?'`
- B. `"[Subject] = 'How you doin'?'""`
- C. `Subject = How you doin'?`
- D. `"[Subject] = 'How you doin'?'"`

8. 在 UiPath Studio 中，用于从 UiPath Orchestrator 的 Integer 资产类型检索值的活动是哪个？
   - A. 设置资产
   - B. 设置凭据
   - C. 获取资产
   - D. 获取凭据

9. 发布包是什么意思？
   - A. 将所有工作流转换为 ZIP 文件
   - B. 将所有工作流转换为 txt 格式
   - C. 将所有工作流转换为 NUPKG 文件
   - D. 将所有工作流转换为 Git 文件

10. 哪个容器活动可检索当前活动窗口并使用户能在其中执行多个操作？
    - A. 附加窗口
    - B. 附加浏览器
    - C. 获取活动窗口
    - D. 打开浏览器

11. 哪个组合键可让您从活动的属性字段自动创建入参数？
    - A. Ctrl + A
    - B. Ctrl + M
    - C. Ctrl + N
    - D. Ctrl + P

12. 在诸如 Open Application 之类的容器中使用 Click 活动时，会生成哪种类型的选择器？
    - A. 动态选择器
    - B. 部分选择器
    - C. 完整选择器
    - D. 部分和完整选择器

13. 调试时，如何在特定活动之前暂停执行？
    - A. 使用 Break 活动
    - B. 使用 MessageBox 活动
    - C. 使用 Delay 活动
    - D. 在调试模式下使用断点

14. 以下表达式将返回什么值？
String.Format("Dear {0}, Welcome to the {1} club. You have been selected for the {3} club membership. Your user name is {0}123", "John", "RPA","elite","UiPath")
    - A. 亲爱的 John，欢迎加入 RPA 俱乐部。您已被选为 elite 俱乐部会员。您的用户名是 John123
    - B. 亲爱的 John，欢迎加入 RPA 俱乐部。您已被选为 elite 俱乐部会员。您的用户名是 UiPath123
    - C. 亲爱的 John，欢迎加入 RPA 俱乐部。您已被选为 elite 俱乐部会员。您的用户名是 John123
    - D. 亲爱的 John，欢迎加入 uiPath 俱乐部。您已被选为 uiPath 俱乐部会员。您的用户名是 John123

15. 一名开发者正在创建自动化流程，以从 PDF 文件中读取一系列页码。页码为 2、5、8、9、10、14 和 20。应如何配置 Read PDF Text 活动的 Range 属性，以便仅读取这些页码？
    - A. 将 Range 属性设置为 "2, 5, 8, 9, 10, 14, 20"
    - B. 将 Range 属性设置为 "2, 5, 8-10, 14 and 20"
    - C. 将 Range 属性设置为 "2, 5, 8-10, 14, 20"
    - D. 将 Range 属性设置为 "2, 5, 8-10, 14, 20-End"

16. 下列哪些信息不能存储在 Orchestrator 资产中？
    - A. 元素周期表
    - B. 电子邮件凭据
    - C. 电话号码
    - D. Studio 中 If 活动的输出

17. 哪个 UiPath 组件执行流程？
    - A. Robot
    - B. Studio
    - C. Orchestrator
    - D. 状态机

