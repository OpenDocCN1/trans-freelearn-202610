

## 基础设置

## 性能

## 卷

## 高级

## Docker 镜像

镜像路径

helloworld/withdocker

镜像标签

latest (default)

注意：如果您想使用私有仓库中的镜像，您首先需要提供您的 Docker Hub 凭证，否则部署将会失败，因为 sloppy.io 无法访问该镜像。

CMD 命令

输入可选 CMD 命令...

## 网络

域名 URI

http(s):// clojure2.sloppy.zone

暴露端口

Port #

注意您刚刚创建的镜像可以直接从 Docker Hub 拉取，以及您现在可以将最好的 sloppy 代码推送到野外：

![](img/d7b631352a782ee50160cf8eb995152f_671_0.png)

clojure.sloppy.zone

# Hello Remote Docker World

sloppy 的一些超级酷炫功能包括通过启动同一镜像的更多实例来实现按需性能：

![](img/d7b631352a782ee50160cf8eb995152f_672_0.png)

此外，重启容器将始终拉取最新的镜像，从而使从开发到生产的旅程几乎可以忽略不计：

![](img/d7b631352a782ee50160cf8eb995152f_672_1.png)

现在轮到你开发和部署了！

## Amazon Web Services 上的 Clojure

本教程是一个独立的菜谱，您可以在其中学习如何将 Clojure 的优雅与 Amazon Web Services (AWS) 相结合。

AWS 于 2006 年启动，并被许多企业用作易于使用的网络服务。这种无服务器服务风格变得越来越流行。您可以在无需自行准备硬件或安装软件的情况下，按需使用计算资源和软件服务。

您将主要使用 `amazonica` 库，它是整个 Amazon AWS API 集的综合 Clojure 客户端。该库封装了 Amazon AWS API，并支持大多数 AWS 服务，包括 EC2、S3、Lambda、Kinesis、Elastic Beanstalk、Elastic MapReduce 和 RedShift。

本教程的大部分内容和灵感来自 Robin Birtle，他是日本 Clojure 社区的主要成员。

## 准备工作

要使用 AWS，您需要一个 AWS 账户和凭证，因此本教程首先向您展示如何进行设置并获取开始所需的密钥。

## 在 AWS 上注册

如果您还没有 AWS 账户，您需要注册 AWS。在这种情况下，请访问 https://aws.amazon.com，点击 Sign In to the Console，然后按照创建账户的说明操作：

![](img/d7b631352a782ee50160cf8eb995152f_674_0.png)

要完成注册，请输入有效的信用卡号码和电话号码。

## 获取访问密钥和秘密访问密钥

要调用 AWS 客户端 API，您现在需要您的 AWS 访问密钥和秘密访问密钥。进入 AWS 控制台并点击屏幕右上角的名称，然后选择 Security Credentials，如以下屏幕截图所示：

选择 Access Keys (Access Key ID and Secret Access Key)，如以下屏幕截图所示：

# 您的安全凭证

使用此页面管理您的 AWS 账户的凭证。要管理 AWS Identity and Access Management (IAM) 用户的凭证，请使用 IAM 控制台。

要了解有关 AWS 凭证类型的更多信息以及它们的用途，请参阅 AWS General Reference 中的 AWS Security Credentials。

- Password
- Multi-Factor Authentication (MFA)
- Access Keys (Access Key ID and Secret Access Key)
- CloudFront Key Pairs
- X.509 Certificates
- Account Identifiers

然后，以下屏幕出现；点击 Create New Access Key：

![](img/d7b631352a782ee50160cf8eb995152f_676_0.png)

您可以查看您的访问密钥和秘密访问密钥，如以下屏幕截图所示：

![](img/d7b631352a782ee50160cf8eb995152f_677_0.png)

复制并保存这些字符串以供以后使用。

## 在 project.clj 中设置依赖项

让我们将 amazonica 库添加到您的 project.clj 并重启您的 REPL：

```
:dependencies [[org.clojure/clojure "1.8.0"]
[amazonica "0.3.67"]]
```

## 如何操作...

从这里开始，我们将通过 Clojure 和 amazonica 库访问的核心 Amazon 服务的一些示例用法。 我们将回顾的三个主要服务如下：

- 1. EC2，Amazon 的 Elastic Cloud，允许我们在 Amazon 云上运行虚拟机。
- 2. S3，Simple Storage Service，为您提供基于云的存储。
- 3. SQS，Simple Queue Services，为您提供基于云的数据

流式传输和处理。

让我们逐一介绍这些。

## 使用 EC2

假设您在东京区域有一个 EC2 微型实例：

![](img/d7b631352a782ee50160cf8eb995152f_678_0.png)

首先，我们将在 amazonica 中声明 core 和 ec2 命名空间：

```
(ns aws-examples.ec2-example
  (:require [amazonica.aws.ec2 :as ec2]
    [amazonica.core :as core]))
```

我们将设置访问密钥和秘密访问密钥以启用 AWS 客户端 API 访问。 core/defcredential 如下所示执行此操作：

```
(core/defcredential "Your Access Key" "Your Secret Access Key" "your region")
;;=> {:access-key "Your Access Key", :secret-key "Your Secret Access Key", :endpoint "your region"}
```

您需要指定的区域是 ap-northeast-1、ap-south-1 或 us-west-2。要获取完整的区域列表，请使用 ec2/describe-regions：

```
(ec2/describe-regions)
;;=> {:regions [{:region-name "ap-south-1", :endpoint "ec2.ap-south-1.amazonaws.com"}
;;=> .....
;;=> {:region-name "ap-northeast-2", :endpoint "ec2.ap-northeast-2.amazonaws.com"}
;;=> {:region-name "ap-northeast-1", :endpoint "ec2.ap-northeast-1.amazonaws.com"}
;;=> .....
;;=> {:region-name "us-west-2", :endpoint "ec2.us-west-2.amazonaws.com"}]}
```

ec2/describe-instances 返回大量信息：

```
(ec2/describe-instances)
;;=> {:reservations [{:reservation-id "r-8efe3c2b", :requester-id "226008221399",
;;=> :owner-id "182672843130", :group-names [], :groups [],
....
```

为了仅获取实例的必要信息，我们定义了以下 `get-instances-info`：

```
(defn get-instances-info[]
  (let [inst (ec2/describe-instances)]
    (->>
      (mapcat :instances (inst :reservations))
      (map
        #(vector
          [:node-name (->> (filter (fn [x] (= (:key x)) "Name") (:tags %)) first :value)]
          [:status (get-in % [:state :name])]
          [:instance-id (:instance-id %)]
          [:private-dns-name (:private-dns-name %)]
          [:global-ip (-> % :network-interfaces first :private-ip-addresses first :association :public-ip)]
          [:private-ip (-> % :network-interfaces first :private-ip-addresses first :private-ip-address)]))
        (map #(into {} %))
        (sort-by :node-name))))
;;=> #'aws-examples.ec2-example/get-instances-info
```

让我们尝试使用以下函数：

```
(get-instances-info)
;;=> ({:node-name "ECS Instance - amazon-ecs-cli-setup-my-cluster",
;;=> :status "running",
;;=> :instance-id "i-a1257a3e",
;;=> :private-dns-name "ip-10-0-0-212.ap-northeast-1.compute.internal",
;;=> :global-ip "54.199.234.18",
;;=> :private-ip "10.0.0.212"}
;;=> {:node-name "EcsInstanceAsg",
;;=> :status "terminated",
;;=> :instance-id "i-c5bbef5a",
;;=> :private-dns-name "",
;;=> :global-ip nil,
;;=> :private-ip nil})
```

如前面的示例函数所示，我们可以获取 instance-id 列表。因此，我们可以使用 ec2/start-instances 和 ec2/stop-instances 相应地启动/停止实例：

```
(ec2/start-instances :instance-ids '("i-c5bbef5a"))
;;=> {:starting-instances
;;=> [{:previous-state {:code 80, :name "stopped"},
;;=> :current-state {:code 0, :name "pending"},
;;=> :instance-id "i-c5bbef5a"}]}
(ec2/stop-instances :instance-ids '("i-c5bbef5a"))
;;=> {:stopping-instances
;;=> [{:previous-state {:code 16, :name "running"},
;;=> :current-state {:code 64, :name "stopping"},
;;=> :instance-id "i-c5bbef5a"}]}
```

## 使用 S3

Amazon S3 是 AWS 云中安全、耐用且可扩展的存储。对于开发人员和其他用户来说使用起来很容易。S3 还提供高耐用性、可用性和低成本。耐用性为 99.999999999 %，可用性为 99.99 %。

让我们按如下方式创建名为 makoto-bucket-1、makoto-bucket-2 和 makoto-bucket-3 的 s3 存储桶：

```
(s3/create-bucket "makoto-bucket-1")
```

