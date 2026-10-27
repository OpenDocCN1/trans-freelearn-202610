
3. 在此处，点击**创建文件夹**按钮，并在**文件夹名称**字段中输入 `lookout-for-metrics`。然后，点击此页面底部的**创建文件夹**按钮。

**有关创建文件夹的信息**

使用文件夹对桶中的对象进行分组。创建文件夹时，S3 会使用您指定的名称后跟一个斜杠 (/) 创建一个对象。该对象会作为文件夹显示在控制台中。了解更多

> 您的桶策略可能会阻止文件夹的创建
> 如果您的桶策略阻止上传没有特定标签、元数据或访问控制列表 (ACL) 授权人的对象，那么您将无法使用此配置创建文件夹。相反，您可以使用上传配置上传一个空文件夹，并指定相应的设置。

**文件夹**

文件夹名称

`lookout-for-metrics /`

文件夹名称不能包含 `"/*"`。查看命名规则

**服务端加密**

> 以下设置仅适用于新文件夹对象，而不适用于其中包含的对象。

服务端加密

- 禁用
- 启用

取消 创建文件夹

*图 14.4 – 创建新文件夹以托管 Amazon Lookout for Metrics 数据*

4. 您将被带回桶的对象列表页面。点击刚刚创建的文件夹名称。然后，点击对象列表顶部的**上传**按钮。您将进入上传页面，可以点击**添加文件**按钮，也可以将之前下载的压缩包解压出的 CSV 文件拖放到此处。

![](img/e4ee48104c730845e796d8c469dfeecc_398_0.png)

*图 14.5 – 在 Amazon S3 上上传电子商务数据集*

5. 将所有其他字段保留为默认值，滚动到屏幕底部，然后点击**上传**开始上传文件。总共有 12.5 MB 和 6,553 个文件要上传。根据您的互联网上传带宽，这可能需要一段时间。当传输进行时，会显示一个上传状态页面。

> **注意**
> 如果您觉得使用浏览器传输 6,553 个文件太慢，我建议使用 **AWS 命令行界面**来优化您的上传速度：
> `https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html`。另一种选择是使用 **AWS CloudShell**，在您连接到控制台的同时，在浏览器中打开命令行：
> `https://docs.aws.amazon.com/cloudshell/latest/userguide/welcome.html`。

6. 完成后，您可以点击**退出**按钮返回到新创建文件夹根部的对象列表。

上传完成后，您应该在对象列表中看到两个子文件夹，一个叫 `backtest`，另一个叫 `live`。

## 授予 Amazon Lookout for Metrics 访问您的数据的权限

默认情况下，不同 AWS 服务之间强制实施的安全机制会禁止除 Amazon S3 以外的任何服务访问您的数据。在您的账户中，您可以上传、删除或移动刚刚创建的桶中的数据。但是，Amazon Lookout for Metrics 是一项不同的服务，无法访问这些数据。我们需要指定 Amazon Lookout for Metrics 可以访问此桶中的任何数据。

> **注意**
> 您可以在摄取步骤中直接从 Amazon Lookout for Metrics 控制台配置此访问权限。但是，如果您希望更多地控制账户中创建的角色和各种访问权限，可以通读本节。否则，您可以跳过它，稍后再回来。

为了启用 Amazon Lookout for Metrics 服务对您的 S3 桶的访问，我们将使用 **AWS 身份与访问管理 (IAM)** 服务按如下方式创建专用 IAM 角色：

1. 在控制台左上角，点击**服务**下拉菜单，并搜索**安全、身份与合规**部分。点击 IAM 服务名称以进入该服务控制台。

2. 在左侧面板菜单中，点击**角色**，然后点击此屏幕左上角的**创建角色**按钮。

3. 在**创建角色**屏幕上，选择 **AWS 服务**作为可信实体类型：

![](img/e4ee48104c730845e796d8c469dfeecc_400_0.png)

*图 14.6 – IAM 角色创建 – 选择可信实体类型*

4. 在下一部分（**选择用例**）中，找到 **Forecast** 并点击该服务名称。

5. 在最后一部分（**选择您的用例**）中，选择**允许 Amazon Forecast 访问 S3 中的数据**。

> **重要说明**
>
> 在撰写本书时，Amazon Lookout for Metrics 在 IAM 服务中还没有现成的用例。这就是为什么在前两个步骤中选择 **Forecast**。您可以稍后调整创建的角色，以匹配您要配置访问权限的服务。

6. 点击“下一步”按钮，直到到达最后一步（审查）。为您的角色命名并添加描述（我将其命名为 LookoutMetricsAccessRole）。

![](img/e4ee48104c730845e796d8c469dfeecc_401_0.png)

*图 14.7 – IAM 角色创建 – 审查步骤*

7. 点击“创建角色”。您的角色已创建，屏幕顶部会显示一条成功消息，您可以在其中点击角色的名称。然后您会看到角色的摘要。在此屏幕顶部，您将看到角色 ARN（Amazon 资源名称）字段。复制此 ARN 并粘贴到方便的地方。当我们把您的数据摄取到服务中时，将需要它。

8. 现在，您将调整此角色以专门限制对您的 S3 桶的访问。为此，请在**权限策略**部分找到 **AmazonS3FullAccess** 行，并通过点击右侧的 X 图标将其移除。确认框会要求您确认确实要从该角色分离此策略。点击**分离**按钮进行确认。

![](img/e4ee48104c730845e796d8c469dfeecc_402_0.png)

*图 14.8 – IAM 角色创建 – 分离完全访问策略*

9. 接下来，点击**添加内联策略**链接，然后点击 **JSON** 选项卡。在编辑器中复制并粘贴以下策略文档。别忘了调整 `<<YOUR-BUCKET>>` 字符串，使之与您之前创建的桶的名称匹配：

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetBucketAcl"
            ],
            "Resource": [
                "arn:aws:s3:::<<YOUR-BUCKET>>"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:GetBucketAcl"
            ],
            "Resource": [
                "arn:aws:s3:::<<YOUR-BUCKET>>/*"
            ]
        }
    ]
}
```

10. 向下滚动到策略定义屏幕底部，点击“审查策略”按钮。

11. 为策略命名（例如 LookoutMetricsDatasetAccess），然后点击“创建策略”。

![](img/e4ee48104c730845e796d8c469dfeecc_403_0.png)

*图 14.9 – IAM 角色创建 – 添加最低权限访问策略*

12. 现在让我们调整此角色以匹配正确的服务（Amazon Lookout for Metrics 而不是 Amazon Forecast）。为此，点击“信任关系”选项卡，然后点击“编辑信任关系”按钮。在“编辑信任关系策略文档”中，粘贴以下 JSON 文档，然后点击“更新信任策略”按钮：

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "",
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "sns.amazonaws.com",
                    "lookoutmetrics.amazonaws.com"
                ]
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

13. 您的信任关系现在应如下所示：

![](img/e4ee48104c730845e796d8c469dfeecc_404_0.png)

*图 14.10 – IAM 角色创建 – 更新信任关系*

当 Amazon Lookout for Metrics 尝试读取您刚刚上传到 S3 的数据集时，它将使用我们刚刚创建的角色向 IAM 请求权限。您定义的用于授予对 S3 桶访问权限的策略，就是 IAM 将用来授权 Amazon Lookout for Metrics 访问您的数据集的依据。另一方面，您定义的信任关系将允许其他服务（例如 Amazon SageMaker）为您运行 Amazon Lookout for Metrics API。您可能已经注意到，我们还在信任关系中添加了 SNS 服务。

在本章后面，您将使用 Amazon 简单通知服务（SNS）在检测到严重异常时创建通知。我们将利用此信任关系授予 Amazon Lookout for Metrics 调用 Amazon SNS API 的能力。

现在，您已准备好将数据集连接到 Amazon Lookout for Metrics。
