
![](img/da864e0670c65edf6f5ab861ef9906b3_46_0.png)

图 1.6：python-openstackclient 将传统的基于服务的客户端整合到一个具有统一命令结构的 shell 中

你也可以通过运行以下命令查看帮助命令：

```
$ openstack help
```

![](img/da864e0670c65edf6f5ab861ef9906b3_46_1.png)

OpenStack 客户端将在后续章节中更详细地讨论。截至 2017 年 9 月，COA 考试基于 OpenStack 的 Newton 版本。考试将提供对 python-openstackclient 和所有基于服务的客户端的访问。大多数考试目标都可以使用 python-openstackclient 完成，除非本书另有说明。

## OpenStack 守护进程通信

现在我们已经讨论了 OpenStack 操作员通过 API 与 OpenStack 交互的各种方式，让我们讨论核心 OpenStack 服务之间的内部通信。在图 1.7 中，你可以看到左上角的颜色代码，它显示了服务和守护进程。服务只是 OpenStack 服务的名称，而守护进程代表实际运行以使服务生效的程序。

为避免紧密耦合，特定服务内守护进程之间的通信通过高级消息队列协议（AMQP）实现。在典型的 OpenStack 环境中，这可以是 RabbitMQ 或 Qpid 等软件。

回顾我们在本章历史部分讨论的云化开发方法。虽然 OpenStack 鼓励其软件开发人员在部署应用程序时遵循这种方法，但实际的 OpenStack 基础设施遵循完全相同的原则：高可用、可扩展且松耦合。

例如，当用户向 `nova-api` 发送启动服务器的请求时，`nova-api` 会将此消息发布到 AMQP 总线上。然后 `nova-scheduler` 会读取此消息。现在让我们设想一个经常收到数百个启动实例请求的 OpenStack 云。为了解决这个问题，你将在基础设施中部署更多服务器，安装更多 `nova-scheduler` 守护进程，并简单地将它们指向 OpenStack 环境的 AMQP 服务器。这就是水平扩展的实际应用！

![](img/da864e0670c65edf6f5ab861ef9906b3_47_0.png)

图 1.7：特定服务内的 OpenStack 守护进程使用 AMQP 总线相互通信

## OpenStack API 通信

虽然 OpenStack 服务守护进程依赖 AMQP 在其特定服务内的守护进程之间进行通信，但 API 既被希望创建虚拟资源的 OpenStack 用户使用，也用于服务到服务的通信。

![](img/da864e0670c65edf6f5ab861ef9906b3_48_0.png)

图 1.8：OpenStack 服务之间 API 通信的示例

设想用户向 nova-api 发送启动实例的请求。为了启动该服务器，需要进行相当多的内部 API 调用。让我们看一下启动虚拟机实例时将发生的 API 调用示例序列。

OpenStack 用户首先会向 `nova-api` 守护进程发送 POST API 调用，其中包含有关他们想要的虚拟机类型的详细信息。该消息将被放入 AMQP 消息总线并由 `nova-scheduler` 守护进程消费。`nova-scheduler` 守护进程然后将使用其调度算法为虚拟机选择最佳匹配。驻留在计算节点上的 `nova-compute` 守护进程会消费此消息。在服务器可以启动之前，必须发生以下情况（参见图 1.8）：

- (1)：`nova-compute` 守护进程向 Keystone 发送 API 请求以检索作用域令牌。
- (2)：`nova-compute` 收到 200 HTTP 响应以及作用域令牌。现在 `nova-compute` 拥有作用域令牌，可以自由地与其他 OpenStack 服务通信。
- (3)：`nova-compute` 现在向 `neutron-server` 发送请求，以验证用于启动实例的指定虚拟网络是否可用。
- (4)：`nova-compute` 收到网络可用且能够附加到实例的响应。
- (5)：`nova-compute` 然后必须向 `glance-api` 发送 API 调用以检索操作系统镜像。
- (6)：`glance-api` 返回将驻留在计算节点上的镜像。

现在我们的实例将被启动！这工作量很大！

![](img/da864e0670c65edf6f5ab861ef9906b3_49_0.png)

别担心！你不需要理解 OpenStack 背后发生的所有魔术就能在考试中取得成功。这仅用于教育目的！

## 关于 Certified OpenStack Administrator 考试

现在你已经了解了一些关于 OpenStack 的知识，让我们来谈谈你真正在这里的原因：

通过 Certified OpenStack Administrator 考试！

## 通过考试的好处

问任何人关于如何在 IT 世界起步，他们可能会建议了解行业认可的技术认证。IT 认证衡量多个领域的胜任能力，是开启机会大门的绝佳方式。虽然它们绝对不应是招聘过程中的唯一决定因素，但获得它们可以衡量你的能力以及面对挑战的决心。

## 如果你通过...

一旦取得及格成绩，你将收到你的证书（参见图 1.9）。将其覆膜、装框或别在家庭办公室墙上或工作隔间——这是你已满足成为正式 OpenStack 管理员所有要求的证明。认证自通过之日起有效期为三年，因此不要忘记续期。

![](img/da864e0670c65edf6f5ab861ef9906b3_50_0.png)

图 1.9：通过 Certified OpenStack Administrator 考试时收到的证书

除了认证之外，COA 徽章将出现在你的名字旁边，位于官方 OpenStack 社区目录中，可在 https://www.openstack.org/community/members/ 获取。

![](img/da864e0670c65edf6f5ab861ef9906b3_51_0.png)

图 1.10：通过考试后，你还将在 OpenStack 基金会成员目录中在你的名字旁边获得官方 COA 徽章

![](img/da864e0670c65edf6f5ab861ef9906b3_51_1.png)

OpenStack 基金会提供了一个很好的工具来帮助雇主验证 COA 认证的有效性。查看 Certified OpenStack Administrator 验证工具（https://www.openstack.org/coa/coa-verify/）。

## 成为 Certified OpenStack Administrator 的七个步骤

让我们开始逐步了解成为 Certified OpenStack 管理员的一些步骤。

## 第 1 步 - 学习！

练习。练习。练习。在你开始 Certified OpenStack Administrator 之旅时，使用本书和包含的 OpenStack 一体化虚拟设备作为资源。如果你仍然发现自己难以掌握本书中的概念和目标，你始终可以参考官方 OpenStack 文档——甚至可以参加 https://www.openstack.org/marketplace/training/ 的现场培训课程。

## 第 2 步 - 购买

一旦你觉得自己准备好征服考试，请前往 https://www.openstack.org/coa/ 并点击 Get Started。登录后，你将被引导至结账页面购买考试。OpenStack 基金会接受所有主要信用卡，截至 2017 年 9 月费用为 300.00 美元（可能变动，请留意网站）。如果你第一次未通过，也可以在原考试购买日期起 12 个月内免费重考一次。

![](img/da864e0670c65edf6f5ab861ef9906b3_52_0.png)

为了鼓励学术界的学生接触 OpenStack 技术，OpenStack 基金会为持有有效学生证的学生提供考试优惠，价格为 150.00 美元（零售价的 50% 折扣）。请访问 https://www.openstack.org/coa/student/ 了解更多信息。

## 第 3 步 - COA 门户页面

一旦你的订单处理完毕，你将收到一封电子邮件，其中包含访问 COA 门户的权限。将门户视为你的个人 COA 网站，你可以在此下载考试收据并跟踪你的认证进展。考试后，你可以返回 COA 门户检查考试状态和分数，甚至可以下载证书和徽章用于你的网站或名片。

## 第 4 步 - 硬件兼容性检查

COA 考试可以在你的个人笔记本电脑或台式机上进行，但你必须确保你的系统满足考试的最低系统要求。COA 门户页面上的链接将为你提供兼容性检查工具，该工具将运行一系列测试以确保你满足要求。它还将帮助你下载用于参加考试的 Chrome 插件。目前，你必须使用 Chrome 或 Chromium 浏览器，并拥有可靠的互联网、网络摄像头和麦克风。
