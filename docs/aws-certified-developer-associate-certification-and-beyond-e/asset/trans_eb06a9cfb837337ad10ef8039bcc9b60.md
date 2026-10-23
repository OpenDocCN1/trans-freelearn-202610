
在 `samconfig.toml` 文件中，你会注意到第 11 行将 `confirm_changeset` 设置为 `true`。这意味着当你部署包时，CloudFormation 会高亮显示将对你的堆栈所做的一系列更改，并且你将有机会确认这些更改。接下来，使用 Windows 上的文件资源管理器或 Mac 上的访达，将桌面上的 `samconfig.toml` 文件复制并粘贴到 `todolistsam` 文件夹中。系统会提示你是否要替换现有文件，请直接替换。

至此，你已经完成了所有前置条件，并为部署配置好了 SAM。

## 项目任务 14.4：构建并部署你的应用程序

请确保你仍处于 Windows 的命令提示符或 Mac/Linux 的终端中，且仍位于 `todolistsam` 文件夹内。

- 1. 输入 `sam build` 命令，然后按回车键。你应看到 `Build Succeeded` 的提示。
- 2. 接下来，你需要打包应用程序。输入以下命令：
```
sam package --output-template-file packaged.yaml --s3-bucket [YOURBUCKETNAME.SAM]
```
将占位符替换为你在项目任务 14.3 的第 20 步中创建的存储桶名称。按回车键。你应收到确认信息，表明 SAM 已成功打包构件，并将输出模板写入名为 `packaged.yaml` 的文件，如下面截图所示。你会在此文件与其他 SAM 配置文件所在的同一文件夹中找到它。

```
iaasacademycreator@IaaSs-MacBook-Air sam-app % ls
README.md    events    samconfig.toml    tests
__init__.py  hello_world    template.yaml    website
iaasacademycreator@IaaSs-MacBook-Air sam-app % sam build

Build Succeeded

Built Artifacts  : .aws-sam/build
Built Template   : .aws-sam/build/template.yaml

Commands you can use next
==========================
[*] Validate SAM template: sam validate
[*] Invoke Function: sam local invoke
[*] Test Function in the Cloud: sam sync --stack-name {{stack-name}} --watch
[*] Deploy: sam deploy --guided
iaasacademycreator@IaaSs-MacBook-Air sam-app % sam package --output-template-file packaged.yaml --s3-bucket todolistapp.sam
Successfully packaged artifacts and wrote output template to file packaged.yaml.
Execute the following command to deploy the packaged template
sam deploy --template-file /Users/iaasacademycreator/Desktop/todoimagesam/sam-app/packaged.yaml --stack-name <YOUR STACK NAME>

iaasacademycreator@IaaSs-MacBook-Air sam-app %
```

图 14.13 – SAM 打包命令输出

3. 接下来，你需要部署应用程序。输入以下命令，确保将 `[YOURSTACKNAME]` 占位符替换为你在之前的 `samconfig.toml` 文件中定义的名称，然后按回车键：

```
sam deploy --template-file packaged.yaml --stack-name [YOURSTACKNAME] --capabilities CAPABILITY_IAM
```

请注意，`CAPABILITY_IAM` 必须使用大写字母。

4. CloudFormation 将提供一份它将执行的更改列表，并询问你是否确认部署此更改集，如下面截图所示：

![](img/34ff9b5b5a734dae3450ab701e223c73_614_0.png)

图 14.14 – 更改集确认请求

5. 输入 `y` 并按回车键。CloudFormation 现在将部署你的堆栈；这可能需要几分钟时间。完成后，你应该会看到类似下面截图的输出界面：

![](img/34ff9b5b5a734dae3450ab701e223c73_615_0.png)

*图 14.15 – CloudFormation 部署操作*

- 6. 现在 CloudFormation 已配置好你的堆栈，你可以通过导航到 AWS 管理控制台中的各个资源来交叉验证堆栈组件：

  I. 在 AWS 管理控制台中，导航到 Amazon S3 管理控制台。你应该会找到用于托管前端网站的存储桶，如下面截图所示：

|  | 存储桶 | 区域 |
|---|---|---|
| ○ | [todolistapp.com](#) | 美国东部（弗吉尼亚北部）us-east-1 |
| ○ | [todolistapp.sam](#) | 美国东部（弗吉尼亚北部）us-east-1 |

*图 14.16 – Amazon S3 todolistapp.com 存储桶*

II. 接下来，导航到 CloudFront 管理控制台，你应该会找到已创建的分配：

![](img/34ff9b5b5a734dae3450ab701e223c73_616_0.png)

图 14.17 – CloudFront 分配

7. 现在你已经确认堆栈已正确部署，需要将网站文件复制到 CloudFormation 创建的 S3 存储桶中。

8. 回到 Windows 的命令提示符或 Mac/Linux 的终端，在 `todolistsam` 文件夹内运行以下命令：

```
aws s3 sync website/ s3://<YOURWEBSITEBUCKETNAME.COM>.
```

将占位符值替换为你使用 SAM 和 CloudFormation 创建的存储桶名称。在本操作指南中，存储桶名称为 `todolistapp.com`。此命令会将网站文件从本地计算机上传到 Amazon S3 存储桶，你将看到命令提示符/终端窗口中显示的相应输出，如下面截图所示：

```
iaasacademycreator@IaasS-MacBook-Air sam-app % aws s3 sync website/ s3://todolistapp.com
upload: website/.DS_Store to s3://todolistapp.com/.DS_Store
upload: website/assets/img/logo.png to s3://todolistapp.com/assets/img/logo.png
upload: website/assets/.DS_Store to s3://todolistapp.com/assets/.DS_Store
upload: website/assets/img/stay2.jpg to s3://todolistapp.com/assets/img/stay2.jpg
upload: website/index.html to s3://todolistapp.com/index.html
upload: website/assets/img/fav.png to s3://todolistapp.com/assets/img/fav.png
upload: website/assets/js/main.js to s3://todolistapp.com/assets/js/main.js
upload: website/assets/css/style.css to s3://todolistapp.com/assets/css/style.css
upload: website/assets/img/steve-jobs.png to s3://todolistapp.com/assets/img/steve-jobs.png
upload: website/assets/img/bg.jpg to s3://todolistapp.com/assets/img/bg.jpg
upload: website/assets/img/head-1.png to s3://todolistapp.com/assets/img/head-1.png
upload: website/assets/img/banner-bg.png to s3://todolistapp.com/assets/img/banner-bg.png
iaasacademycreator@IaasS-MacBook-Air sam-app %
```

图 14.18 – 使用 `aws s3 sync` 命令将网站内容上传到 S3 存储桶

- 9. 网站文件上传完成后，在 AWS 账户中导航到 CloudFront 管理控制台。单击 CloudFormation 创建的分配 ID。
- 10. 记下分配域名，并将该 URL 粘贴到另一个浏览器标签页中。
- 11. 你应该会看到新的待办事项应用“即将上线”网站正在运行，如下面截图所示：

![](img/34ff9b5b5a734dae3450ab701e223c73_617_0.png)

图 14.19 – 待办事项前端静态网站——即将上线页面

AWS CloudFormation 是一个通用的**基础设施即代码（IaC）**工具，可让你以声明方式定义并配置广泛的 AWS 基础设施资源。AWS SAM 是一个专门用于构建无服务器应用程序的 IaC 工具。虽然 CloudFormation 支持广泛的 AWS 服务，但 SAM 通过提供更高级别的抽象和 SAM 专用语法，简化了 Lambda 函数、API Gateway 和 DynamoDB 等无服务器资源的定义、部署和管理。SAM 模板会扩展为完整的 CloudFormation 模板，但使用 SAM 编写的代码更少。

在下一个项目任务中，你将删除由 SAM 创建的 CloudFormation 堆栈，以避免不必要的费用。

## 项目任务 14.5：删除 CloudFormation 堆栈

你可以使用 `sam delete` 命令删除 CloudFormation 堆栈。但这里有一点需要注意：由于你的网站存储桶中包含对象，堆栈删除过程将会失败。这是因为 CloudFormation 无法删除包含对象的 S3 存储桶。要快速解决此问题，你必须先删除存储桶中的内容，然后才能执行 `sam delete` 命令来删除堆栈：

- 1. 导航到 Amazon S3 管理控制台，单击由 SAM 创建的网站存储桶——即你在*项目任务 14.3* 的*第 3 步*中于 `template.yaml` 文件第 46 行定义的存储桶。
- 2. 选中存储桶中的全部内容，然后点击**删除**按钮，如*图 14.20* 所示。

![](img/34ff9b5b5a734dae3450ab701e223c73_618_0.png)

