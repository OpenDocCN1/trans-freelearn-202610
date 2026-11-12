

![](img/76681dcfb8cd2bd107d0c06ab088f916_0_0.png)

社区经验提炼

## 树莓派的 Yocto

利用 Yocto 与树莓派的强大组合，创建独特而惊人的项目

Pierre-Jean Texier
Petter Mabacker

[PACKT] 开源

## 目录

- 树莓派的 Yocto
- 致谢
- 关于作者
- 关于评审人
- www.PacktPub.com
- 电子书、折扣优惠及更多
- 为何订阅？

## 前言

- 本书内容
- 本书所需
- 适读人群
- 约定
- 读者反馈
- 客户支持
- 下载示例代码
- 下载本书的彩色图片
- 勘误表
- 反盗版
- 问题

## 1. 初识 Yocto 项目

- Yocto 项目
- 了解构建系统
- 核心组件
- 什么是 Poky？
- 主管 - BitBake
- OpenEmbedded-Core
- 探索元数据
- Yocto 项目 - 工作流
- 小结

## 2. 为树莓派构建我们的第一个 Poky 镜像

- 为主机系统安装所需软件包
- Ubuntu 上的 Poky
- Fedora 上的 Poky
- 下载 Poky 元数据
- 下载树莓派 BSP 元数据
- oe-init-build-env 脚本
- 编辑 local.conf 文件
- 编辑 bblayers.conf 文件
- 构建 Poky 镜像
- 镜像选择
- 运行 BitBake

### 创建 SD 卡
### 在树莓派上启动镜像
### 小结

## 3. 使用 Hob 和 Toaster 精通构建

### Hob

- 为 Hob 准备环境
- 运行 Hob
- 配置配方和软件包
- 构建镜像

### 探索 Toaster

- 为主机系统安装所需软件包
- 运行 Toaster
- 运行 BitBake
- 运行 Web 界面
- 小结

## 4. 理解 BitBake

### BitBake
### 元数据

- 配置
- 类
- 配方
- 解析元数据
- 偏好和提供者
- 依赖项

### 获取

- 本地文件获取器
- HTTP 获取器
- Git 获取器

### 理解 BitBake 的任务

- 小结

## 5. 在树莓派上创建、开发和部署

### 软件开发工具包 (SDK)

- 通用 SDK - meta-toolchain
- image.bb -c populate_sdk
- Qt SDK - meta-toolchain-qt
- Qt5 SDK - meta-toolchain-qt5
- 交叉编译 - 示例
- SDK 环境的配置
- 工具列表
- 编译

### 树莓派与包管理器

- 包格式可用性
- 选择包格式
- 在目标设备上安装和更新软件包
- RPM 包

- 手动安装
- 自动安装

### IPK 包

- 手动安装
- 自动安装

### 我们的应用 - 简介
### 我们的应用 - 创建配方

- 配方解释

### 小结

## 6. 使用外部层

### 层简介
### 层的基本概念

- 理论
- 软件层

  - README 和 COPYING
  - classes 文件夹
  - conf 文件夹
  - recipes-* 目录

- 机器 (BSP) 层
- 向树莓派添加外部层
- 小结

## 7. 在树莓派上部署自定义层

- 使用 yocto-layer 脚本创建 meta-packt_rpi 层
- 将 gpio-packt 添加到 meta-packt_rpi
- 为 gpio-packt 打补丁

  - 生成补丁
  - 将补丁添加到配方文件

- 创建 raspberry-packt-image.bb 镜像

  - 创建环境
  - 修改配方文件

- 部署 raspberry-packt-image.bb 镜像
- 小结

## 8. 深入树莓派外设与 Yocto 配方

### SPI 总线

- spi-tools 项目

  - 包含在 meta-oe 层中
  - 构建 spi-tools

### 在树莓派上测试

- spi-config
- spi-pipe
- 结论

### I2C 总线
### Wii Nunchuck

- Nunchuck 连接器
### 树莓派连接

### Nunchuck 协议

- 加密
- 请求传感器数据
- 测试 I2C 连接
- 创建 Nunchuck 应用
- 与 meta-packt_rpi 集成
- 创建 Nunchuck 配方
- 测试 Nunchuck 应用
- V4L 介绍
- 视频支持
- v4l-utils 集成

### 小结

## 9. 在树莓派上制作媒体中心

- 项目描述：CPU 温度监控
- 概述
- 硬件/软件要求
- 创建主应用程序
- server.js
- index.html
- 创建 Yocto/OE 环境
- 修改镜像
- 创建配方文件
- 说明
- 自动启动 - init 文件
- 说明
- 自动启动 - 配方文件
- 说明
- 部署 raspberry-packt-image
- 测试应用程序
- 该项目的未来

### 小结

## 10. 与 LCD 触摸屏和 Linux 内核交互

- Linux 内核
- Linux 内核与树莓派 Linux 内核的对比
- Linux 内核入门
- 在 Yocto 中配置内核
- 配置带 LCD 支持的内核
- 树莓派设备树
- 从内核角度配置触摸屏
- 使用 Yocto 项目为树莓派设置 LCD 显示屏
- 树莓派 7 英寸触摸屏
- PiTFT 2.8 电阻式触摸屏
- 在 LCD 显示屏上开发和使用应用程序
- 使用 Qt 开发自定义应用程序

### 小结

## 11. 为树莓派 BSP 层做贡献

- 开源
- 为开源项目做贡献
- 探索 Git
- 什么是 Git？
- 使用 Git
- 为 Yocto 项目做贡献
- 为 meta-raspberrypi 做贡献
- 设置你的 Git 仓库
- 创建你的提交
- 将更改发送到社区
- 跟进
- 实际示例 - 将自定义工具上游提交
- 小结

## 12. 家庭自动化项目 - 启动自定义镜像

- 使用树莓派进行家庭自动化
- 项目所需材料
- 搭建项目基础
- 创建新层
- 自定义镜像配方
- 构建和启动镜像
- 创建服务器端
- 为你的镜像创建软件包列表
- 设置自定义软件包列表
- 在 meta-packt-iot 中开始使用自定义软件包列表
- 整合所有内容
- 树莓派的串行和 SSH 连接
- 使用树莓派控制继电器
- 使用树莓派控制台灯
- 从智能手机开启/关闭台灯
- 额外 - 使用带 LCD 的树莓派作为客户端
- 小结

版权所有 © 2016 Packt 出版社

保留所有权利。未经出版商事先书面许可，本书任何部分不得以任何形式或之任何方式复制、存储在检索系统中或传播，除非是嵌入在评论文章或书评中的简短引用。

在编写本书时已尽力确保所呈现信息的准确性。然而，本书中的信息按原样出售，不提供任何明示或暗示的担保。作者、Packt 出版社及其经销商和分销商均不对由此书直接或间接造成或声称造成的任何损害承担责任。

Packt 出版社已努力通过适当使用大写字母提供本书中提及的所有公司和产品的商标信息。然而，Packt 出版社无法保证该信息的准确性。

初版：2016 年 6 月

出版参考：1170616

由 Packt Publishing Ltd. 出版

Livery Place

35 Livery Street

Birmingham

B3 2PB, UK.

ISBN 978-1-78528-195-2

www.packtpub.com

## 致谢

| 作者 | 文案编辑 |
| :--- | :--- |
| Pierre-Jean TEXIER<br>Petter Mabacker | Madhusudan Uchil |
| 评审人 | 项目协调员 |
| Burt Ganz<br>Dave (Jing) Tian<br>Helmi ROMDHANI<br>Pierre FICHEUX | Judie Jose |
| 委托编辑 | 校对员 |
| Amarabha Banerjee | Safis Editing |
| 获取编辑 | 索引编制人 |
| Meeta Rajani | Hemangini Bari |
| 内容开发编辑 | 制作协调员 |
| Rashmi Suvarna | Shantanu N. Zagade |
| 技术编辑 | 封面制作 |
| Mohit Hassija | Shantanu N. Zagade |

