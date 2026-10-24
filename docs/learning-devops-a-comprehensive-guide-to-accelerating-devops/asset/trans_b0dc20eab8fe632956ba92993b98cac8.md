

## 第 5 章：使用 Vagrant 构建开发环境

- 1. Vagrant 的作用是创建本地开发环境。
- 2. 创建虚拟机的 Vagrant 命令是 `vagrant up`。
- 3. 通过 SSH 连接到虚拟机的 Vagrant 命令是 `vagrant ssh`。

## 第 6 章：使用 Git 管理源代码

- 1. Git 是一个分布式版本控制系统（DVCS）。
- 2. 初始化仓库的命令是 `git init`。
- 3. 工件是由保存部分代码组成的提交。
- 4. 允许将代码保存到本地仓库的命令是 `git commit`。
- 5. 允许将代码发送到远程仓库的命令是 `git push`。
- 6. 允许从远程仓库更新本地仓库的命令是 `git pull`。
- 7. 分支是允许隔离代码的机制。
- 8. GitFlow 是 Git 中的分支管理模型。

## 第 7 章：持续集成与持续交付

- 1. 构建 CI 流水线的前提是将代码放在源代码管理器中。
- 2. 每当团队成员提交/推送代码时，CI 流水线都会被触发。
- 3. 包管理器是一个中央仓库，用于集中和共享包、开发库、工具或软件。
- 4. NuGet 包管理器允许存储 .NET 库/框架。
- 5. Azure Artifacts 集成到 Azure DevOps 中。
- 6. 不，它是一个本地工具，需要安装在服务器上。
- 7. 在 Azure DevOps 中，管理 CI/CD 流水线的服务是 Azure Pipelines。
- 8. GitLab 服务包括源代码管理器、CI/CD 流水线管理器和用于项目管理的看板。
- 9. 在 GitLab CI 中，CI 流水线是通过名为 `.gitlab-ci.yml` 的 YAML 文件构建的。

## 第 8 章：使用 CI/CD 流水线部署基础设施即代码

- 1. 使用的工具是 Azure Pipelines。
- 2. 供应顺序是 Packer，然后是 Terraform，最后是 Ansible。

## 第 9 章：使用 Docker 容器化应用程序

- 1. Docker Hub 是 Docker 镜像的公共注册表。
- 2. 基本元素是 Dockerfile。
- 3. 指令是 FROM。
- 4. 创建 Docker 镜像的命令是 `docker build`。
- 5. Docker 容器的实例化命令是 `docker run`。
- 6. 发布镜像的 Docker 命令是 `docker publish`。

## 第 10 章：使用 Kubernetes 有效管理容器

- 1. Kubernetes 的作用是管理容器。
- 2. 在 Kubernetes 中，所有对象都写在 YAML 规范文件中。
- 3. Kubernetes CLI 工具称为 `kubectl`。
- 4. 在 K8s 中应用部署的命令是 `kubectl apply`。
- 5. Helm 是 Kubernetes 的包管理器。
- 6. Azure Kubernetes Services 是 Azure 中的托管 Kubernetes 集群。

## 第 11 章：使用 Postman 测试 API

- 1. Postman 是一个允许执行 API 测试的工具。
- 2. 要创建的第一个元素是集合。
- 3. 在 Postman 中，API 配置位于请求中。
- 4. 集合运行器允许执行集合中的所有请求。
- 5. Newman 是一个命令行工具，用于在 CI/CD 流水线中执行 Postman 测试。

## 第 12 章：使用 SonarQube 进行静态代码分析

- 1. SonarQube 是用 Java 开发的。
- 2. 安装 SonarQube 时，需要安装 Java。
- 3. SonarQube 是一个用于静态代码分析的工具。
- 4. SonarLint 允许开发人员在编写代码时进行代码分析。

## 第 13 章：安全与性能测试

- 1. 不。ZAP 不是用于分析应用程序源代码的工具。
- 2. 在 Postman 中，性能指标是每个请求的执行时间。

## 第 14 章：DevOps 流程中的安全性与 DevSecOps

- 1. 它的作用是测试系统或基础设施的合规性。
- 2. 包管理器是 Gem。
- 3. 命令是 `inspec exec`。
- 4. Vault 由 HashiCorp 编辑。
- 5. 命令是 `vault server -dev`。
- 6. 不。本地安装时，它只能用于开发和测试。
- 7. 在这种模式下，数据存储在内存中。

## 第 15 章：减少部署停机时间

- 1. 减少停机时间的 Terraform 选项是 `create_before_destroy`。
- 2. 蓝绿部署基础设施由一个蓝色环境、一个绿色环境以及一个路由器或负载均衡器组成。
- 3. 两种模式都是金丝雀发布和暗部署。
- 4. 允许蓝绿部署的 Azure 组件是应用服务部署槽位和 Azure 流量管理器。
- 5. 功能标志的作用是启用或禁用应用程序的功能，而无需重新部署。
- 6. FeatureToggle 是一个简单的功能标志，是 .NET 应用程序的开源框架。
- 7. LaunchDarkly 是一个 SaaS 解决方案，可让您摆脱任何安装。

## 第 16 章：开源项目的 DevOps

- 1. 不，要修改另一个仓库的代码，您需要创建该仓库的分支。
- 2. 允许合并的元素是拉取请求。
- 3. `CHANGELOG.md` 文件允许显示发行说明。
- 4. 发行版与 Git 标签相关联。
- 5. 在 Travis CI 中，作业的配置写入 YAML 文件。
- 6. 两个工具是 SonarCloud 和 WhiteSource Bolt。
- 7. 安全漏洞列在 **Issues** 选项卡中。

## 第 17 章：DevOps 最佳实践

- 1. 部署自动化消除手动错误并缩短部署周期。
- 2. 不。任何允许您编写基础设施配置脚本的工具都可以使用。
- 3. 安全团队必须与 Dev 和 Ops 集成。
- 4. 不，监控涉及应用程序、基础设施和 CI/CD 流水线。
- 5. 在应用程序中，最好将应用程序的功能或领域分开，以便拥有易于部署的代码。
- 6. 在 DevOps 组织结构中，团队是多学科的。

## 索引

## A

- 敏捷方法
- 参考链接 506
- Amazon Elastic Container Registry (ECR)
- 参考链接 280
- Amazon Web Services (AWS) 263, 412
- Ansible
- 配置 73-75
- 执行 85, 86
- 安装 69
- 安装，使用脚本 69-71
- 集成，到 Azure
- Cloud Shell 71, 72
- 预览/干运行选项，使用 87, 88
- 运行，在 Azure Pipelines 中 252-257
- 使用，在 Packer 模板中 120
- 变量，使用进行配置 89-92
- Ansible 工件
- hosts 73
- inventory 73
- playbook 73
- ANSIBLE_CONFIG 环境变量
- 参考链接 74
- Ansible 配置键
- 参考链接 75

- Ansible 执行
- 日志级别输出，增加 88
- Ansible Galaxy
- URL 84
- Ansible，在本地安装
- VirtualBox 环境
- 参考链接 70
- Ansible，在操作系统上安装
- 参考链接 70
- Ansible，集成到
- Azure Cloud Shell
- 参考链接 72
- Ansible inventory
- 创建 76
- 动态 inventory 76
- 参考链接 78
- 静态 inventory 76
- ansible-local provisioner
- 参考链接 122
- Ansible modules
- 关于 82
- 参考链接 82
- Ansible playbook
- 集成，在 Packer 模板中 121, 122
- 参考链接 121
- 编写 120, 121

## 索引

- ansible-playbook 命令
- 参考链接 88
- Ansible provisioner
- 参考链接 122
- Ansible variables
- 参考链接 92
- Ansible Vault
- 关于 92
- 敏感数据，使用保护 93, 94
- 使用，用于保护数据 89
- 应用程序编程
- 接口 (API) 295, 421
- apply 命令行
- 参考链接 53
- App Service slots
- 使用 446-448
- Artifact Hub
- 公共 Helm chart，使用 317-320
- URL 317
- Artifactory from JFrog
- 参考链接 280
- 自动 CI/CD 过程 58
- 自动模式 94
- 自动化最佳实践 498
- AWS Secrets Manager
- 参考链接 421
- az cli 命令
- 身份验证信息 415
- 参数 414
- az resource
- 参考链接 96
- Azure
- 蓝绿部署，应用 446
- 配置，用于 InSpec 414, 415
- SonarQube，安装 380-384
- Terraform，配置 36
- Azure Active Directory (AD) 36
- Azure AD Service Principal (SP) 116

- Azure Artifacts
- 关于 198
- 优势 198
- Azure CLI 命令
- 参考链接 37
- Azure CLI 工具 414
- Azure Cloud Shell
- Ansible，集成到 71, 72
- Packer，集成与 108
- 参考链接 34
- 使用，用于集成 Terraform 34, 35
- Azure Cloud Shell 工具套件 414
- Azure Container Instances (ACI)
- 容器，使用部署
- CI/CD 流水线到 282
- 容器 291
- Docker Compose 容器，
- 部署 299-301
- 参考链接 282
- Terraform 代码，编写 283-285
- Azure Container Registry (ACR)
- 关于 269
- 参考链接 280
- Azure DevOps
- 关于 208
- 功能 208
- 参考链接 209
- 注册与 209
- Azure 环境变量用于 Ansible
- 参考链接 95
- Azure 镜像
- 构建，使用 Packer 模板 116-119
- Azure 基础设施
- 部署，通过编写
- Terraform 脚本 40-45
- 动态 inventory，使用用于 95-99
- Azure 基础设施合规性
- 测试，使用 Chef InSpec 411

