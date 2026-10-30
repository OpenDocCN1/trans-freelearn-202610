

+   5. 将 .mp4 格式的音频文件上传到输入文件夹。这将触发 Lambda 函数。根据代码，输出将以 JSON 格式存储在 S3 存储桶中，然后你可以使用它来读取文件的内容。

+   6. 导航到 CloudWatch > CloudWatch Logs > Log groups > aws/lambda/transcribe-lambda。从列表中选择最新的流。它看起来如下所示：

![](img/209e007237d34506e7ba16180f2e905c_64_0.png)

图 2.14 – CloudWatch 控制台中指定日志组的日志流中的日志

+   7. 输出以 JSON 格式保存到 S3 存储桶中，具体取决于代码中提到的 jobname（你可以使用 S3 getObject API 下载并读取它）：

![](img/209e007237d34506e7ba16180f2e905c_64_1.png)

图 2.15 – S3 存储桶中的输出 JSON 文件

**重要说明**
最好不要过度配置函数的超时设置。始终了解你的代码性能并据此设置函数超时。过度配置函数超时会导致 Lambda 函数运行时间更长，并造成意外的成本。如果你在使用 Lambda 函数中的异步 API 调用，那么最好将它们写入 SNS 主题以实现成功，并从中触发另一个 Lambda 函数。如果需要人工干预，则建议使用 AWS Step Functions。

在本节中，我们学习并应用了 Amazon Transcribe 将语音转换为文本。在下一节中，我们将学习一种最强大的 AWS AI 服务，我们可以用它从文本数据中获得最大量的洞察。

## 使用 Amazon Comprehend 实现自然语言处理

此服务可帮助你从非结构化文本中提取洞察。非结构化文本信息呈指数级增长。以下是一些数据源示例：

- *客户参与度：* 呼叫中心、问题分诊、客户调查和产品评论
- *业务流程：* 客户/供应商电子邮件、产品支持消息和运营支持反馈
- *记录和研究：* 白皮书和医疗记录
- *新闻和社交媒体：* 社交媒体分析、品牌趋势和相关事件

现在，问题是，我们可以用这些数据做什么？我们如何分析它并从中提取价值？答案是 Amazon Comprehend，它用于从非结构化数据中获取洞察。

Amazon Comprehend 的一些常见用途包括以下内容：

- 信息管理系统
- 对组织主题的更准确的搜索系统
- 用户情感分析
- 支持工单分类
- 从文档中检测语言，然后使用 Amazon Translate 将其翻译成英语
- 创建一个系统来标记非结构化临床数据以协助研究和分析目的
- 从公司会议或电视新闻的保存音频文件中提取主题

接下来，我们将探索 Amazon Comprehend 的优势。

## 探索 Amazon Comprehend 的优势

使用 Comprehend 的一些优势可以在以下图像中看到：

![](img/209e007237d34506e7ba16180f2e905c_66_0.png)

图 2.16 – 显示 Amazon Comprehend 功能的框图

让我们更详细地看一下这些内容：

- 它检测文本的语言并提取关键短语。Amazon Comprehend 也可用于情感分析和主题建模。
- Amazon Comprehend Medical 可用于提取医疗信息。
- 由于这是一项完全托管的服务，你按使用付费；你无需为基础设施付费。你无需训练、开发和部署自己的模型。
- 主题建模服务通过提取最多 100 个主题来工作。主题是一个关键词桶，这样你就可以看到实际文档语料库中的内容。
- 它准确、持续训练且易于使用。

接下来，我们将亲手操作 Amazon Comprehend。

## 亲手操作 Amazon Comprehend

在本节中，我们将构建一个管道，在其中可以将 AWS Lambda 与 Amazon Rekognition 和 Amazon Comprehend 集成。我们将读取存储在 S3 存储桶中的图像文件并检测从图像中提取的文本的语言。我们还将使用 CloudWatch 打印输出。以下是我们用例的图表：

![](img/209e007237d34506e7ba16180f2e905c_67_0.png)

图 2.17 – 所需用例的架构图

让我们开始创建 IAM 角色：

+   1. 导航到 IAM 控制台页面。
+   2. 从左侧菜单中选择 **Roles**。
+   3. 选择 **Create role**。
+   4. 选择 **Lambda** 作为受信任实体。
+   5. 添加以下托管策略：
    - `AmazonS3ReadOnlyAccess`
    - `AmazonRekognitionFullAccess`
    - `ComprehendFullAccess`
    - `CloudWatchFullAccess`
+   6. 将角色保存为 `language-detection-from-image-role`。
+   7. 现在，让我们创建 Lambda 函数。导航到 **Lambda > Functions > Create Function**。

8. 将函数命名为 `language-detection-from-image`。

9. 将运行时设置为 `Python 3.6`。

10. 使用我们现有的角色；即 `language-detection-from-image-role`。

11. 从 https://github.com/PacktPublishing/AWS-Certified-Machine-Learning-Specialty-MLS-C01-Certification-Guide/tree/master/Chapter-2/Amazon%20Transcribe%20Demo/lambda_function 下载代码，将其粘贴到函数中，然后单击 **Deploy**。

此代码将读取你上传的图像中的文本并检测文本的语言。我们使用了 Amazon Rekognition 的 `detect_text` API 从图像中检测文本，并使用了 Amazon Comprehend 的 `batch_detect_dominant_language` API 来检测文本的语言。

12. 现在，转到你的 AWS S3 控制台并创建一个名为 *language-detection-image* 的存储桶。

13. 创建一个名为 `input-image` 的文件夹（在此示例中，我们将只上传 `.jpg` 文件）。

14. 导航到 **Properties > Events> Add notification**。

15. 在 **Events** 部分填写以下信息中的必填字段；然后，单击 **Save**：

- **Name:** `image-upload-event`
- **Events:** `All object create events`
- **Prefix:** `input-image/`
- **Suffix:** `.jpg`
- **Send to:** `Lambda Function`
- **Lambda:** `language-detection-from-image`

16. 导航到 **Amazon S3>language-detection-image>input-image**。在文件夹中上传 `sign-image.jpg` 图像。（此文件可在本书的 GitHub 仓库 https://github.com/PacktPublishing/AWS-Certified-Machine-Learning-Specialty-MLS-C01-Certification-Guide/tree/master/Chapter-2/Amazon%20Comprehend%20Demo/input_image 中获得）。

17. 此文件上传将触发 lambda 函数。你可以从 CloudWatch> CloudWatch Logs> Log groups> /aws/lambda/language-detection-from-image 监控日志。

18. 单击流并选择最新的一个。检测到的语言会打印在日志中，如以下屏幕截图所示：

![](img/209e007237d34506e7ba16180f2e905c_69_0.png)

图 2.18 – 用于验证输出的 CloudWatch 中的日志

重要说明
建议你在生产环境中使用批量操作，例如 BatchDetectSentiment 或 BatchDetectDominantLanguage。这是因为单个 API 操作可能会导致 API 级别的节流。更多详细信息请参见此处：https://docs.aws.amazon.com/comprehend/latest/dg/functionality.html。

在本节中，我们学习了如何使用 Amazon Comprehend 检测文本的语言。文本是通过 Amazon Rekognition 提取到我们的 Lambda 函数中的。在下一节中，我们将学习如何通过 Amazon Translate 将同一文本翻译成英语。

## 使用 Amazon Translate 翻译文档

大多数时候，人们更喜欢用自己的语言交流，即使是在数字平台上。Amazon Translate 是一项文本翻译服务。我们可以提供各种语言的文档或文本字符串，并以不同的语言取回。它使用预训练的深度学习技术，因此我们不必担心模型，也不用担心它们的管理。我们可以发出 API 请求并取回结果。

Amazon Translate 的一些常见用途包括以下内容：

- 如果组织范围内需要准备不同语言的文档，则 Translate 是将一种语言转换为多种语言的解决方案。
- 在线聊天应用程序可以实时翻译，以提供更好的客户体验。
- 更快速、更经济地将网站内容本地化为更多语言。
- 一旦翻译完成，就可以在不同语言上应用情感分析。
- 为新闻发布网站提供非英语语言支持。

接下来，我们将探索 Amazon Translate 的优势。

## 探索 Amazon Translate 的优势

使用 Amazon Translate 的一些优势包括以下内容：

- 它使用神经机器翻译，这模仿了人类大脑的工作方式。
- 无需维护你的资源。
- 产生高质量的结果并保持其一致性。
- 你可以自定义你的品牌名称和模型名称，任何其他独特术语都可以使用自定义术语功能进行翻译。
- 可以通过 API轻松集成到应用程序中。
- Amazon Translate 在你需要它做更多事情时自行扩展。

接下来，我们将亲手操作 Amazon Translate。

