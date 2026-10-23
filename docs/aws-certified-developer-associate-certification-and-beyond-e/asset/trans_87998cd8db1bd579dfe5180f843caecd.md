
更新后的应用程序代码位于我们的GitHub仓库中：

```
https://packt.link/OxeLU
```

请确保下载此仓库中的.zip文件，并将其存储在本地计算机上。你将在下一节详细介绍的项目任务中使用这些文件。

## 项目任务9.1 – 更新应用程序后端代码

你需要更新应用程序，以确保它能与Cognito中经过身份验证的用户进行连接。回想一下，托管应用程序API的EC2实例当前在自动扩缩组中运行，你希望确保任何更新都自动复制到EC2实例。由于应用程序代码存放在Amazon S3存储库中（如前图所示，即Amazon S3 Todo List代码存储库存储桶），你必须使用新的应用程序代码更新该存储桶。然后，你可以终止现有EC2实例，强制Auto Scaling服务部署新实例，并将应用程序复制到新创建的实例。在下一章中，你将探索旨在自动化部署新版本更新的DevOps策略：

1. 以IAM用户“Alice”身份重新登录你的AWS账户，并确保你在北弗吉尼亚区域操作。
2. 导航至Amazon S3控制台。
3. 选择`todoapprepo`存储桶，如图9.12所示：

![](img/34ff9b5b5a734dae3450ab701e223c73_372_0.png)

图9.12：Todo应用存储库存储桶

> **注意：** 你会注意到现有的后端应用程序采用.zip格式。该文件被下载到EC2实例，EC2实例使用脚本（回想一下用户数据脚本，它是EC2实例Auto Scaling启动配置的一部分）来配置实例的应用程序代码文件。

4. 单击Upload按钮，上传你之前从GitHub仓库下载的`todo-plus-nodejs.zip`文件，如前文“技术要求”部分所述。用此新版本替换存储桶中的现有文件，如图9.13所示：

![](img/34ff9b5b5a734dae3450ab701e223c73_373_0.png)

图9.13 – 将更新后的Node.js应用程序代码复制到S3存储桶

5. 接下来，通过在屏幕顶部的搜索栏中搜索，导航到EC2管理控制台。
6. 从左侧菜单中选择Instances。
7. 在右侧窗格中，你应该发现有两个正在运行并托管后端应用程序的实例。回想一下，你在第7章“构建具有高可用性、弹性和数据安全性的应用程序解决方案”中配置了一个Auto Scaling组来部署这些后端实例。

你必须通过选择这些实例，然后从Instance state下拉列表中单击Terminate instance来终止它们，如以下截图所示：

![](img/34ff9b5b5a734dae3450ab701e223c73_374_0.png)

图9.14：终止现有实例以强制Auto Scaling服务部署新实例

8. 系统会提示你确认终止这些实例。在出现的对话框中，继续单击Terminate。

9. 一旦这些实例被终止，Auto Scaling服务将使用来自你的Amazon S3存储桶（作为存储库）的更新应用程序代码启动新实例。这通常需要几分钟。在Auto Scaling部分，从左侧菜单中单击Auto Scaling Groups。从右侧窗格中，选择你在第7章“构建具有高可用性、弹性和数据安全性的应用程序解决方案”中创建的`todo-list-auto-scaling-group`。在右侧窗格中，单击Activity。你将发现之前的两个实例已被删除，并替换为两个新实例，如以下截图所示：

![](img/34ff9b5b5a734dae3450ab701e223c73_374_1.png)

图9.15：Auto Scaling组响应上一步对现有实例的终止操作，启动两个新实例

在下一个任务中，你将更新前端网站代码。这个新版本包含与Amazon Cognito的必要集成，使用户能够注册和登录应用程序。

## 项目任务9.2 – 更新前端代码

在此项目任务中，你将更新配置为托管在Amazon S3存储桶上的静态网站的前端应用程序代码。在第3章“了解使用Amazon S3、混合存储和静态网站托管的对象存储”中，你配置了一个启用静态网站托管的Amazon S3存储桶。然后，你将前端代码部署到该存储桶。原始版本的注册和登录表单无法使用，因为当时还没有与Amazon Cognito集成。

此更新后的代码版本包含对Amazon Cognito的调用，以使用其托管UI作为注册和登录表单，该UI连接到你第2章“使用AWS身份和访问管理保护访问权限”中创建的Cognito用户池：

1. 确保你已登录到你的AWS开发账户。
2. 通过在网页顶部的搜索栏中搜索该服务，导航到Cognito控制台。确保你在北弗吉尼亚区域。
3. 从左侧菜单中选择User pools。
4. 在右侧窗格中，单击你在第2章“使用AWS身份和访问管理保护访问权限”中创建的用户池，它应该名为`todolist-userpool`。
5. 选择App integration选项卡，如以下截图所示：

![](img/34ff9b5b5a734dae3450ab701e223c73_375_0.png)

图9.16：Amazon Cognito App Integration选项卡，用于识别你的Cognito客户端ID

6. 向下滚动页面底部，直到到达App client list部分。
7. 记下Client ID。

8. 接下来，你需要修改新更新的前端网站的`index.html`文件：

   I. 解压你从我们的GitHub仓库下载的`frontend.zip`文件的内容。

   II. 在你喜欢的编辑器中打开`index.html`文件，例如Visual Studio Code。然后，滚动到文件末尾，直到到达代码中`<!-- Template Main JS File -->`部分的`window.cognitoClientId`，如图9.17所示。

   III. 使用你的Cognito客户端ID更新Client ID，替换引号中提供的占位符。图9.17展示了一个用于进行更改的占位符。请注意，如果你按照第8章“使用AWS Lambda进行事件驱动计算并通过Amazon API Gateway保护后端API访问”中的项目任务8.5（与API Gateway集成）为API Gateway创建了不同于“development”的阶段名称，则必须确保在此处更新它，如以下截图中的第二个占位符所示：

```
<!-- Vendor JS Files -->

<!-- Template Main JS File -->
<script src="main.js"></script>

<script>
  window.cognitoClientId = "Enter Your Cognito Client ID Here" // Put your client id here
  window.BASE_URL = "/development"; // Put your base path here (if different)
</script>

</body>
</html>
```

图9.17：使用你的Cognito客户端ID修改index.html页面的新版本

   IV. 保存`index.html`文件。

9. 接下来，导航到Amazon S3控制台。
10. 单击你配置用于托管前端网站的存储桶。
11. 接下来，将解压后的`frontend.zip`文件按照相同的文件夹层级结构上传到存储桶的根目录。请记住，你之前刚刚更新了`index.html`文件。你还会注意到，需要额外上传一个`main.js`文件。上传所有文件，如图9.18所示：

![](img/34ff9b5b5a734dae3450ab701e223c73_377_0.png)

图9.18：使用更新的HTML文件和文件夹更新前端静态网站存储桶

12. 你应该会收到一条确认信息，表明你的文件已成功上传。

在下一个任务中，你将创建一个REST API授权器，实现Cognito与API Gateway之间的集成，确保授权用户可以调用API。

## 项目任务9.3 – REST API授权器

1. 确保你仍登录到AWS开发账户，并在北弗吉尼亚区域。
2. 接下来，搜索并导航到API Gateway Web控制台。
3. 从左侧菜单中，单击**APIs**。
