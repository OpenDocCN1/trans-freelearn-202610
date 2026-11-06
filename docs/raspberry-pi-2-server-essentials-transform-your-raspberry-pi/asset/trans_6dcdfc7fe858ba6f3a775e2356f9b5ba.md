
![](img/a8753cacc9d454dfc09eda1ed5012bd3_0_0.png)

社区经验精粹

## Raspberry Pi 2 服务器精要

使用本实用且易懂的教程，将你的 Raspberry Pi 转变为支持整个多媒体世界的通用 Web 服务器！

Piotr J Kula

[PACKT] 出版

## Raspberry Pi 2 服务器精要

使用本实用且易懂的教程，将你的 Raspberry Pi 转变为支持整个多媒体世界的通用 Web 服务器！

Piotr J Kula

[PACKT]
出版
BIRMINGHAM - MUMBAI

## Raspberry Pi 2 服务器精要

版权所有 © 2016 Packt Publishing

保留所有权利。本书的任何部分，未经出版者事先书面许可，不得以任何形式或任何手段复制、存储于检索系统或传播，但嵌入关键文章或评论中的简短引文除外。

本书的准备过程中已尽最大努力确保所提供信息的准确性。然而，本书所包含的信息按“原样”出售，不提供任何明示或暗示的保证。作者、Packt Publishing 及其经销商和分销商不对因本书直接或间接造成或声称造成的任何损害承担责任。

Packt Publishing 已努力通过适当使用大写字母为本书中提及的所有公司和产品提供商标信息。然而，Packt Publishing 不能保证此信息的准确性。

首次出版：2016 年 4 月

生产参考：1220416

由 Packt Publishing Ltd. 出版。
Livery Place
35 Livery Street
Birmingham B3 2PB, UK.

ISBN 978-1-78398-569-2

www.packtpub.com

## Credits

**作者**
Piotr J Kula

**审阅者**
Ian McAlpine
Cédric Verstraeten

**策划编辑**
Priya Singh

**收购编辑**
Usha Iyer
Reshma Raman

**内容开发编辑**
Sanjeet Rao

**技术编辑**
Jayesh Sonawane

**校对编辑**
Yesha Gangani

**项目协调员**
Judie Jose

**校对员**
Safis Editing

**索引员**
Monica Ajmera Mehta

**图形**
Disha Haria

**制作协调员**
Nilesh Mohite

**封面设计**
Nilesh Mohite

## About the Author

**Piotr J Kula** 从 6 岁时拆开父亲的 IBM PC/XT 机器开始，对计算机技术的热情就被点燃了。机器重建后成功启动，证明了 Piotr 在技术方面与生俱来的能力。

25 年后，Piotr 在不同国家的各种公司工作。他一直解决复杂问题或从事新体验。只要有某种处理器，Piotr 总是第一个尝试、修复或配置它。

今天，Piotr 是其在英国的公司 Kula Solution Ltd. 的首席执行官。Piotr 在多项 Microsoft 技术方面获得认证，并专门从事 Microsoft 和 Linux 技术。

Piotr 的梦想是 Linux 和 Microsoft 之间实现统一体验。他从未因任何一方的负面情绪而气馁，反而努力在这些遥远的巨头之间架起桥梁。在发现 Raspberry Pi 后，他完全用它取代了拥有的所有其他嵌入式设备，并成功发布了 *Raspberry Pi Server Essentials*，由 *Packt Publishing* 出版。

今天，随着 *Raspberry Pi 2 Server Essentials* 的发布，Piotr 距离实现梦想又近了一步。本书主要关于 Linux，但也谈到了在你心爱的 Raspberry Pi 上集成和运行 Windows。

我谨向我的了不起的妻子 Katarzyna Kula 表示感谢，她帮助我决定何时工作已足够！感谢她在我所有项目和无数在电脑前度过的时光中一直支持我。我还要感谢整个 Raspberry Pi 社区，特别是 Stack Exchange 上的成员，*Packt Publishing* 的每一个人，以及以各种方式帮助我完成本书的人。

## About the Reviewers

**Ian McAlpine** 第一次接触计算机是在他学校的 Research Machines RML-380Z，以及他的物理老师的 Compukit UK101。随后是 Sinclair ZX81，然后是他至今仍拥有的 BBC Micro Model A。这份兴趣促使他获得了 Aston University 的电子系统工程 MEng 学位和利物浦大学的信息技术 MSc 学位。Ian 目前在加拿大温哥华 SAP Labs 的 BI & Analytics 能力中心担任产品专家。

Raspberry Pi 的引入不仅重新点燃了他 *tinker* 的欲望，还提供了回馈社区的机会。因此，Ian 积极担任志愿者，参与 The MagPi 的工作，这是一本关于 Raspberry Pi 的月刊杂志，你可以在网上阅读或免费从 `https://www.raspberrypi.org/magpi` 下载。他还持有业余无线电执照（呼号 VE7FTO），并担任当地社区应急管理办公室的通讯志愿者。他曾担任 *Packt Publishing* 出版的 *Raspberry Pi Cookbook for Python Programmers* 和 *Raspberry Pi Projects for Kids* 书籍的技术审阅者。

我谨向我亲爱的妻子 Louise，以及我棒极了的孩子 Emily 和 Molly 表示感谢，感谢她们允许我消失在我的“办公室”里……并感谢她们训练我们的狗来把我找回来！

**Cédric Verstraeten** 拥有工程硕士学位，主要活跃于 C++ 社区。他担任软件工程师，是开源的狂热爱好者。他将大部分时间花在可以自动化和简化人们生活的副项目上。他是 Raspberry Pi Belgium 聚会的组织者，也是开源视频监控系统 Kerberos.io 的创始人。

> 我要感谢 Packt Publishing 允许我作为审阅者参与其中。我真的认为他们的书籍可以为人们提供对特定主题的深入概述。

www.PacktPub.com

## eBooks, discount offers, and more

你知道 Packt 提供每本书的电子书版本吗？文件格式包括 PDF 和 ePub。你可以在 www.PacktPub.com 升级到电子书版本，作为印刷书籍的客户，你有权享受电子书的折扣。请通过 customercare@packtpub.com 与我们联系了解更多详情。

在 www.PacktPub.com，你还可以阅读大量免费技术文章，订阅各种免费通讯，并获得 Packt 书籍和电子书的独家折扣和优惠。

![](img/a8753cacc9d454dfc09eda1ed5012bd3_7_0.png)

https://www2.packtpub.com/books/subscription/packtlib

你需要即时解决 IT 问题吗？PacktLib 是 Packt 的在线数字图书库。在这里，你可以搜索、访问和阅读 Packt 的整个图书库。

## Why subscribe?

- 在 Packt 出版的每本书中完全可搜索
- 复制、粘贴、打印和书签内容
- 按需访问，并通过网络浏览器访问

## Table of Contents

- 前言 ix
- 第 1 章：开始使用 Raspberry Pi 1
  - 硬件要求 1
    - 电源 2
    - USB 集线器 3
    - SD 卡 4
  - 额外外设 4
    - 必备外设 5
    - 有用外设 5
      - 互联网 3G 适配器 5
      - 音频录制 6
      - 其他媒体附加组件 6
    - 娱乐外设 7
  - 在 Raspberry Pi 上安装 Raspbian 8
  - 了解 Raspberry Pi 的设计 9
    - 启动过程 10
    - 其他功能 10
    - 硬件限制 11
      - 网络速度 11
      - USB 瓶颈 11
      - 时间 12
  - 参考文献 12
  - 摘要 13
- 第 2 章：准备网络 15
  - 局域网 (LAN) 15
    - eth0 端口 16
    - wlan0 接口 17
    - 回环 (LO) 接口 17
- 无线配置 (Wi-Fi) 17
  - 推荐的无线适配器 17
  - 从桌面设置 18
  - 从控制台设置 18
  - 使用 wicd-curses 21
- 静态网络地址 22
- 测试和基准测试你的网络 23
  - 基础测试 23
  - 高级基准测试工具 24
    - 速度测试应用 24
    - iPerf 25
  - 推荐带宽 25
- 互联网配置 26
  - 家用套餐 27
  - 企业套餐 27
- 动态 DNS 28
  - 安装客户端 28
  - 免费动态 DNS 域名变通方案 29
- 摘要 30
- 第 3 章：配置额外功能 31
  - 更新 Raspberry Pi 31
    - 更新固件 32
    - 更新软件包 32
    - 更新发行版 32
    - 结果 33
- 硬件看门狗 33
  - 启用看门狗和守护进程 34
  - 测试看门狗 35
- 启用额外解码器 35
  - 购买许可证 35
  - MPEG-2 36
  - VC-1 36
- 硬件监控 36
- 摘要 37
- 第 4 章：使用快速 Web 服务器和数据库 39
- Apache 与 nginx 与 Kestrel 39
  - 安装 nginx 40
  - 配置 nginx 虚拟主机 41
- nginx 的额外框架 43
  - Python API 43
    - 执行 Python 43
