

## 11
数据湖安全
与监控

在前几章中，我们学习了数据整理流程，以及如何利用 AWS 生态系统中的不同服务进行数据整理活动。到目前为止，你应该已经掌握了使用 Glue DataBrew、SageMaker Data Wrangler 和 AWS SDK for pandas 创建数据管道的足够知识。我们还讨论了 AWS 服务（如 AWS Glue、Athena、Amazon S3 和 QuickSight）提供的功能，所有这些都更有效地支持数据整理管道中的特定活动。在本章中，我们将探讨如何为您的工作负载的数据整理管道设置安全和监控。

本章涵盖以下主题：

- 数据湖安全
- 监控与审计

## 数据湖安全

在本节中，我们将讨论增强数据湖安全性的各种选项和方法。我们将讨论跨越多个层级的安全性；然而，这应仅被视为一个起点。你需要进一步完善，并根据项目的需要添加额外的安全层级。

## 数据湖访问控制

为了保障数据湖的安全，我们可以结合使用基于用户的访问策略和基于资源的访问策略。用户策略附加到用户/角色，并控制用户/角色在账户内可执行的操作。除了基于用户的控制策略外，我们还可以通过将访问策略附加到 Amazon S3、Amazon SQS 等资源来控制对该特定资源的访问。这为保护 AWS 资源提供了另一层安全。作为一个通用最佳实践，访问策略应保持最小权限原则，仅允许在特定条件下执行所需的行动。

### 基于用户的访问策略

AWS 身份和访问管理（IAM）以集中式的方式帮助控制对 AWS 资源的访问。它同时为您的 AWS 账户提供身份验证和授权：

- 身份验证使用其凭据在访问 AWS 资源之前验证资源/人员的身份
- 授权验证已验证身份的用户/资源是否被允许执行特定操作

我们将在本节中更多地讨论 IAM 授权，以了解它如何保护数据湖的访问。

一些 IAM 策略的最佳实践在此处有详细说明。你还应遵循以下 AWS 文档中概述的最佳实践：[https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)。让我们来看一下：

- 你应该使用服务控制策略（SCPs）为整个账户或组织内的多个账户应用权限护栏。护栏策略是用户在各账户上的最大可用权限。组织可以使用这些策略来阻止某些操作，以确保严格的安全姿态。一个例子是添加一个 SCP 策略，阻止个人团队成员创建新的角色/用户。以下链接提供了你开始使用的一些示例 SCP 策略：[https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_examples_general.html#example-scp-restricts-iam-principals](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_examples_general.html#example-scp-restricts-iam-principals)。
- 你还可以为用户或角色提供 IAM 权限边界，以定义可以附加到该角色的权限最大边界。这将防止添加原本意图之外的额外权限。即使通过托管或内联策略为 IAM 用户/角色添加了更广泛的权限，权限边界也将作为预防性护栏，使该用户/角色只能拥有定义在权限边界内的访问权限。一个常见的用例是允许开发人员创建额外角色以执行日常任务，但同时也限制这些服务角色拥有更广泛的权限，以避免任何安全问题。请参阅以下博文，了解有关使用权限边界的更多详细信息：[https://aws.amazon.com/blogs/security/when-and-where-to-use-iam-permissions-boundaries/](https://aws.amazon.com/blogs/security/when-and-where-to-use-iam-permissions-boundaries/)。

你可以通过基础架构供应代码或控制台为特定用户将权限边界附加到个人用户角色：

```
1 - {
2       "Version": "2012-10-17",
3       "Statement": [
4           {
5               "Sid": "Statement1",
6               "Principal": "*",
7               "Effect": "Deny",
8               "Action": "*",
9               "Resource": ["arn:aws:s3:::wranglerbook", "arn:aws:s3:::wranglerbook/*"],
10              "Condition": {
11                  "NotIpAddress": {
12                      "aws:SourceIp": "172.0.0.0/24"
13                  }
14              }
15          }
16      ]
17  }
```

![](img/e28602b44ec813a47df57c96b204412c_390_0.png)

- 你还可以通过文件夹级别控制用户的访问。请注意，Amazon S3 是一个对象数据存储，具有扁平的文件存储层次结构；AWS 控制台将具有相同前缀的数据可视化为文件夹，以便于浏览。因此，在共享数据集的情况下，仅授予特定前缀的访问权限。例如，假设我们在不同的文件夹下存储了来自多个客户的数据。我们可以将特定用户角色的访问权限限制在特定的文件夹/前缀。请参阅以下文档路径，了解设置文件夹级别访问权限的存储桶策略示例：`https://docs.aws.amazon.com/AmazonS3/latest/userguide/example-bucket-policies.html#example-bucket-policies-folders`。

- 在需要为多组用户提供不同文件夹中对象访问权限的情况下，使用对象标签将访问权限限制为 S3 存储桶中的特定对象。例如，假设我们有三个不同的数据源（电子商务、代理、其他）将用户数据存入 S3 存储桶。每个源每天将上传两个文件，一个包含敏感数据（不应被用户访问），另一个不包含敏感数据（可被所有用户访问）。每个源将文件上传到以其名称命名的文件夹下。在这种情况下，我们可以使用对象标签来提供对不包含敏感数据的文件的访问权限，如下所示：

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Principal":{"AWS":[
        "arn:aws:iam::<<accountid>>/role/role_name"]},
      "Effect": "Allow",
      "Action": ["s3:GetObject"],
      "Resource": "arn:aws:s3::wranglerbook/*",
      "Condition": { "StringEquals": {"s3:ExistingObjectTag/dataclassification": "sensitive" } }
    }
  ]
}
```

图 11.3：基于附加到 S3 存储桶的标签限制访问的 S3 访问策略语句

你还可以在数据上传到 S3 存储桶时确保对象被正确标记，通过使标签成为 PutObject 操作的条件。请参阅以下 AWS 文档，了解基于对象的标签的更多详细信息：[https://docs.aws.amazon.com/AmazonS3/latest/userguide/example-bucket-policies.html#example-bucket-policies-object-tags](https://docs.aws.amazon.com/AmazonS3/latest/userguide/example-bucket-policies.html#example-bucket-policies-object-tags)。

### 控制数据湖访问的额外选项

你可以执行以下活动来保护数据湖中的数据：

- **S3 访问点**帮助简化被多个团队访问的共享存储桶的访问控制。它们是附加到 S3 存储桶的命名网络端点，具有不同的权限策略。S3 端点通过将每个用例的策略分离到独立的端点中，帮助管理具有复杂存储桶策略且难以维护的存储桶。一些常见用例包括托管大型共享数据集的存储桶，其中每个数据集具有不同的访问策略，以及在影响现有策略的情况下在 S3 存储桶上测试新的访问策略更改。

- **对象 Lambda 访问点**在你希望在访问点前使用 Lambda 函数时非常有用。它们有助于使用 Lambda 对象进行 PII 数据脱敏等情况。请参阅以下博文以了解如何实现此解决方案：[https://docs.aws.amazon.com/AmazonS3/latest/userguide/tutorial-s3-object-lambda-redact-pii.html](https://docs.aws.amazon.com/AmazonS3/latest/userguide/tutorial-s3-object-lambda-redact-pii.html)。

- **IAM Access Analyzer** 有助于验证策略的语法和最佳实践，识别与外部实体共享的资源，并基于 CloudTrail 日志生成策略。它帮助你应用遵循最小权限模型的政策，仅授予用户所需的最小权限。应定期验证，以确保根据最近的用户活动更新策略。

