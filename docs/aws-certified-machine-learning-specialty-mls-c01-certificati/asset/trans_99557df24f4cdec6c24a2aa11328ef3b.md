

![](img/209e007237d34506e7ba16180f2e905c_49_0.png)

Figure 2.1 – The Create role dialog

接下来，我们将创建一个 Lambda 函数。

+   7. 导航到 AWS Lambda 控制台页面。
+   8. 选择 **Create function**。
+   9. 创建一个函数：
+      - 选择 **Author from scratch**。
+      - 为函数命名，例如 `lambda-rekognition`。
+      - 从 **Runtime** 下拉菜单中选择 Python 3.6。
+      - 选择 **Use an existing role**。添加你之前创建的角色名称；即 `rekognition-lambda-role`：

![](img/209e007237d34506e7ba16180f2e905c_50_0.png)

Figure 2.2 – Creating the Lambda function

10. 在 lambda_function.py 中输入以下代码：

```
from __future__ import print_function
import boto3
def lambda_handler(event, context):
    print("======lambda_handler started======")
    # read the bucket name (key) from the event
    name_of_the_bucket=event['Records'][0]['s3']['bucket']['name']
    # read the object from the event
    name_of_the_photo=event['Records'][0]['s3']['object']['key']
    detect_labels(name_of_the_photo,name_of_the_bucket)
    print("Labels detected Successfully")
def detect_labels(photo, bucket):
    client=boto3.client('rekognition')
    response=client.detect_labels(Image={'S3Object':{'Bucket':bucket,'Name':photo}})
    print('Detected labels for ' + photo)
```

```
print('============================')
    for label in response['Labels']:
        print ("Label: " + label['Name'])
        print ("Confidence: " +
str(label['Confidence']))
        print ("Instances:")
        for instance in label['Instances']:
            print ("  Bounding box")
            print ("Top:
"+str(instance['BoundingBox']['Top']))
            print ("Left: \
"+str(instance['BoundingBox']['Left']))
            print ("Width: \
"+str(instance['BoundingBox']['Width']))
            print ("Height: \
"+str(instance['BoundingBox']['Height']))
            print ("Confidence:
"+str(instance['Confidence']))
            print()
        print ("Parents:")
        for parent in label['Parents']:
            print ("   " + parent['Name'])
        print ("----------")
        print('============================')
    return response
```

现在，我们将为 Lambda 函数创建一个触发器。

11. 导航到 AWS S3 控制台页面。创建一个存储桶，例如 rekognition-test-baba，如以下屏幕截图所示：

![](img/209e007237d34506e7ba16180f2e905c_52_0.png)

Figure 2.3 – AWS S3 console page

12. 点击 Create folder 并将其命名为 images。点击 Save。

13. 点击我们存储桶的 Properties 选项卡。

14. 滚动到该存储桶的 Events。

15. 在 Events 窗口中，选择 Add notification 并设置以下属性：

+   - Name: rekognition_event
+   - Events: All object create events
+   - Prefix: images/
+   - Send to: Lambda Function
+   - Lambda: lambda-rekognition:

![](img/209e007237d34506e7ba16180f2e905c_53_0.png)

Figure 2.4 – S3 bucket Events window

接下来，我们将从共享的 GitHub 仓库上传图像到 S3 存储桶 images 文件夹。

16. 一旦上传完成，你可以在 Lambda 控制台的 Monitoring 选项卡中检查事件，如以下屏幕截图所示：

![](img/209e007237d34506e7ba16180f2e905c_53_1.png)

Figure 2.5 – CloudWatch monitoring the event in the Lambda console

17. 导航到 CloudWatch > CloudWatch Logs > Log groups > /aws/lambda/ lambda-rekognition。选择所有流中的最新流并向下滚动日志以查看输出，如以下屏幕截图所示：

![](img/209e007237d34506e7ba16180f2e905c_54_0.png)

Figure 2.6 – CloudWatch logs

在本节中，我们学习了如何实现 Amazon Rekognition AI 服务来检测图像中的对象并为每个对象提供置信度分数。我们将在后续章节中看到 Amazon Rekognition 的更多用例，当我们检测图像中的文本时。在下一节中，我们将学习 Amazon 的文本转语音服务并实现它。

## Text to speech with Amazon Polly

Amazon Polly 负责将文本转换为语音，并使用预训练的深度学习模型来实现。它是一个完全托管的服务，因此我们无需做任何事情。你提供纯文本作为合成的输入，或以语音合成标记语言 Speech Synthesis Markup Language (SSML) 格式提供，以便返回音频流。它还提供多种语言和语音选择，包括男性和女性选项。Amazon Polly 的输出音频可以以 MP3 格式保存以供应用程序（Web 或移动）进一步使用，也可以是用于书面语音的 JSON 输出。

例如，如果我们将文本 “Baba went to the library” 输入到 Amazon Polly，输出语音标记对象将如下所示：

```
{"time":370,"type":"word","start":5,"end":9,"value":"went"}
```

单词 “went” 在音频流开始后 370 毫秒开始，并在给定的输入文本的字节 5 开始、字节 9 结束。

它还以 `ogg_vorbis` 和 `pcm` 格式返回输出。当使用 `pcm` 时，返回的内容是 audio/pcm，采用有符号 16 位、1 通道（单声道）、小端格式。

Amazon Polly 的一些常见用途如下：

+   - 可用作阅读网页内容的辅助工具。
+   - 可与 Amazon Rekognition 集成以帮助视障人士阅读标志。你可以用文字拍照并将其馈送到 Amazon Rekognition 以提取文本。输出文本可作为 Polly 的输入，它将返回语音输出。
+   - 可用于公共广播系统，管理员团队只需传递要广播的文本，Amazon Polly 即可完成其余工作。
+   - 通过将 Amazon Polly 与 **Amazon Connect**（电话后端服务）结合，你可以构建 **audio/video receiver (AVR)** 系统。
+   - 诸如智能电视、智能手表和 **Internet of Things (IoT)** 设备等智能设备可将其用于音频输出。
+   - 旁白生成。
+   - 与 Amazon Lex 结合时，可为应用程序开发功能齐全的语音用户界面。

现在，让我们探索 Amazon Polly 的优势。

## Exploring the benefits of Amazon Polly

使用 Amazon Polly 的一些优势如下：

+   - 该服务是完全托管的，不需要任何管理成本来维护或管理资源。
+   - 它提供即时语音校正和增强功能。
+   - 你可以使用 Amazon Polly 的 HTTP API 开发自己的访问层。由于支持大量语言，如 Python、Ruby、Go、C++、Java 和 Node.js，开发变得简单。
+   - 对于某些神经语音，可以使用 Newscaster 风格进行语音合成，使其听起来像电视或广播播音员。
+   - Amazon Polly 还允许你修改特定单词的发音或使用新单词。

接下来，我们将亲手操作 Amazon Polly。

## Getting hands-on with Amazon Polly

在本节中，我们将构建一个管道，将 AWS Lambda 与 Amazon Polly 集成以读取文本文件，然后将相同的 MP3 文件生成到同一存储桶的另一个文件夹中。我们将在 CloudWatch 日志中监控任务进度。

我们将首先为 Lambda 创建一个 IAM 角色。让我们开始：

+   1. 导航到 IAM 控制台页面。
+   2. 从左侧菜单选择 Roles。
+   3. 选择 Create role。
+   4. 选择 Lambda 作为受信任实体。
+   5. 添加以下托管策略：
+      - AmazonS3FullAccess
+      - AmazonPollyFullAccess
+      - CloudWatchFullAccess
+   6. 将角色保存为 polly-lambda-role。

接下来，我们将创建一个 Lambda 函数：

+   1. 导航到 Lambda > Functions > Create Function。
+      - 将函数命名为 polly-lambda
+      - 将运行时设置为 python 3.6。
+      - 使用现有角色；即 polly-lambda-role
+   2. 将代码粘贴到 https://github.com/PacktPublishing/AWS-Certified-Machine-Learning-Specialty-MLS-C01-Certification-Guide/tree/master/Chapter-2/Amazon%20Rekognition%20Demo/lambda_code 中到你的 lambda 函数中，并在 CloudWatch 控制台中检查其进度。我们将对该代码使用 Amazon Polly 的 start_speech_synthesis_task API；它是一个异步合成任务。
+   3. 向下滚动并在 Basic Settings 部分将 Timeout 更改为 59 秒，如以下屏幕截图所示，然后点击 Save：

> **Important note**
> 默认值为 3 秒。由于这是一个异步操作，任何重试尝试都会创建更多文件。

