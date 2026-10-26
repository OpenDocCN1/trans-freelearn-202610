
AWS 提供现成的云组件，例如默认 VPC，让用户能够在其 AWS 账户中快速上手。

## 使用脚本自动化基础设施部署

一旦用户熟悉了 AWS 服务，并理解了将多个服务捆绑在一起以解决业务用例的思路，他们就会开始使用 CLI 命令和 AWS SDK 从终端触发一些操作。如果这些操作遵循特定顺序，也可以使用 Bash 脚本等将它们串联起来。这些脚本还可以在自动化软件交付流水线中使用，借助 Jenkins 和 AWS CodeBuild 等工具。

## 采用声明式方法

当需求超出基本的脚本解决方案时，组织通常会借助更高级的服务（如 CloudFormation 和 Terraform）迈出下一步。在某些情况下，根据公司现有的经验和知识，他们甚至可能直接采用这些服务。

这些服务为基础设施定义提供声明式方法，用户只需定义其 AWS 账户的*目标状态*。然后工具会利用底层服务 API 使这一声明的状态变为现实。要达到这种目标状态，需要在状态管理、解决更新冲突、依赖解析等方面进行大量幕后“魔法”般的工作。所有这一切对最终用户都是透明的。

要开始使用这些工具，用户需要熟悉相应的配置语言和模板结构。需要注意的是，这些服务不会超出模板文件中所写的内容。因此，用户仍然需要在最细粒度上定义架构的所有组件，并将它们组合起来，使其作为一个解决方案协同工作。

以下是以声明式方式定义 EC2 实例的两个配置示例：

- 使用 Terraform：

```
```hcl
resource "aws_instance" "web" {
  ami           = "ami-a1b2c3d4"
  instance_type = "t3.micro"
}
```
```

- 使用 CloudFormation：

```
```yaml
EC2Instance:
  Type: AWS::EC2::Instance
  Properties:
    ImageId: "ami-a1b2c3d4"
    InstanceType: t3.micro
```
```

修改这些属性中的任何一个（如 ImageId 或 ami）都会导致实例更新，但这完全由相应的工具处理，而用户只需专注于定义基础设施的目标状态。

## 使用基础设施定义生成器

与服务原生的配置语言不同，还有一类工具提供了使用常规编程语言（如 Python 或 Golang）生成 AWS CloudFormation 模板的灵活性。当组织对特定编程语言具有较高的成熟度和熟练度时，这些工具会很有用。

Troposphere 和 GoFormation 等工具在该领域广为人知，它们可以根据用户编写的代码动态生成立即可用的 CloudFormation 模板。

让我们看看如何用 Troposphere 定义配置：

```
>>> from troposphere import Ref, Template
>>> import troposphere.ec2 as ec2
>>> t = Template()
>>> instance = ec2.Instance("myinstance")
>>> instance.ImageId = "ami-951945d0"
>>> instance.InstanceType = "t1.micro"
>>> t.add_resource(instance)
<troposphere.ec2.Instance object at 0x101bf3390>
>>> print(t.to_json())
{
    "Resources": {
        "myinstance": {
            "Properties": {
                "ImageId": "ami-951945d0",
                "InstanceType": "t1.micro"
            },
            "Type": "AWS::EC2::Instance"
        }
    }
}
>>> print(t.to_yaml())
Resources:
  myinstance:
    Properties:
      ImageId: ami-951945d0
      InstanceType: t1.micro
    Type: AWS::EC2::Instance
```

采用此类工具的团队仍然需要了解 AWS CloudFormation 等服务在底层如何运作，以及它们向最终用户暴露了哪些构造（构建块）等。

## 使用提供高层抽象的框架

一旦用户熟悉了 AWS CloudFormation 等服务，并在调试生产级部署方面积累了丰富经验，他们可能会考虑采用 **AWS CDK** 和 **Pulumi** 等工具。AWS CDK 提供了一种使用通用编程语言（如 TypeScript、Python、Go、Java 等）管理基础设施的方法。作为 AWS 的官方产品，它在长期承诺、支持和云提供商的开发方面具有一定优势。

活跃在该领域的另一个著名 IaC 平台是 Pulumi。Pulumi 和 AWS CDK 在运作方式上存在一些关键差异，但从最终用户的角度来看，功能大体相同——使用常用编程语言进行基础设施资源管理。Pulumi 与 AWS CDK 一样，也提供了与通用编程语言的集成，但这种支持不仅限于 AWS。它还提供了 60 多种与其他云或 SaaS 提供商的集成。

## Pulumi 与 CDK 对比

在底层，AWS CDK 更像是一个**转译器**。转译器是将一段源代码从一种高级语言转换为另一种高级语言的程序。这些程序的输出与输入具有相同级别的抽象。这与编译器有着根本区别，因为编译器生成的是与底层机器直接交互的低级代码，而且性能更高。

另一方面，Pulumi 不依赖中间状态（例如 AWS CDK 中的 AWS CloudFormation 模板），从而与编程运行时和云提供商进行了更深入的集成。Pulumi 负责管理用户定义目标状态的资源的生命周期。这种集成级别还带来了额外的好处，可以通过利用模拟响应和其他构造来实现高效的单元测试和集成测试。

图 6.1 展示了不同基础设施管理方法及相应工具和框架的可视化映射：

> **注意**
> 本部分中的步骤已经为你预先在 `chapter-6/` 目录中执行过了，因此你只需浏览一下即可理解。

让我们在 Cloud9 IDE 上启动一个新会话，并在终端中切换到 `chapter-6/` 目录，然后再开始 CDK 项目初始化过程：

```
aws-devops-simplified:~/environment/chapter-6 $ npx aws-cdk init
--language typescript
npx: installed 1 in 8.495s
Applying project template app for typescript
# Welcome to your CDK TypeScript project
This is a blank project for CDK development with TypeScript.
The `cdk.json` file tells the CDK Toolkit how to execute your app.
## Useful commands
* `npm run build`    compile typescript to js
* `npm run watch`    watch for changes and compile
* `npm run test`     perform the jest unit tests
* `cdk deploy`       deploy this stack to your default AWS account/region
* `cdk diff`         compare deployed stack with current state
* `cdk synth`        emits the synthesized CloudFormation template
Initializing a new git repository...
...
...
```

`cdk init` 会使用目录名称来为大多数内容（类、子文件夹等）生成动态前缀或后缀。

> **提示**
> 你可能已经注意到，我们没有直接使用 cdk 命令行，而是使用了 `npx aws-cdk`。这种做法可以让你将这些组件固定到特定版本，并确保每个开发者都使用相同的版本。一般来说，这些组件的新版本总是向后兼容的，因此出现问题的可能性很小。

一旦引导过程完成，你将在项目目录中看到以下文件夹和文件的层级结构。我添加了一些描述，以突出每一项的意义：

```
aws-devops-simplified:~/environment/chapter-6 $ tree -L 1 .
.
├── README.md
├── bin                >> (Application entry point)
├── cdk.json           >> (CDK project configurations)
```
