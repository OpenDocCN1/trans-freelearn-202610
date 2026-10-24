
云服务提供商甚至可以处理使用情况跟踪和计费，如果你决定将API背后的功能作为付费服务提供给客户的话。

## 云安全与分析

你可以使用主流云基础设施提供商支持的多种方法，将安全构建到分析中。

### 公钥/私钥

云服务提供商在其所有服务中使用非对称加密。系统会生成公钥和私钥。你保留私钥，因此服务方没有副本。服务方持有公钥。使用公钥/私钥的通信是安全的，并且从未被攻破。

云服务提供商即使把公钥刊登在明天的报纸上也无妨；仅凭公钥无法破解加密。公钥用于加密数据却不能用于解密，这看起来有违直觉，但确实可行。

每次访问以HTTPS开头的网站时，都在使用公钥/私钥加密。这是SSL和TLS加密的基础，而HTTPS通信正是采用这种加密方式。

在构建安全流程时，你会经常为物联网分析使用公钥/私钥。可以把它想象成你的分析工具的“用户名+密码”，但更胜一筹。密码学是一个引人入胜的学科（至少对我们中的一些人来说是这样）；想了解更多细节，不妨阅读相关资料。否则，你可以完全相信它没有问题——事实也确实如此！

### 公有子网与私有子网

当为分析搭建云环境时，你实际上是在创建自己的网络环境。你需要（或者希望由一位熟练的网络专家来）定义网络结构。其中一项基本组件就是子网的概念。子网是云环境中整个网络的逻辑细分。你将资源部署到某个子网中，该资源将遵循为该子网定义的互联网寻址规则。公有子网中的资源可以从外部互联网被寻址。但这并不意味着子网内的所有资源都能从外部被发现；你需要先为其分配公网IP地址。

私有子网无法从外部互联网寻址。虽然可以通过网关设备（通常是网络地址转换（NAT）设备）实现互联网通信，但外部对象无法直接与私有子网中的资源发起通信。

出于安全原因，大多数分析处理应在私有子网中进行，这给资源连接增加了一些复杂性。这正是我们在这里讨论它的原因。然而，为了防范恶意破坏，这样做完全值得。请了解你的哪些子网是私有的，并确保在那里启动新的资源。

### 访问限制

将分析资源的访问权限仅限制为指定的用户。避免使用单一的用户ID和密码让所有人共用的陷阱。对于通过公钥/私钥访问的资源，务必将私钥存放在安全可靠的地方。

对于网络安全，只允许必要的网络流量访问你的资源，阻止所有其他流量。从一开始就锁定一切，只在需要时才开放。

在分布式计算环境中（例如用于物联网分析的环境），网络是安全和问题排查的关键要素。你会遇到分析任务无法正常运行的情况，并发现根源是网络或安全相关设置。最好从分析项目一开始就理解这一点并采用最佳实践。

### 保护客户数据

确保物联网数据的安全，使用访问控制，并对数据文件进行加密。物联网数据可用来推断客户的有关信息，因此要特别小心。你是客户信息的守护者。如果没有妥善保护客户数据，你可能会无意中使公司面临法律风险。

加密可以通过两种方式实现：传输中加密和静态加密。传输中加密指网络传输，如SSL或TLS。大多数云服务都要求使用它们。确保支持分析流程的所有网络通信在传输中均经过加密。

静态加密指数据存储，可以是服务器上的数据文件，也可以是数据库中的数据。如果你使用公钥/私钥对来保护数据，云服务提供商可以为你加密数据，并且对你透明。

当数据可能被大量用户（例如公司全体员工）访问时，请采用加密。如果由于严格的访问控制，数据仅能被少数人访问，并且位于私有子网中，你可以稍微宽松一些——数据不太可能泄露出去。一般来说，如果加密是无缝的，就使用它。如果你的数据可通过公共互联网访问，那么一定要使用加密。

云服务提供商从诞生之初就在公共互联网环境中运营。安全内建于每一个层级。充分利用这一点，使用公钥/私钥、私有子网、访问控制和加密来保护你的宝贵数据。

## AWS 概览

在云基础设施领域，AWS是王者。它是同类服务中最早的，于2005年推出，并且遥遥领先于其他服务。在Gartner云基础设施提供商魔力象限的每一个细分领域，它都排名第一。

据《计算机世界》2016年报道，其计算能力是紧随其后的14家竞争对手总和的十倍。Netflix和AirBnB等整个公司都在其上运营业务。如下图所示，AWS拥有超过30%的市场份额，而最接近的竞争对手只有9%。

![](img/9f2a55db1236f995dcd9392acbc82910_95_0.png)

AWS提供从网络、计算到物联网的广泛服务。以下是AWS管理控制台中的服务列表。管理控制台是你启动新服务、监控现有服务以及查看账单的地方：

| 计算 | 迁移 | 安全、身份与合规 | 应用程序服务 |
|---|---|---|---|
| Amazon EC2 | AWS Database Migration Service | AWS Identity and Access Management (IAM) | AWS Step Functions |
| Amazon EC2 Container Registry | AWS Server Migration Service | Amazon Inspector | Amazon API Gateway |
| Amazon EC2 Container Service | AWS Snowball | AWS Certificate Manager | Amazon Elastic Transcoder |
| Amazon Lightsail | AWS Snowball Edge | AWS CloudHSM | Amazon AppStream |
| Amazon VPC | AWS Snowmobile | AWS Directory Service |  |
| AWS Batch |  | AWS Key Management Service | 消息 |
| AWS Elastic Beanstalk | 网络与内容分发 | AWS Organizations | Amazon SQS |
| AWS Lambda | Amazon VPC | AWS Shield | Amazon Pinpoint |
| Auto Scaling | Amazon CloudFront | AWS WAF | Amazon SES |
| Elastic Load Balancing | Amazon Route 53 | AWS Artifact | Amazon SNS |
|  | AWS Direct Connect |  |  |
| 存储 | Elastic Load Balancing | 分析 | 企业生产力 |
| Amazon Simple Storage Service (S3) | 开发人员工具 | Amazon Athena | Amazon WorkDocs |
| Amazon Elastic Block Storage (EBS) | AWS CodeCommit | Amazon EMR | Amazon WorkMail |
| Amazon Elastic File System (EFS) | AWS CodeBuild | Amazon CloudSearch |  |
| Amazon Glacier | AWS CodeDeploy | Amazon Elasticsearch Service | 桌面与应用流 |
| AWS Storage Gateway | AWS CodePipeline | Amazon Kinesis | Amazon WorkSpaces |
| AWS Snowball | AWS X-Ray | Amazon Redshift | Amazon AppStream 2.0 |
| AWS Snowball Edge | AWS Command Line Interface | Amazon QuickSight |  |
| AWS Snowmobile |  | AWS Data Pipeline | 软件 |
|  |  | AWS Glue | AWS Marketplace |
| 数据库 | 管理工具 |  |  |
| Amazon Aurora | Amazon CloudWatch | 人工智能 | 物联网 |
| Amazon RDS | Amazon EC2 Systems Manager | Amazon Lex | AWS IoT Platform |
| Amazon DynamoDB | AWS CloudFormation | Amazon Polly | AWS Greengrass |
| Amazon ElastiCache | AWS CloudTrail | Amazon Rekognition | AWS IoT Button |
| Amazon Redshift | AWS Config | Amazon Machine Learning |  |
| AWS Database Migration Service | AWS OpsWorks |  | 游戏开发 |
|  | AWS Service Catalog | 移动服务 | Amazon Lumberyard |
|  | AWS Trusted Advisor | AWS Mobile Hub |  |
|  | AWS Personal Health Dashboard | Amazon API Gateway |  |
|  | AWS Command Line Interface | Amazon Cognito |  |
|  | AWS Management Console | Amazon Pinpoint |  |
|  | AWS Managed Services | AWS Device Farm |  |
|  |  | AWS Mobile SDK |  |

AWS服务列表。来源：AWS管理控制台

你可以将这些服务归为三大类，需要正确配置以支持你的分析：网络、计算和存储。

网络包括安全并处理通信路由。你的服务需要能够相互通信。

