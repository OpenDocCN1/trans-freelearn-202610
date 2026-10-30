

10. 向下滚动并填写突出显示的字段，如图 6.2 所示：

## 数据库配置

数据库名称（可选）
指定要创建的附加数据库的名称。
glue-dev
名称必须为 1-64 个字母数字字符（仅小写），且不能是保留字。

数据库端口（可选）
数据库接受入站连接的端口号。集群创建后无法更改端口。
5439
端口必须为数字（1150-65535）。

主用户名
为您的 DB 实例的主用户输入登录 ID。
awsuser
名称必须为 1-128 个字母数字字符，且不能是保留字。

主用户密码
******
显示密码
主密码必须为 8 - 64 个字符。
该值必须包含至少一个大写字母。
该值必须包含至少一个小写字母。
该值必须包含至少一个数字。
主密码只能包含 ASCII 字符（ASCII 代码 33-126），且不能包含 '（单引号）、"（双引号）、/、\ 或 @。

图 6.2 – Amazon Redshift 集群数据库配置部分的屏幕截图

11. 向下滚动并更改“其他配置”字段，如图 6.3 所示：

## 其他配置

这些配置是可选的，已定义默认设置以帮助您开始使用集群。关闭“使用默认值”以立即修改这些设置。

## 网络和安全

虚拟私有云（VPC）

此 VPC 定义此集群的虚拟网络环境。选择具有子网组的 VPC。列表中仅启用有效 VPC。

默认 VPC
vpc-140d146e

集群创建后无法更改与此集群关联的 VPC。了解更多

VPC 安全组

此 VPC 安全组定义集群在 VPC 中可以使用哪些子网和 IP 范围。

选择一个或多个安全组

redshift-self
sg-059c2134eb1a29141

图 6.3 – Amazon Redshift 集群其他配置部分的屏幕截图

12. 同样更改 IAM 权限，如图 6.4 所示：

## 集群权限（可选）

您的集群需要权限代表您访问其他 AWS 服务。为所需的权限添加“redshift.amazonaws.com”。您可以将多达 10 个 IAM 角色与此集群关联。了解更多

可用 IAM 角色

AWSServiceRoleForRedshift

已附加的 IAM 角色
状态

此资源未关联任何 IAM 角色

图 6.4 – Amazon Redshift 集群集群权限部分的屏幕截图

13. 向下滚动并单击 **Create Cluster**。需要一两分钟才能使集群处于可用状态。

接下来，我们将创建一个 IAM 角色。
14. 导航到 AWS IAM 控制台并在屏幕的 **Access Management** 部分选择 **Roles**。
15. 单击 **Create role** 按钮并从服务中选择 **Glue**。单击 **Next: permissions** 按钮导航到下一页。
16. 搜索 **AmazonS3FullAccess** 并选择。然后搜索 **AWSGlueServiceRole** 并选择。由于我们在此示例中将数据写入 Redshift，请选择 **AmazonRedshiftFullAccess**。单击 **Next: Tags**，然后单击 **Next: Review** 按钮。
17. 提供名称，`Glue-IAM-Role`，然后单击 **Create role** 按钮。角色显示如下图 6.5 所示：

![](img/209e007237d34506e7ba16180f2e905c_200_0.png)

图 6.5 – IAM 角色的屏幕截图

现在，我们有了输入数据源和输出数据存储。下一步是从 AWS Glue 控制台创建 Glue 爬虫。

18. 在 **Databases** 下选择 **Connections**。单击 **Add connection** 按钮并按如下所示完成字段：

a) **Connection name:** `glue-redshift-connection`

b) **Connection type:** `Amazon Redshift`

19. 单击 **Next**，然后按如下所示填写字段：

a) **Cluster:** `redshift-glue-example`

b) **Database name:** `glue-dev`

c) **Username:** `awsuser`

d) **Password:** `********`（输入 *步骤 10* 中创建的值）

20. 单击 **Next** 然后 **Finish**。要验证工作是否正常，单击 **Test Connection**，为 IAM 角色部分选择 `Glue-IAM-Role`，然后单击 **Test Connection.**

21. 转到 **Crawler** 并选择 **Add Crawler**。为爬虫提供名称，`s3-glue-crawler`，然后单击 **Next**。在 **Specify crawler source type** 页面，保留所有默认设置，然后单击 **Next。**

22. 在 **Add a data store** 页面，选择 **Include path:** `s3://aws-glue-example-01/input-data/sales-records.csv.`

23. 单击 **Next.**

24. **Add another datastore:** No。单击 **Next.**

25. **Choose an existing IAM Role:** `Glue-IAM-Role`。然后单击 **Next.**

26. **Frequency:** `Run on demand`。单击 **Next.**

27. 没有创建数据库，因此单击 **Add database**，提供名称，`s3-data`，单击 **Next**，然后单击 **Finish。**

28. 选择爬虫，`s3-glue-crawler`，然后单击 **Run Crawler**。运行完成后，您可以看到 **Tables Added** 列中有 1。这意味着已创建数据库，`s3-data`，如上一步所述，并添加了表。单击 **Tables** 并选择新创建的表，`sales_records_csv`。您现在可以看到架构已被发现。您可以根据需要更改数据类型，如果推断的数据类型不符合要求。

在本动手部分，我们学习了数据库表、数据库连接、S3 上的爬虫以及 Redshift 集群的创建。在下一个动手部分，我们将学习使用 Glue 创建 ETL 作业。

## 使用 AWS Glue ETL 组件动手操作

在本节中，我们将使用之前创建的数据目录组件来构建作业。让我们开始创建作业：

- 1. 导航到 AWS Glue 控制台并在 **ETL** 部分下单击 **Jobs**。
- 2. 单击 **Add Job** 按钮并按如下所示完成字段：
  - **Name:** `s3-glue-redshift`
  - **IAM role:** `Glue-IAM-Role`（这是我们在上一节中创建的同一角色）
  - **Type:** `Spark`
  - **Glue version:** `Spark 2.4, Python 3 with improved job start up times (Glue version 2.0)`
- 3. 将其他字段保持原样，然后单击 **Next**。
- 4. 选择 `sales_records_csv` 并单击 **Next**。
- 5. 默认选择 **Change Schema**，然后单击 **Next**（在撰写本书时，机器学习转换不适用于 Glue 2.0）。
- 6. 选择 **Create tables in your data target**。选择 JDBC 作为数据存储，并将 `glue-redshift-connection` 作为连接。提供 `glue-dev` 作为数据库名称（如上一节中创建的），然后单击 **Next**。
- 7. 接下来是 **Output Schema Definition** 页面，您可以在其中选择要从目标架构中删除的所需列。向下滚动并单击 **Save job and edit script**。
- 8. 您现在可以看到屏幕左侧创建的管道和右侧建议的代码，如图 6.6 所示。您可以根据需要修改代码。单击 **Run job** 按钮。会出现一个弹出窗口，询问您是否要编辑任何详细信息。这是可选的。然后，单击 **Run job** 按钮：

![](img/209e007237d34506e7ba16180f2e905c_203_0.png)

图 6.6 – AWS Glue ETL 作业的屏幕截图

9. 作业成功后，导航到 Amazon Redshift 并单击 Query editor。

10. 将数据库名称设置为 glue-dev，然后提供用户名和密码以创建连接。

11. 选择 public schema，现在您可以查询表以查看记录，如图 6.7 所示：

![](img/209e007237d34506e7ba16180f2e905c_204_0.png)

图 6.7 – Amazon Redshift Query editor 的屏幕截图

我们现在了解了如何使用 AWS Glue 创建 ETL 作业，将数据从 S3 存储桶复制到 Amazon Redshift。我们还使用 UI 控制台中的查询编辑器在 Amazon Redshift 中查询了数据。如果您已成功完成步骤，建议删除 Redshift 集群和 AWS Glue 作业。AWS 在您的账户中创建两个存储桶来存储 AWS Glue 脚本和 AWS Glue 临时结果。因此，请一并删除以节省成本。我们将在下一节中使用在 S3 数据上创建的数据目录。

在下一节中，我们将学习如何使用 Athena 查询 S3 数据。

## 使用 Athena 查询 S3 数据

Athena 是一种无服务器服务，专为查询存储在 S3 中的数据而设计。它是无服务器的，因为客户端不需要管理用于计算的服务器：

- Athena 使用架构来根据存储在 S3 中的数据呈现查询结果。您定义数据的显示方式的架构，Athena 从 S3 读取原始数据并根据定义的架构显示结果。

