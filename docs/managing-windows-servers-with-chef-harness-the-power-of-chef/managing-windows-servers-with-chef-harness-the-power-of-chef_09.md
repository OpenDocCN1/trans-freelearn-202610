### 索引

#### A

- Amazon EC2
  - custom user scripts, executing in 66
- Amazon EC2 instances
  - authentication, setting up 65
  - Chef node, removing 70
  - custom user script, providing 67, 68
  - knife plugin, installing 64
  - managing 64
  - provisioning 65, 66
  - terminating 69
  - user script, writing 66, 67
- attributes 12
- autorun script
  - about 27
  - example 28
- Azure virtual machine
  - creating 60, 61

#### B

- batch scripts
  - example 27
  - executing 25, 26
- bootstrap 12
- bootstrap script 12

#### C

- Chef 0.11.x resource. See registry_key resource
- Chef 10.x resource. See windows_registry resource
- Chef architecture
  - overview 13
- Chef client
  - installing, on Windows 14
  - installing, on Windows with MSI 17-20
- Chef client installation, Windows
  - hosts, bootstrapping 14-17
  - MSI used 17-19
- Chef components
  - attributes 12
  - bootstrap 12
  - cookbook 12
  - data bags 13
  - environments 13
  - node 11
  - provider 12
  - recipes 12
  - resources 12
  - role 12
  - run list 12
  - workstation 11
- Chef node
  - removing, from EC2 70
  - removing, from Rackspace 75
- ChefSpec
  - used, for testing 84-87
- cloud providers
  - scaling with 7
- code parameter 26
- command parameter 26
- cookbook 12
- creates parameter 26
- custom user scripts
  - executing, in EC2 66-69
- cwd parameter 26

#### D

data bags 13
decclarative language
about 77
benefits 77-79
Deployment Image Servicing and Management. See DISM
DISM 23
domain-specific language (DSL) 6

#### E

EC2 authentication
setting up 65
EC2 knife plugin
installing 64
end hosts
interacting with 6
environments 13

#### F

feature_name parameter 24
features, Windows
managing 22-24
flags parameter 26

#### G

GitHub
Umbraco cookbook, downloading from 50
group parameter 26

#### I

IIS pool
generating 54
IIS service
preparing 52
IIS site
generating 54

#### K

knife
configuring, for Azure 60

#### L

Linux-based systems
interacting with 8, 9

#### M

management certificate, Azure
downloading, URL 60
Microsoft Azure
knife, configuring for 60
management certificate, downloading 60
new virtual machine, creating 60, 61
node, bootstrapping 62
working with 59
Microsoft Azure node
bootstrapping 62
reusable image, creating 63, 64
Microsoft installer package. See MSI
MSI
about 17
used, for installing Chef client on Windows 17-19
multiple platform recipes
complexity, reducing 82, 83
multiple platforms
handling 79
recipes 80-82
support, declaring in cookbook 79

#### N

node
about 11
role, applying to 56, 57

#### P

PowerShell
scripting with 8
prerequisites, Umbraco recipe
installing 51, 52
printer ports
managing 31
managing, examples 32
printers
actions and parameters 33, 34
managing 32
managing, examples 34, 35
manipulating 30
provider 12

#### R

**Rackspace Cloud**
Chef node, removing 75
configuration, injecting into virtual machine 72-74
instance, provisioning 71, 72
interacting with 70
**Rackspace Cloud instance**
provisioning 71-74
terminating 74, 75
recipes
about 12
testing, ChefSpec used 84
testing, RSpec used 84
**recipe, Umbraco**
application, configuring 53
application, fetching 53
IIS pool, generating 54
IIS service, preparing 52
IIS site, generating 54
prerequisites, installing 51, 52
**registry_key resource** 38
**registry_key resource actions**
create 38
create_if_missing 38
delete 38
delete_key 38
**registry_key resource parameters**
architecture 38
key 38
provider 38
recursive 38
values 38
**registry keys**
managing, examples 37
**registry values**
managing, examples 39
**remove action** 24
**resources** 12

**role**
about 12
applying, to node 56, 57
creating 55, 56
**roles**
installing, different mechanisms used 24, 25
managing 22-24
**RSpec**
used, for testing 84, 85
**run action** 26
**run list** 12

#### S

**scripts**
running, at startup 27
**software packages**
installing 28, 29
installing, examples 30
**source control** 83
**system path**
managing 39
managing, examples 40

#### T

**tasks**
managing, example 42
scheduling 40, 41
**tests**
executing 87
expanding 89, 90
failure, understanding 88

#### U

**Umbraco CMS**
configuring 53
downloading 53
installing 54
role, applying, to node 56, 57
role, creating 55, 56
**Umbraco CMS installation**
host, bootstrapping 55




Umbraco cookbook
关于 47
下载，来自 GitHub 50
检查 48, 49
安装 49
配方，检查 51
URL 47
用户参数 26

V

版本控制
关于 83, 84
与源代码控制 83, 84
基础知识，测试 84, 85
配方，测试 84
测试扩展 89, 90
测试失败 88
测试执行 87
URL 83
虚拟机 (VM) 62

W

Windows
与主机交互 6
重启 44
重启，示例 45
Chef 支持的平台 10
使用 5, 6
windows_auto_run 资源操作
创建 27
移除 27
windows_auto_run 资源参数
args 27
name 27
program 27
windows_batch 资源参数
code 26
command 26
creates 26
cwd 26
flags 26
group 26
user 26
windows_feature 资源操作
安装 24
移除 24

windows_feature 资源参数
feature_name 24
Windows 主机
引导 7, 16, 17
与云提供商扩展 7
使用 PowerShell 脚本 8
Windows 主机，引导
启用基本身份验证 16
配置防火墙端口 16
启用 WinRM 15
windows_package 资源操作
安装 29
移除 29
windows_package 资源参数
checksum 29
installer_type 29
options 29
package_name 29
source 29
success_codes 29
timeout 29
version 29
windows_pagefile 资源操作
删除 42
设置 42
windows_pagefile 资源参数
automatic_managed 42
initial_size 42
maximum_size 42
name 42
system_managed 42
Windows 页面文件
与页面文件交互 42
管理，示例 43
windows_path 资源操作
添加 39
移除 39
windows_path 资源参数
path 39
windows_printer_port 资源
关于 31
操作 31
参数 31
windows_printer_port 资源操作
创建 31
删除 31
windows_printer_port 资源参数
ipv4_address 31
port_description 31
port_name 31
port_number 31
port_protocol 31
snmp_enabled 31
windows_printer 资源 30, 33
windows_printer 资源操作
创建 33
删除 33
windows_printer 资源参数
comment 33
default 33
device_id 33
driver_name 33
ipv4_address 33
location 33
shared 33
share_name 33
windows_reboot 资源操作
取消 45
请求 45
windows_reboot 资源参数
reason 45
timeout 45
Windows 注册表
Chef 0.11.x 资源 38, 39
Chef 10.x 资源 36, 37
与注册表交互 35
windows_registry 资源 36
windows_registry 资源操作
创建 36
force_modify 36
修改 36
移除 36
windows_registry 资源参数
key_name 36
type 36
values 36
Windows 远程管理。参见
WinRM
Windows Server 2012 主机
引导 55
Windows 特定资源
Chef 支持的平台 22
列表 21
使用 10, 21

windows_task 资源操作
更改 40
创建 40
删除 40
运行 40
windows_task 资源参数
command 41
cwd 41
frequency 41
frequency_modifier 41
name 41
password 41
run_level 41
start_day 41
start_time 41
user 41
windows_zipfile 资源操作
解压缩 43
压缩 43
windows_zipfile 资源参数
checksum 43
overwrite 43
path 43
source 43
WinRM 6
工作站 11
Z
ZIP 文件
管理 43
管理，示例 44

## 感谢购买
## 使用 Chef 管理 Windows 服务器

## 关于 Packt 出版社

Packt，发音为 'packed'，于 2004 年 4 月出版了第一本书《Mastering phpMyAdmin for Effective MySQL Management》，随后继续专注于出版针对特定技术和解决方案的高度聚焦图书。

我们的图书和出版物分享了您的 IT 专业同行在调整和定制当今系统、应用程序和框架方面的经验。我们的解决方案导向图书为您提供知识和能力，以定制您正在使用的软件和技术来完成工作。Packt 图书比您过去看到的 IT 图书更具体、更不笼统。我们独特的商业模式使我们能够为您提供更多聚焦的信息，让您了解更多需要知道的内容，而减少不需要知道的内容。

Packt 是一家现代且独特的出版公司，专注于为开发者、管理员和新手社区制作高质量、前沿的图书。
如需更多信息，请访问我们的网站：www.packtpub.com。

## 关于 Packt 开源

2010 年，Packt 推出了两个新品牌，Packt Open Source 和 Packt Enterprise，以继续其专业化的重点。本书是 Packt Open Source 品牌的一部分，该品牌专注于围绕开源许可构建的软件图书，并为从高级开发人员到初学者网页设计师的任何人提供信息。开源品牌还运行 Packt 的开源版税计划，通过该计划，Packt 为每个销售其软件图书的开源项目提供版税。

## 为 Packt 撰写

我们欢迎所有有兴趣撰写书籍的人士的咨询。书籍提案应发送至 author@packtpub.com。如果您的书籍创意仍处于早期阶段，并且希望在撰写正式书籍提案之前先讨论，请联系我们；我们的一位委托编辑将与您联系。

我们不仅寻找已出版的作者；如果您拥有强大的技术技能但没有写作经验，我们经验丰富的编辑可以帮助您发展写作生涯，或者仅仅为您的专业知识获得一些额外回报。

## Chef 基础架构自动化食谱

ISBN: 978-1-84951-922-9 平装：276 页

超过 80 个美味配方，使用 Chef 自动化您的云和服务器基础架构

+   1. 配置、部署和扩展您的应用程序。
+   2. 自动化容易出错且繁琐的手动任务。
+   3. 在本地或云中管理您的服务器。
+   4. 使用基于任务的配方解决现实世界的自动化挑战。

## Chef 入门指南

ISBN: 978-1-78216-346-6 平装：70 页

一本实用指南，帮助您开始使用 Chef，这是一个不可或缺的工具，用于配置和管理您系统的基础架构

+   1. 即学即用！一本简短、快速、聚焦的指南，立即见效。
+   2. 了解 Chef 的核心功能及其如何与您的基础架构集成。
+   3. 设置您自己的 Chef 服务器来管理您的基础架构。
+   4. 轻松配置新服务器并开发您自己的配方以供 Chef 使用。

请查看 www.PacktPub.com 以获取我们书籍的信息

## 学习 Windows Azure 移动服务用于 Windows 8 和 Windows Phone 8

ISBN: 978-1-78217-192-8        平装：124 页

一本简短、快速且聚焦的指南，通过利用 Windows Azure 移动服务的力量来增强您的 Windows 8 应用程序

+   1. 通过基于 XAML 的案例研究游戏深入了解 Azure 移动服务。
+   2. 使用推送通知和通知中心增强您的应用程序。
+   3. 按照逐步说明进行面向结果的示例。

## 使用 Vagrant 创建开发环境

ISBN: 978-1-84951-918-2        平装：118 页

使用 Vagrant 结合 Puppet、Chef 和 VirtualBox 创建和管理虚拟开发环境

+   1. 使用 Puppet 和 Chef 配置虚拟机。
+   2. 在本地复制多服务器环境。
+   3. 设置虚拟 LAMP 开发服务器。

请查看 www.PacktPub.com 以获取我们书籍的信息
