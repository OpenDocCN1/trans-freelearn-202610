

你可以看到本世纪记录最高气温的三个地点——梅里登（Meriden）、丹伯里（Danbury）和新 Haven（New Haven）。此外，正如你从分析中可以看到的那样，自 2000 年以来，气温从未超过 50°C：

![](img/e28602b44ec813a47df57c96b204412c_238_0.png)

前面的练习展示了使用 Amazon Athena 进行此类分析的威力，而它依赖于数据的质量和准确性以获取正确结果。你需要依靠数据清洗和数据质量规则来创建更干净的数据集，以提供准确的分析结果。

Amazon Athena 支持所有类型的连接，如 INNER JOIN、LEFT JOIN、RIGHT JOIN、OUTER JOIN 和 CROSS JOIN，具体取决于你的使用场景需求。此外，Athena SQL 中还有一组丰富的功能，可用于对数据进行高度复杂的分析和转换。请参阅文档，了解 Athena SQL 功能的完整列表，见 `https://docs.aws.amazon.com/athena/latest/ug/ddl-sql-reference.html`。

> **挑战练习**
> 你可以连接我们在*第 10 章，使用 SageMaker Data Wrangler 进行机器学习的数据处理*中使用的体育博彩使用案例的数据集。你可以使用 Athena 查询重新创建我们在该章的数据富集部分创建的体育队伍赛程。

## 为源数据库设置数据联合

我们现在将更深入地探索从不同数据源连接数据集的方法。由于 NOAA 天气数据集仅存储在 Amazon S3 桶中，我们将向**关系型数据库服务 (RDS)** MySQL 数据库添加一些数据集，并展示如何在不将数据移动到中心位置的情况下查询数据。这有助于业务团队使用 SQL 运行快速分析，而无需依赖数据工程团队将数据从一个数据源移动到另一个位置。这对业务团队来说非常有用，因为他们可以在不依赖其他团队的情况下运行快速分析，但仅应用于即席分析场景，即你需要在短时间内获取结果的情况；而对于重复的数据分析以获取更好的性能，则应设置数据管道。

Athena 查询联合通过创建带有必要代码的 Lambda 函数来连接源数据库，从而起作用。支持的源的源代码可以在此处找到：`https://github.com/awslabs/aws-athena-query-federation`。由于这是开源代码，你可以进一步开发并根据需求进行自定义，按照仓库中提供的说明创建你自己数据库的代码。一旦你在 AWS 账户中部署了无服务器应用程序，它就会创建一个 Lambda 函数，由 Athena 在后端有效执行以连接你的数据库。Athena 使用有效的机制通过多次 Lambda 调用连接源数据库，并且 Amazon Athena 拥有一种机制来绕过 Lambda 的 15 分钟超时限制，执行运行时间超过 15 分钟的查询。

## 前提条件 – 在 RDS MySQL 中创建源表

让我们假设一种场景，我们想要查询南极洲的天气数据，并以类似之前了解本世纪最低温度是如何做到的方式去理解。我们在 `noaa_data_parkue` 表中拥有国家代码值，但我们可能不知道南极洲的正确国家代码。我们将模拟一种场景，即国家查找表数据（包含国家代码和国家名称）来自 MySQL 数据库。为了创建该场景，我们将手动将数据加载到 MySQL 表中。你可以按照此处提供的信息创建 RDS MySQL 实例：`https://aws.amazon.com/getting-started/hands-on/create-mysql-db/`。

你可以使用 MySQL Workbench 连接到公共实例，如果你更喜欢在私有子网中启动 RDS 实例，则我们将使用**弹性计算云 (EC2)** 实例连接到 RDS 数据库。我们还将创建与 RDS MySQL 位于同一 VPC 中的 EC2 实例，并使用此机器连接到 MySQL 数据库。你可以按照此处描述的步骤设置 EC2 跳板主机：`https://aws.amazon.com/premiumsupport/knowledge-center/rds-connect-ec2-bastion-host/`。

在本节中，我们将登录到 EC2 实例并使用 `scp` 命令上传国家文件。在上传文件之前，你可以使用任何你选择的编辑器，将第一次出现的空格替换为逗号，如下所示，然后依次替换逗号并用双引号括起来。这将把数据转换为具有两个字段（国家代码和国家名称）的 CSV 文件：

```
1 AC,Antigua and Barbuda
2 AE,United Arab Emirates
3 AF,Afghanistan
4 AG,Algeria
5 AJ,Azerbaijan
6 AL,Albania
```

图 7.25：浏览国家查找文件中的数据

在进行上述更改后，将文件保存为 CSV 格式，并使用 `scp` 命令将文件上传到 EC2 实例的主目录，如下所示：

```
scp -i "yourpemfile.pem" <path/to/ghcnd-countries.csv> ec2-user@ec2-XXX-XXX-XXX-XXX.region.compute.amazonaws.com:~/
```

我们将使用 `ssh` 命令（从 EC2 控制台的连接屏幕）登录到 EC2 实例，然后使用以下命令连接到 `mysql` 实例：

```
mysql -h <<database endpoint string>> -u <<db username>> -P 3306 -p
```

我们将创建一个新的数据库，如下面的截图所示：

![](img/e28602b44ec813a47df57c96b204412c_240_0.png)

图 7.26：在 RDS MySQL 中创建数据库以加载国家查找文件

我们将创建一个表并将数据加载到该表中，如下所示。此时，我们已完成在 MySQL 中创建源表的前提条件：

![](img/e28602b44ec813a47df57c96b204412c_240_1.png)

图 7.27：将国家查找文件加载到 RDS MySQL

现在，我们将为此 MySQL 数据库在 Amazon Athena 中设置查询联合。导航到 Amazon Athena 控制台并创建一个数据源。或者，你也可以转到 AWS 无服务器应用程序仓库，创建一个应用程序以部署 Lambda 函数和支持查询联合所需的资源，如下所示：

![](img/e28602b44ec813a47df57c96b204412c_241_0.png)

图 7.28：搜索应用程序

在我们的使用案例中，我们将通过 Athena 控制台创建数据源，如下文所述。

## 在你的 AWS 账户中设置数据联合

从 Amazon Athena 创建一个新的数据源，如下所示：

![](img/e28602b44ec813a47df57c96b204412c_241_1.png)

图 7.29：Amazon Athena 数据源屏幕

在这里，我们将选择 mysql 作为数据源并点击 Next。你可以根据不同的使用案例选择相应的源数据库引擎：

![](img/e28602b44ec813a47df57c96b204412c_241_2.png)

图 7.30：Amazon Athena 针对 MySQL 数据源的数据联合

为应用程序输入一个名称——在我们的使用案例中是 wranglerathena——并点击**创建 Lambda 函数**，这将带你到无服务器应用程序部署屏幕。为了更多地了解连接器，你可以阅读许可证、README 文件和模板部分，以了解将要部署的资源。在**应用程序设置**下，你可以为当前的使用案例输入以下值：

- **应用程序名称：** `AthenaMySQLConnector`（保留默认值）。
- **SecretNamePrefix：** `SecretsManager` 密钥的名称，其中包含源数据库的用户名和密码。对于我们的测试，我们将直接在连接字符串中提供它们。但对于生产部署，强烈建议使用带有密码轮换功能的 secrets manager 以实现高效的安全。因此，在此字段中输入 `dummysecret` 值。
- **Spillbucket：** 提供 Lambda 函数可以存储超出其内存限制的数据的 S3 桶的名称。此功能有效地防止了较大查询结果的 Lambda 内存错误。
- **DefaultConnectionString：** 以以下格式提供连接字符串：
- **DisableSpillEncryption：** `False`（除非你不想加密 S3 中的 spill 数据）。
- **LambdaFunctionName：** 提供与 Athena 数据源连接器相同的名称以便于识别。在我们的情况下，这是 wranglerathena。
- **LambdaMemory** 和 **LambdaTimeout：** 除非你想更改它们，否则保留默认的最大值。
- **PermissionsBoundaryARN：** 在我们的使用案例中留空。
- **SecurityGroupIds：** 提供你希望与 Lambda 函数关联的安全组 ID。安全组需要有访问 MySQL 数据库的规则。
- **SpillPrefix：** 将此保留为默认值 `athena-spill`。
- **SubnetIds：** 提供 Lambda 函数将启动并连接到 RDS MySQL 的子网 ID。提供 RDS MySQL 实例所在的子网 ID，或者一个具有访问数据库权限的子网列表。

```
mysql://jdbc:mysql://<<database endpoint>>:3306/noaa?user=<<username>>&password=<<password>>
##Replace << >> values in the above string with correct values for your use case
```

应用程序创建完成后，选择在上一步中创建的 Lambda 函数，并在 Amazon Athena 中完成数据源的创建。

