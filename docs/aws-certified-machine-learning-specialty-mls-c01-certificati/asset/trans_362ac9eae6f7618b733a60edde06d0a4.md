

name_of_the_bucket=event['Records'][0]['s3']['bucket']['name']
# 从事件中读取对象
name_of_the_doc=event['Records'][0]['s3']['object']['key']
print(name_of_the_bucket)
print(name_of_the_doc)
response =
textract_client.analyze_document(Document={'S3Object': {'Bucket': name_of_the_bucket,'Name': name_of_the_doc}},FeatureTypes=["TABLES","FORMS"])
print(str(response))
doc=Document(response)
for page in doc.pages:
    # 打印表格
    for table in page.tables:
        for r, row in enumerate(table.rows):
            for c, cell in enumerate(row.cells):
                print("Table[{}][{}] = {}".format(r, c, cell.text))
for page in doc.pages:
    # 打印字段
    print("Fields:")
    for field in page.form.fields:
        print("Key: {}, Value: {}".format(field.key, field.value))

与前面的示例不同，我们将创建一个测试配置来运行代码。

+   5. 单击“测试”按钮左侧的下拉菜单。
+   6. 选择“配置测试事件”，然后选择“创建新的测试事件”。
+   7. 在“事件模板”下拉菜单中选择“Amazon S3 Put”。
+   8. 在 JSON 正文中，根据存储桶名称和键修改高亮显示的值，如下所示：

"x-amz-id-2": "EXAMPLE123/5678abcdefghijklambdaisawesome/" }, "s3": { "s3SchemaVersion": "1.0", "configurationId": "testConfigRule", "bucket": { "name": "textract-document-analysis", "ownerIdentity": { "principalId": "EXAMPLE" }, "arn": "arn:aws:s3:::textract-document-analysis" }, "object": { "key": "receipt.png", "size": 1024, "eTag": "0123456789abcdef0123456789abcdef", "sequencer": "0A1B2C3D4E5F678901" }

图 2.24——用于测试 Lambda 函数的事件模板

9. 在“事件名称”字段中，将测试配置命名为 TextractDemo。

10. 单击“保存”。

11. 选择测试配置（TextractDemo），然后单击“测试”：

![](img/209e007237d34506e7ba16180f2e905c_79_0.png)

图 2.25——运行测试前选择测试配置

12. 这将触发 Lambda 函数。你可以通过 CloudWatch > CloudWatch Logs > Log groups > /aws/lambda/ read-scanned-doc 监控日志。

13. 单击流并选择最新的流。其显示效果如下；以下屏幕截图中可以看到键值对：

![](img/209e007237d34506e7ba16180f2e905c_80_0.png)

图 2.26——CloudWatch 中用于验证输出的日志

## 重要提示

通过生成 S3 Put 事件，借助 Lambda 异步调用下游 AI 工作流，最具可扩展性且最具成本效益的方法是生成 AWS 预签名 URL，然后将其提供给移动应用或 Web 应用的用户。通过这种方式可以同时服务许多用户，并且还可能提升性能和吞吐量。

让 AWS AI 服务与 S3 存储桶位于同一区域，可能有助于提高性能并减少网络延迟。AWS VPC 端点可以在不使用公共互联网的情况下增强安全性。你可以将 AWS AI 处理结果存储在 AWS S3 存储桶中，并对其余数据进行加密，以获得更高的安全性。

在本节中，我们学习了如何从扫描文档中提取文本并打印其中的表单数据。与其他章节不同，我们使用 Lambda 函数的测试功能，创建了一个包含事件模板的测试配置。在下一节中，我们将学习如何为组织创建聊天机器人以及如何使用它。

## 在 Amazon Lex 上创建聊天机器人

Alexa 中提供的大多数功能都由**Amazon Lex**提供支持。你可以使用 Amazon Lex 轻松构建聊天机器人。它在后台使用自然语言理解和自动语音识别技术。通过 SLU，Amazon Lex 接收自然语言语音和文本输入，理解用户意图，并满足用户的意图。Amazon Lex 机器人既可以通过控制台创建，也可以通过 API 创建。其基本要求如下图所示。

Amazon Lex 的一些常见用途包括：

- 同时能够监听并接收文本输入的应用。
- 聊天机器人。
- 用于提供更佳客户和销售体验的对话式 AI 产品。
- 通过 AWS Lambda 函数提供辅助功能的定制业务机器人。
- 用于呼叫中心的语音助手，可以与用户交谈、安排会议或请求账户详细信息。
- 通过与 Amazon Cognito 集成，你可以控制用户管理、身份验证以及跨所有设备的同步。

接下来，我们将探索 Amazon Lex 的优势。

## 探索 Amazon Lex 的优势

使用 Lex 的一些原因包括：

- 可以直接从 AWS 管理控制台构建和测试聊天机器人。这些聊天机器人可以通过其丰富的格式功能轻松集成到 Facebook Messenger、Slack 和 Twilio SMS 中。
- 可以将对话日志存储在 Amazon CloudWatch 中，以便进一步分析。你可以使用这些日志监控机器人并获取洞察，从而改善用户体验。
- Amazon Lex 可以集成到其他 AWS 服务中，例如 Amazon Cognito、AWS Lambda、Amazon DynamoDB、Amazon CloudWatch 和 AWS Mobile Hub，从而利用 AWS 平台中的应用程序安全性、监控、用户身份验证、业务逻辑、存储和移动应用开发功能。
- Amazon Lex 聊天机器人也可以集成到自定义 Web 应用程序中。你只需构建一个聊天机器人小组件并将其集成到用户界面中。

接下来，我们将实际操作 Amazon Lex。

## 实际操作 Amazon Lex

让我们开始吧：

+   1. 登录 https://console.aws.amazon.com/lex/。
+   2. 单击**开始使用**，然后选择**自定义机器人**。
+   3. 填写以下详细信息，然后单击**创建**：

![](img/209e007237d34506e7ba16180f2e905c_82_0.png)

图 2.27——Amazon Lex 的“创建”对话框

+   4. 单击**创建意图**。此时将出现一个对话框。选择**创建意图**。
+   5. 将新意图命名为`MovieIntent`，然后单击**添加**。
+   6. 转到**槽位**部分，并使用以下详细信息：
    - 名称：`movie_type`
    - 槽位类型：`AMAZON.Genre`
    - 提示语：`Which movie do you like?`
+   7. 单击**设置**中的 +。

以下屏幕截图展示了一些示例话语。在此示例中，movie_type 是我的变量：

![](img/209e007237d34506e7ba16180f2e905c_83_0.png)

图 2.28——“示例话语”部分

8. 向下滚动到“响应”部分以添加消息：

![](img/209e007237d34506e7ba16180f2e905c_83_1.png)

图 2.29——Amazon Lex 的“响应”部分

9. 向下滚动到“保存意图”，然后单击“构建”。提示语构建成功后，将显示以下成功消息：

![](img/209e007237d34506e7ba16180f2e905c_84_0.png)

图 2.30——Amazon Lex 的“响应”部分

10. 现在，你可以测试机器人，如下图所示：

![](img/209e007237d34506e7ba16180f2e905c_84_1.png)

图 2.31——“测试机器人”对话框

**重要提示**
并非所有 Amazon Polly 功能都可在 Alexa 中使用，尤其是 Amazon Polly 的 SSML 功能，这使得 Amazon Polly 与 Alexa 存在差异。

本章关于各种可用 AWS 应用服务的介绍到此结束。

## 总结

在本章中，我们了解了一些可用于解决各种问题的 AWS AI 服务。我们使用了 Amazon Rekognition 服务，它可以检测对象和面部（包括名人面部），还可以从图像中提取文本。在文本转语音方面，我们使用了 Amazon Polly；在语音转文本方面，我们使用了 Amazon Transcribe。在本章末尾，我们使用 Amazon Lex 构建了一个聊天机器人。

对于图像中的语言检测和翻译，我们使用了 Amazon Rekognition、Amazon Comprehend 和 Amazon Translate。我们学习了如何将它们全部组合到一个 Lambda 函数中来解决问题。

对于认证考试，你不需要记住本章中使用的所有 API。考试可能会考查我们学到的一些最佳实践，或者考查能够解决特定问题的服务名称。练习使用这些 AWS AI 服务总是有益的，因为这将提升你的架构设计能力。

下一章中，我们将学习数据准备和转换，这是机器学习中最重要的方面。

## 问题

+   1. 使用预定义的逻辑和规则向在线购物者提供产品推荐，是机器学习的一个示例。
    a. 正确
    b. 错误
+   2. ML 技术栈中的哪个层级可以帮助你构建自定义 ML 模型，而无需管理基础设施？
    a. 顶层（AI 服务）
    b. 中间层（Amazon SageMaker）
    c. 底层（ML 框架和基础设施）
    d. 自有基础设施和代码层

