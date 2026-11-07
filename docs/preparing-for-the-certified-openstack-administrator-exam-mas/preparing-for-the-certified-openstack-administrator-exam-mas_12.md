# 12 模拟考试

使用此练习测试为 Certified OpenStack Administrator 认证考试做准备。

### 开始之前...

请确保在开始之前删除任何先前正在运行的实例。如果您需要帮助检测正在运行的实例并删除它们，请参阅第 5 章《Nova 计算服务》的末尾。

### 指示

在 Horizon 仪表板或 CLI 上完成所有目标。某些问题可能取决于其他问题中创建的资源。能够在一小时内完成所有这些目标应该能让您在考试中处于良好状态。如果未提供特定细节，请假定默认值。

### 问题 1

在 **会计** 项目中，创建一个名为 **租户网络1** 的新租户网络。它应具有名为 **租户子网1** 的子网，具有以下详细信息：

- CIDR：192.168.5.0/24
- DHCP：已启用
- 分配范围：192.168.5.50 至 192.168.5.60

### 问题 2

从您的终端环境内部，为将作用域限定为 auto 项目的管理员用户构建一个 openrc 凭据文件。

### 问题 3

在 accounting 项目中创建一个名为 keypair1 的新密钥对。

### 问题 4

您的基础设施工程师刚刚在您的 OpenStack 环境中安装了 Manilla 服务。在服务目录中创建一个新的条目，宣传以下信息：

- 服务名称：manilla
- 服务描述：OpenStack Shared File Systems
- 类型：share
- 区域：RegionOne
- 公共端点：http://192.168.56.56:8786/v1/
- 内部端点：http://192.168.56.56:8786/v1/
- 管理端点：http://192.168.56.56:8786/v1/

### 问题 5

在 trade 项目中创建一个名为 database-sg 的新安全组。它应有一条规则，允许从默认安全组传入 3306。

### 问题 6

在 accounting 项目中启动一个名为 instance1 的新实例，使用 m1.tiny 规格。它应使用 Cirros 镜像并应连接到 tenant-network1。确保 keypair1 公钥被注入到实例中。

### 问题 7

创建一个新的 Nova 规格，只能由 **生产** 项目访问。它应具有以下属性：

- **名称**：nice-flavor
- **RAM**：512 MB
- **VCPU**：1
- **磁盘**：1 GB
- **临时存储**：0
- **交换**：0

### 问题 8

在 **会计** 项目中，创建一个新的 1 GB 空卷，名为 **卷1**。将卷附加到 **实例1**。成功附加卷后，使用 novnc 控制台登录到实例 [用户名：cirros，密码：cubswin:]。使用 ext3 文件系统格式化附加的卷并挂载到 **/mnt** 目录。在挂载的驱动器上放置一个名为 `myfile.txt` 的文件。卸载并从 **实例1** 断开卷的连接。

### 问题 9

从 **自动** 项目创建空的 1 GB 卷，名为 **卷1**。卷应为类型：lvm。

### 问题 10

调整 **管理员** 项目中的配额，允许总共 20 个 **实例** 和 100 个 **卷**。

### 问题 11

将 **sallyp** 用户的电子邮件地址更改为 `sallyp@openstack.org`。禁用该帐户。

### 问题 12

在 accounting 项目中创建 instance1 的 Nova 快照。快照应命名为 instance1-snapshot。

### 问题 13

在 accounting 项目中创建一个名为 router1 的路由器。将 tenant-subnet1 添加到路由器接口。设置路由器网关为 provider-network-dashboard。

![](img/da864e0670c65edf6f5ab861ef9906b3_327_0.png)

如果您看不到 provider-network-dashboard，请前往第 6 章《Neutron 网络服务》并按照练习创建它。

### 问题 14

在 accounting 项目中分配浮动 IP 地址 172.16.0.99。将此浮动 IP 分配给 instance1。

### 问题 15

尝试通过上一题中设置的浮动 IP 地址（172.16.0.99）ping instance1 以获取回复。如果未收到回复，请在默认安全组中设置特定的安全组规则以允许此操作。

![](img/da864e0670c65edf6f5ab861ef9906b3_327_1.png)

您必须从虚拟设备内部 ping 该实例，而不是运行 VirtualBox 的笔记本电脑或桌面系统。

### 问题 16

仅在 CLI 上执行此特定任务。使用 wget 从官方上游网站 https://download.cirros-cloud.net/0.3.5/cirros-0.3.5-x86_64-disk.img 获取 Cirros 镜像。将 qcow2 镜像上传到 trade 项目。镜像应命名为 awesome-cirros。镜像描述应为“COA Exam Prep Awesome Image”。镜像的最小 RAM 应为 256，最小磁盘应为 1 GB。它只能由 trade 项目访问。




## 问题 17

在 auto 项目中创建一个名为 stuff 的新 Swift 容器。

将名为 object.txt 的文件上传到 stuff 容器。容器内容应可被公众读取/下载，但只有 accounting 项目中的 lisaw 可以写入。

## 问题 18

限定在 trade 项目范围内，使用 CLI 将 awesome-cirros 镜像下载到 /tmp 目录。

## 问题 19

在 accounting 项目中根据以下模板创建一个 Heat 堆栈：

```
heat_template_version: 2013-05-23
description: Test Stack
parameters:
  ServerName:
    type: string
    description: A name for the server
  Image:
    type: string
    description: Image ID used to boot the server
  Flavor:
    type: string
    description: Flavor of server
  Net:
    type: string
    description: Network ID for the server
  VolumeName:
    type: string
    description: A name for the volume

resources:
  server1:
    type: OS::Nova::Server
    properties:
      name: { get_param: ServerName }
      image: { get_param: Image }
      flavor: { get_param: Flavor }
      networks:
      - network: { get_param: Net }
  volume1:
    type: OS::Cinder::Volume
    properties:
      name: { get_param: VolumeName }
      size: 1

  volumeattach1:
    type: OS::Cinder::VolumeAttachment
    properties:
      volume_id: { get_resource: volume1 }
      instance_uuid: { get_resource: server1}
```

堆栈名称应为 test-accounting-stack。使用 Cirros 镜像、m1.tiny 规格，并连接到 tenant-network1 网络。创建的实例应命名为 heat-instance。卷应命名为 volume1。

## 问题 20

在 auto 项目中，将 stuff 容器中的 object.txt 文件的过期时间设置为 1596822650（2020 年 8 月 7 日星期五，下午 5:50:50）。

## 问题 21

创建一个名为 zevotoys 的新域。

## 问题 22

在此域中创建一个名为 maryw 的新用户，密码设置为 password。

## 问题 23

在 zevotoys 域中创建一个名为 marketing 的项目。

## 问题 24

在 zevotoys 域中创建一个名为 students 的新组。将 maryw 添加到该组。

## 问题 25

将 _成员_ 角色分配给 marketing 项目上的 students 组。

## 问题 26

将 awesome-cirros 镜像（在 trade 项目中）与 quality-control 项目共享。

## 问题 27

在 admin 项目中创建新的 provider 网络和子网，详细信息如下：

- **网络名称**：provider-network-practice-test
- **Provider 网络类型**：Flat
- **物理网络**：public-test
- **外部**：true
- **子网名称**：provider-subnet-practice-test
- **子网范围**：172.16.2.0/24
- **网关**：172.16.2.1
- **DHCP**：已禁用

## 问题 28

在 auto 项目中创建 volume1 的快照。快照名称为 volume1-snapshot。描述应为：COA Prep volume1-dashboard snapshot。

## 问题 29

在 procurement 项目中，尝试创建另一个名为 default 的安全组。你应该会收到一个包含请求 ID 的错误。在 neutron 日志文件中查找有关此错误的任何信息并记录你找到的信息。

## 问题 30

将 accounting 项目中 tenant-subnet1 的分配池调整为 192.168.5.55-192.168.5.60。

## 问题 31

在 human-resources 项目中，从现有的 volume1-dashboard 卷创建可启动卷。

## 问题 32

从 accounting 项目，将 volume1 卷备份到名为 accounting-volume-backups 的容器。

## 问题 33

通过其浮动 IP 172.16.0.99 SSH 进入 accounting 项目中的 instance1。如果无法连接到实例，请打开必要的 security group 规则。如果能使用问题 3 中生成的私钥无密码 SSH 进入实例，可获得额外加分。

## 问题 34

更新 accounting 项目中的 test-accounting-stack，使其使用镜像 instance1-snapshot（在问题 12 中创建）。确保不销毁现有堆栈，只更新镜像。镜像成功更新后，删除整个堆栈。

## 问题 35

在 accounting 项目中，创建另一个名为 tenant-network2 的租户网络。它应有一个名为 tenant-subnet2 的子网，详细信息如下：
- CIDR：192.168.6.0/24
- DHCP：已启用
- 分配范围：192.168.6.50 至 192.168.6.60

网络和子网创建后，在 accounting 项目中启动一个名为 instance2 的新实例，使用 m1.tiny 规格。它应使用 Cirros 镜像并连接到 tenant-network2。实例启动后，使用 Horizon 控制台仪表板通过其固定 IP 地址 ping instance1。你可能需要创建一个路由器以允许 tenant-network1 和 tenant-network2 之间的流量。

## 索引

### A

- 操作，OpenStack 服务
  - 访问和安全 25
  - 管理 25
  - 标识 25
  - 镜像 25
  - 实例 24
  - 网络 25
  - 对象存储 25
  - 编排 25
  - 卷 25
- 管理面板组 72
- 管理员用户
  - 关于 72
- 使用 Identity 面板 73
- 高级消息队列协议 (AMQP) 30
- 一体化 Newton 环境
  - 关于 48
  - 硬件要求 50
- Amazon
  - 关于 12
  - 缺点 14
- API
  - 用于检查组件状态 293
- appliance
  - 在 Linux 上执行 56
  - 在 Windows 上执行 56
  - 导入 54
  - 在 mac 上 56
- 使用 Horizon 仪表板验证 OpenStack 连接性 58
- 验证 OpenStack 服务连接性
  - CLI 57
  - 使用 SSH 56
- 架构，Cinder
  - cinder-api 228
  - cinder-backup 229
  - cinder-scheduler 229
  - cinder-volume 229
- 架构，Heat
  - heat-api 272
  - heat-api-cfn 272
  - heat-engine 272
- 架构，Neutron
  - neutron-dhcp-agent 161
  - neutron-l3-agent 161
  - neutron-linuxbridge-agent 161
  - neutron-metadata-agent 161
  - neutron-server 161
- 架构，Nova
  - nova-compute 128
  - nova-conductor 128
  - nova-consoleauth 128
  - nova-novncproxy 128
  - nova-scheduler 128
- 架构，Swift
  - swift-account-auditor 254
  - swift-account-reaper 254
  - swift-account-replicator 254
  - swift-account-server 254
  - swift-container-auditor 254
  - swift-container-replicator 254
  - swift-container-server 253
  - swift-container-sync 254
  - swift-container-updater 254
  - swift-object-auditor 253
  - swift-object-expirer 253
  - swift-object-replicator 253
  - swift-object-server 253
  - swift-proxy-server 253
- 身份验证
  - 关于 62, 69
  - 项目范围，通过仪表板选择 71
- 授权 62

### B

- 可启动 235
- 启动 21

### C

- Certified OpenStack Administrator 考试
  - 关于 32
  - 优势 33
  - URL 34
- Certified OpenStack Administrator，考试目标
  - 关于 39
  - 块存储 41
  - 验证仪表板操作 42
  - 身份管理 40
  - 镜像管理 40
  - 计算实例操作 40
  - 网络 41
  - 对象存储 42
  - OpenStack 40
  - 编排 42
  - 故障排除 43
- Certified OpenStack Administrator
  - 认证 34, 35, 37, 38
  - 考试目标 39
  - 参考资料 35
  - 先决条件 URL 78
- 弹性云的特征
  - 强调 API 13
  - 云开发 14
  - 水平扩展 13
  - 开源 14
  - 标准硬件 13
- 企业虚拟化的特征
  - 昂贵硬件 11
  - 强调 GUI 11
  - 专有 12
  - 传统软件开发 12
  - 垂直扩展 11
- Cinder，考试目标
  - 创建备份 249
  - 管理备份 247, 249
  - CLI 235, 237
  - horizon 仪表板 233, 238, 242, 244
  - 创建实例 242
  - 创建快照 245
  - 管理快照 243
  - 管理卷 232
- Cinder
  - 关于 25, 226, 228
  - 架构 228
  - 后端软件 231
  - 备份 231
  - 考试目标 232
  - 硬件驱动 231
  - iSCSI（Internet Small Computer Systems Interface）229
  - LVM（Logical Volume Manager）229
  - 快照 231
  - 技术 229
  - 卷 231
- Cirros 105
- 无类域间路由 (CIDR) 171
- CLI 帮助 302
- 云镜像
  - 关于 101
  - 构建 102
  - glance 架构 104
  - Glance 镜像属性，与 Glance 元数据定义对比 106, 109
  - glance，在 Horizon 仪表板上 105
  - hashicorp packer 102
  - 镜像文件格式 103
  - OpenStack 磁盘镜像构建器项目 102
  - Oracle VirtualBox 102
- 云
  - Amazon 12
  - Amazon，缺点 14
  - 企业虚拟化，起源 10
  - 历史 9
  - NASA 15
  - 软件开发人员的困境 9
  - Rackspace 15
- 命令行界面 (CLI)
  - 关于 62
  - 调试 289
  - 项目范围 76
  - 基于服务的客户端，与 OpenStack 客户端对比 77




## D
- 数据存储 102
- Amazon 缺点
  - 成本 14
  - 安全 14
  - 供应商锁定 14

## E
- 弹性云
  - 关于 13
  - 特征 13
- 弹性计算云 (EC2) 13
- 企业虚拟化
  - 关于 11
  - 特征 11
  - 起源 10
- 临时存储 226
- 考试控制台
  - URL 38
- 考试环境 38
- 外部监视器 304
- 外部资源 198

## F
- 固定 IP 160
- 浮动 IP 203

## G
- glance 镜像服务，考试目标
  - 关于 109
- 创建 glance 镜像 110
- horizon 仪表板 111
- Horizon 仪表板 120
- 设置镜像属性 119
- 下载镜像 118
- 与项目共享镜像 118
- 设置元数据定义 119
- python-openstackclient 114, 117, 118, 124
- glance
  - 关于 25
  - 架构 104
  - 镜像属性与元数据定义对比 106
- 图形用户界面 (GUI) 11

## H
- Heat 资源
  - 关于 276
  - 与 AWS 的向后兼容性 277
- Heat 模板
  - 关于 273
  - 输出 275
  - 参数 275
  - 资源 275
  - 模板头 275
- Heat，考试目标
  - CLI 283, 287
  - horizon 仪表板 278, 283, 285
  - 管理堆栈 278
  - 更新堆栈 285
- Heat
  - 关于 271
  - 架构 272
  - 考试目标 277
  - horizon 仪表板 62
  - Nova 的超visor 类型
- QEMU (快速仿真器) 129
- QEMU-KVM (快速仿真器 - 基于内核的虚拟机) 130
- XEN 130

## I
- 镜像文件格式
- ISO (国际标准化组织，ISO9660) 103
- QCOW2 (QEMU 写时复制) 103
- RAW 103
- VDI (虚拟磁盘映像) 103
- VHD 和 VHDX (虚拟硬盘) 103
- VMDK (虚拟机磁盘) 103
- 基础设施即服务 (IaaS) 19
- 实例访问
  - 控制台 145
  - 浮动 IP 146
  - 网络命名空间 146
- 实例
  - 关于 21, 81
  - 启动 138
- 实习生 92
- iSCSI (Internet 小型计算机系统接口) 229

## K
- 密钥对
  - 私钥 132
  - 公钥 132
- Keystone，考试目标
  - 关于 77
  - 创建域 91
  - 创建组 91
  - horizon 仪表板 80, 87, 96
  - 创建项目 91
  - python-openstackclient 83, 86, 91, 97
  - 调整配额 79
  - 创建角色 91
  - 管理服务目录 93
  - 通过 CLI 添加用户 90
  - 管理用户 87
- Keystone
  - 关于 61
  - 与 OpenStack CLI 73, 75
  - 架构 62
  - 确定身份验证 62
  - 确定授权 62
  - 概念 63
  - 域 63
  - 端点 67
  - 组 66
  - 项目 64
  - 配额 67
  - 角色 65
  - 服务目录 67
  - 在虚拟设备中使用 68

## L
- Libvirt 130
- LinuxBridge 插件 162
- 日志文件
  - 分析 295
  - 问题 305
- 逻辑卷管理器 (LVM) 48, 229

## M
- MySQL
  - 管理 296

## N
- NASA 15
- Neutron，考试目标
  - 关于 164
  - CLI 213
  - 管理东西向路由器流量 183
  - 管理浮动 IP 214
  - horizon 仪表板 167, 170, 172, 173, 175, 176, 179, 181, 184, 198, 202, 205, 209, 212, 214
  - 管理提供商网络 198
  - 管理提供商子网 203
  - python-openstackclient 167, 169, 175, 182, 183, 196, 198, 202, 207, 221
  - 管理安全组和规则 175, 176
  - 管理租户网络 164
  - 管理租户子网 169
- Neutron
  - 关于 160
  - 架构 160
  - LinuxBridge 插件 162
  - 网络 162
  - 端口 163
  - 路由器 163
  - 安全组和规则 163
  - 子网 162
- Nova API 文档
  - URL 94
- Nova，考试目标
  - 关于 131
  - 管理规格 135
  - horizon 仪表板 132, 136, 139, 146, 151, 155
  - 管理实例操作 154
  - 创建实例快照 150
  - 访问实例 145, 149
  - 启动实例 138
  - 管理密钥对 132
  - openrc-human-resources 144
  - python-openstackclient 133, 138, 149, 150
- Nova
  - 关于 24, 128
  - 架构 128
  - 超visor 类型 129
  - nova-api 128

## O
- 对象 255
- 开放虚拟设备 (OVA)
  - 关于 46
  - 清单 (MF) 46
  - 开放虚拟化格式 (OVF) 46
  - 虚拟机磁盘文件 (VMDK) 46
- OpenStack CLI
  - 使用 73, 75
- OpenStack 云
  - 关于 47
  - 块存储节点 48
  - 计算节点 48
  - 控制器 47
  - 网络节点 47
  - 对象存储节点 48
- OpenStack 守护进程
  - 管理 292
- OpenStack 磁盘映像构建项目
  - 关于 102
  - URL 102
- OpenStack 基金会 16
- OpenStack 服务连接性
  - 使用 CLI 验证 57
- OpenStack 服务
  - Cinder 22
  - 目录 294
  - glance 21
  - heat 23
  - keystone 21
  - neutron 22
  - Nova 21
  - 概述 20
  - Swift 22
- OpenStack
  - 关于 15
  - 管理员 19
  - 应用开发者 19
  - 客户端 27
  - 命令行界面 (CLI) 26
  - 守护进程通信 29
  - 探索 305
  - Heat 编排模板 (HOT) 27
  - Horizon 仪表板 (GUI) 26
  - 基础设施工程师 19
  - 交互 26
  - 工作 18
  - 开放社区 16
  - 开放设计 16
  - 开放开发 16
  - 开源 16
  - OpenStack API 通信 31
  - OpenStack 客户端 27
  - OpenStack 基金会 16
  - OpenStack 服务，操作 24
  - OpenStack 服务，概述 20
  - 操作员 19
  - 产品开发者 20
  - 软件开发工具包 (SDK) 26
  - 类型 17
  - 上游 OpenStack 开发者 20
  - URL 16
- OSC (OpenStack 客户端)
  - 关于 28
  - 基于服务的客户端 28
  - 统一客户端 28
  - URL 28
- 重叠 IP 170

## P
- 持久 225
- 私钥 132, 221
- Project Panel Group 72
- 项目范围
  - 关于 300
  - 通过 CLI 76
- 项目 64
- 提供商网络 162
- 公钥 132
- Putty
  - URL 56

## Q
- 质量控制 233

## R
- Rackspace 15
- 读取 ACL 262
- 只读 66
- 表征状态转移 (REST) 13

## S
- 范围 64
- SDN (软件定义网络) 159
- 服务 65
- 服务目录
  - 管理员 95
  - 内部 95
  - 管理 93
  - 公共 95
- 服务到服务通信 31
- 简单对象访问协议 (SOAP) 13
- 简单存储服务 (S3) 12
- Swift，考试目标
  - CLI 260, 262, 267
  - 管理容器 ACL 262
  - 管理容器 256
  - horizon 仪表板 257
  - 管理对象 256
  - 设置对象过期 268, 269
  - 参考链接 260
- Swift
  - 关于 251
  - 账户 255
  - ACL (访问控制列表) 255
  - 架构 253
  - 容器 255
  - 考试目标 256
  - 对象过期 255
  - 对象版本控制 255
  - 对象 255
  - 静态网站托管 255
  - swiftoperator 角色 262

## T
- 租户 64
- 租户网络 162
- 时间管理 301
- OpenStack 类型
  - 设备 17
  - 发行版 17
  - 托管私有云 17
  - 项目 17
  - 公有云 17

## U
- 通用唯一标识符 (UUID) 47, 64

## V
- 虚拟专用服务器 (VPS) 12
- VirtualBox 网络
  - 配置 51
  - 确认 Host-Only 网络 IP 地址 54
  - Linux 52
  - mac 52
  - Windows 53
- VirtualBox
  - 关于 46
  - 下载 51
  - 安装 51
  - URL 51
- 可见性 105

## W
- 写入 ACL 262

## 为 Certified OpenStack Administrator 考试做准备
本书为您提供了一种具体的策略，以通过 OpenStack 基金会的第一项专业认证：Certified OpenStack Administrator。最近的一项调查显示，78% 的受访者表示 OpenStack 技能短缺阻碍了他们采用 OpenStack。请将此视为一个机会，通过证明您拥有管理现实世界 OpenStack 云所需技能来提高雇主和客户的信心。

您将从熟悉 OpenStack 环境开始，了解参加考试的好处，并安装包含的 OpenStack 一体化虚拟设备，以完成书中涵盖的目标。在探索各个服务的基础知识后，您将学习实现与 Keystone、Glance、Nova、Neutron、Cinder、Swift、Heat 和故障排除相关的考试目标的策略。

最后，您将受益于特别提示部分和一次练习考试，以检验您的知识。到旅程结束时，您将准备好成为一名 Certified OpenStack Administrator！

## 您将学到什么：
- 通过创建和修改域、组、项目、用户、角色、服务、端点和配额来管理 Keystone 身份服务
- 上传 Glance 镜像、启动新的 Nova 实例，并创建规格、密钥对和快照
- 发现 Neutron 租户和提供商网络、安全组、路由器和浮动 IP
- 通过创建卷并将其附加到实例来管理 Cinder 块存储服务
- 创建 Swift 容器并设置访问控制列表以允许对对象进行读/写访问
- 探索 Heat 编排模板并创建、列出和更新堆栈
