

## CloudFormation 堆栈角色

在部署 CloudFormation 堆栈时，该服务使用登录到 AWS 账户的 IAM 主体的身份来执行必要的堆栈操作，例如创建、更新或删除。因此，IAM 主体必须通过附加到其上的策略（或用户所属的组）拥有所需的权限，或通过承担角色获得。

通常，在大型组织中，您可能需要将创建和更新堆栈的能力分配给团队成员，而这些成员不应拥有创建和管理构成堆栈的各个资源的完全访问权限。例如，您可能有一个帮助台团队，可以授予其部署 CloudFormation 堆栈的能力，但他们不得在 CloudFormation 之外单独部署堆栈的资源。在此场景中，您可以为 CloudFormation 配置服务角色。使用服务角色，您可以确保遵循最小权限原则。流程如下：

- 管理员创建一个 IAM 角色，该角色授予创建 CloudFormation 堆栈所需的资源所需的权限。管理员必须创建信任策略，其中受信任实体为 CloudFormation 服务，如以下策略代码所示：

  ```
  {
    "Version": "2012-10-17",
    "Statement": {
      "Effect": "Allow",
      "Principal": {"Service": "cloudformation.amazonaws.com"},
      "Action": "sts:AssumeRole"
    }
  }
  ```

- 然后必须授予帮助台员工使用 CloudFormation 创建、更新或删除堆栈的权限，并确保 CloudFormation 拥有配置资源的权限。帮助台团队应被分配使用 `iam:PassRole` 操作将 IAM 角色传递给 CloudFormation 的权限，如以下代码片段所示：

  ```
  {
    "Sid": "AllowPassingAppRoles",
    "Effect": "Allow",
    "Action": "iam:PassRole",
    "Resource": "arn:aws:iam::<account ID>:role/cfnroles/*"
  }
  ```

使用 CloudFormation 堆栈角色，您可以增加一层安全性，以确保只有授权人员可以直接操作资源。其他可能需要部署基础设施组件的用户只能通过 AWS CloudFormation 进行部署。

到目前为止，我们已详细介绍了 AWS CloudFormation，探讨了在构建和设计云基础设施时如何采用强大的 IaC 解决方案，使用可重复和可再现的方法来提高效率、安全、速度和敏捷性。

在下一节中，我们将转向 AWS CDK。该服务允许您使用高级库在流行的编程语言中开发云应用程序及其相关基础设施。

## AWS CDK 概述

AWS Cloud Development Kit（CDK）是一个软件开发框架，用于以代码形式定义云基础设施并通过 AWS CloudFormation 进行配置。借助 CDK，开发人员可以使用熟悉的编程语言，包括 TypeScript、Python、Java 和 C#，来定义其基础设施。CDK 使用熟悉的编程结构，例如类和对象，来定义云资源，使开发人员更容易使用和理解。

CDK 为 AWS 服务提供了一个高级、面向对象的接口，使开发人员能够以比传统 CloudFormation 模板更抽象、更简洁的方式定义其基础设施。例如，而不是在 JSON 或 YAML 文件中指定 EC2 实例的属性，开发人员可以使用其首选编程语言中的类和对象来定义实例。

CDK 还提供了一组构造，它们是用于定义基础设施的预写、可重用的构建块。构造包括从 EC2 实例和 RDS 数据库等基本组件到 Auto Scaling 组和负载均衡器等更复杂组件的一切。使用构造可以更轻松、更快地定义基础设施，并减少需要编写的代码量。

CDK 的另一个好处是与 AWS CLI 和其他 AWS 工具的集成，包括 AWS CloudFormation、AWS CloudWatch 和 AWS CodeDeploy。这种集成允许开发人员使用其首选工具和工作流程来管理其基础设施，并有助于确保其开发和生产环境之间的一致性和稳定性。

AWS CDK 的主要组件如下：

- **构造**：构造是应用程序的组件，表示由 CloudFormation 创建的一个或多个资源。例如，构造可以表示您的 EC2 实例或 RDS 数据库实例。您将构造定义为导入到 CDK 应用程序中的类。您可以从 AWS Constructs 库访问预定义的构造，创建自己的构造或使用第三方构建的构造。
- **堆栈**：堆栈是 CDK 中作为单个单元创建和管理的 AWS 资源集合。它们可以包含一个或多个构造，并用于定义和管理云中特定的一组资源。

![](img/34ff9b5b5a734dae3450ab701e223c73_581_0.png)

图 13.10 – 生产 VPC 的架构参考

您的 CloudFormation 堆栈的关键组件如下：

- VPC 使用 10.10.0.0/16 的 CIDR 范围进行配置。请记住，您在开发账户中为 VPC 使用的 CIDR 范围是 10.0.0.0/16。建议您的 VPC 不要使用重叠的 IP 地址，以确保在需要时可以建立 VPC 对等连接。
- CloudFormation 模板定义了一个互联网网关，然后将其附加到 VPC。
- 配置包括六个子网——跨两个可用区的两个公有子网和四个私有子网（两个用于计算资源，如 EC2、ECS 等，两个用于数据库资源）。
- 模板中声明了一个 NAT 网关，以实现私有子网中计算资源对互联网的安全访问。请注意，在本实验项目中，您只会部署单个 NAT 网关以尽量减少成本，因为这是收费服务。AWS 建议您每个 AZ 部署一个 NAT 网关。
- 路由表用于确保流量在 VPC 中路由到正确的端点，并与其相关子网关联。虽然不是绝对必要，但模板为每个子网定义了一个路由表。
- 然后在模板中声明安全组，以确保对内部资源的流量按端口/协议和源进行限制。

此处的意图是展示您如何使用 CloudFormation 模板快速部署基础设施，并让您了解如何快速准确地跨多个账户为不同环境部署解决方案。

## 技术要求

请记住，您在创建 Todo Plus 生产账户时并未使用 AWS Organizations 服务登录该账户，因此您需要导航到 AWS 管理控制台的主登录页面，并为生产账户的根用户请求密码重置，如图 13.11 所示。具体来说，您必须提供与 AWS 账户根用户关联的电子邮件地址。

另外，请确保从以下链接的 GitHub 仓库下载 CloudFormation 模板并将其保存在桌面本地：https://packt.link/xM0tt

![](img/34ff9b5b5a734dae3450ab701e223c73_582_0.png)

图 13.11 – 请求重置您的生产账户密码

重置根用户密码后，您可以登录并为生产账户创建一个具有完全管理权限的新 IAM 用户。建议您首先创建一个名为 **ProdAdmins** 的 IAM 组，将 **AdministratorAccess** IAM 策略分配给该组，然后创建一个属于该组 IAM 用户的 IAM 用户。为清晰起见，您可以将此 IAM 用户称为 **Bob**，因为 *Alice* 是开发账户的高级管理员。

要创建名为 Bob 的 IAM 用户，请按照 *第 2 章《使用 AWS Identity and Access Management 安全访问》中的练习 2.1* 中的说明进行操作，确保 IAM 用户的名称设置为 **Bob**。

创建 Bob IAM 用户后，请以根用户身份退出生产账户，然后以 Bob IAM 用户身份再次登录，然后再继续项目任务。

## 项目任务 13.1：为 VPC 堆栈创建 CloudFormation 模板

现在您可以按照以下步骤为 VPC 堆栈创建 CloudFormation 模板：

1. 以 Bob IAM 用户身份登录您的 Todo Plus 生产账户（按照 *技术要求* 部分中的说明进行操作）。
2. 在顶部搜索栏中搜索并导航到 CloudFormation 控制台。
3. 单击 **Create stack** 按钮。
4. 在 **Prerequisite – Prepare template** 部分，确保选择 **Choose an existing template**。
5. 在 **Specify template** 部分选择 **Update a template file** 选项。
6. 单击 **Choose file** 按钮下的 **Upload a template file**，如下图所示。

![](img/34ff9b5b5a734dae3450ab701e223c73_583_0.png)

图 13.12 – 选择 Choose file 按钮以上传您从 GitHub 仓库下载的模板

