

社区经验精粹

## TypeScript 精要

使用 TypeScript 开发大规模响应式 Web 应用程序

Christopher Nance

[PACKT] 开源*

![](img/83a7f7e1723158e56008cf70304a8da4_0_0.png)

## 目录

- TypeScript 精要
  - 制作人员
  - 作者简介
  - 审稿人简介
  - www.PacktPub.com
    - 支持文件、电子书、折扣优惠等
      - 为什么订阅？
      - Packt 账户持有人免费访问
  - 前言
    - 本书涵盖的内容
    - 本书所需准备
    - 本书适合谁
    - 约定
    - 读者反馈
    - 客户支持
      - 下载示例代码
      - 勘误
      - 盗版
      - 问题
  - 1. TypeScript 入门
    - TypeScript 的优势
    - 搭建 IDE
    - Hello World
    - 命令行编译
    - 集成 Visual Studio
    - 创建新项目
    - 构建选项
    - 小结
  - 2. TypeScript 基础
    - 类型
    - 函数
    - 接口
    - 类
      - 定义
      - 接口
      - 静态和实例成员
      - 属性
    - 枚举
    - 模块
      - 内部模块
      - 通用对象
    - 小结
  - 3. TypeScript 编译器
    - ECMAScript 生成
    - ECMAScript 版本
    - 代码操作
    - 控制编译器输出
    - JavaScript 输出
    - 源映射
    - 高级选项
    - 小结
  - 4. 使用 TypeScript 进行面向对象编程
    - 基础
    - SOLID——面向对象设计
    - 理解继承
    - 封装
    - 抽象
    - 多态
    - 方法重载
    - 运算符重载
    - 方法重写
    - 参考
    - 小结
  - 5. 创建简单的绘图应用程序
    - 设置项目
    - 形状
    - 基本形状
    - 绘制形状
    - 让应用程序可交互
    - 引擎
    - 鼠标事件
    - 用户选项
    - 小结
  - 6. 声明文件与库集成
    - 声明文件
    - 第三方库集成
    - 安装 NuGet 包
    - 与 jQuery 集成
    - 与 Knockout 集成
    - 使用 RequireJS
    - 小结
  - 7. 增强绘图应用程序
    - 转换为 AMD 模块
    - 绑定用户控件
    - 可复用控件
    - 创建 ViewModel
    - 生成单一输出文件
    - 应用程序样式
    - 小结
  - 8. 调试 TypeScript
    - 调试
    - 源映射
    - 调试器
    - 测试驱动开发
    - 单元测试
    - 添加测试
    - 小结
  - 索引

## TypeScript 精要

版权所有 © 2014 Packt Publishing

保留所有权利。未经出版商事先书面许可，不得以任何形式或任何方式复制、存储于检索系统或传播本书的任何部分，除非在评论文章或评论中引用简短片段。

在编写本书时已尽一切努力确保所提供信息的准确性。然而，本书所含信息按“原样”销售，不提供任何明示或暗示的担保。作者、Packt Publishing 及其经销商和分销商均不对因本书直接或间接造成或声称造成的任何损害承担责任。

Packt Publishing 已尽力通过适当使用大写字母提供本书中提到的所有公司和产品的商标信息。然而，Packt Publishing 无法保证此信息的准确性。

首次出版：2014 年 10 月  
生产参考编号：1161014  
由 Packt Publishing Ltd. 出版  
Livery Place  
35 Livery Street  
Birmingham B3 2PB, UK.  
ISBN 978-1-78398-576-0  
[www.packtpub.com](http://www.packtpub.com)

### 制作人员

Adonia Jones

Komal Ramchandani

Nitesh Thakur

#### 封面设计

Komal Ramchandani

## 作者简介

**Christopher Nance** 是一位经验丰富的软件工程师，从事富 Web 应用程序开发已超过 4 年。在 KnowledgeLake，他采用 TypeScript 开发了一系列可重用组件，以扩展到多个产品和平台。

我想感谢我的父母一直以来的支持和鼓励。我还要感谢 KnowledgeLake 的团队，他们推动我不断提高作为一名工程师的技能。

## 审稿人简介

**Andrea Martinelli** 是一位充满热情的软件开发人员，目前正在开发 Shaman.IO——一款自动检测并从 Web 提取结构化数据的工具。

过去，他曾开发过 Songr，一款音乐播放器和聚合器。他的兴趣涵盖 Web 数据提取、代码性能以及静态类型语言。他是一名熟练的 C# 开发人员，自 TypeScript 首次发布以来就对其产生了兴趣。他毕业于特伦托大学计算机科学专业，之后在丹麦技术大学学习，尽管他现在在欧洲各地奔波的同时，将更多时间投入到了 Shaman.IO 项目上。

我想感谢我的朋友们，尤其是 Gianluca 和 Stefano，他们一直非常支持我。我还要感谢 Filz 教授，他是我高中时最有影响力的老师之一。也要感谢 Mads，一个我永远不会忘记的特别的人。以及我的家人，我已经很久没有见到他们了。

**Nathan Rozentals** 撰写商业软件已超过 23 年，从大型机上的 COBOL 起步，历经 C、C++ 和 Java，最终专注于 C# 和 ASP.NET。

他在 2012 年 10 月——即 0.8.0 版本发布后的第二天——接触到了 TypeScript，并从此爱不释手。在 TypeScript 中，他找到了一种语言，可以把他多年来在各种语言中学到的所有设计模式和实践带到 JavaScript 中。

在 0.8.0 版本发布大约 6 天后，他开始撰写关于 TypeScript 的博客；内容涵盖多种主题，包括单元测试、实现 IoC 容器，以及使用 AMD 模块组织代码。当微软自己开始在 CodePlex 讨论论坛中引用他的博客时，他知道自己成功了。

你可以在 http://blorkfish.wordpress.com 找到他的博客。

他目前从事医疗行业，为医疗系统带来触摸屏界面，从而让临床医生和医院工作人员能够自带设备（BYOD）。

他对代码质量、单元测试和持续集成充满热情，并帮助许多大型团队在多种不同语言的不同软件项目中实施 CI。

他不写代码时，喜欢风帆冲浪和踢足球。他也是利物浦足球俱乐部的忠实支持者。

我想感谢我的伴侣 Kathy，感谢她无尽的爱和支持，并给了我自由，让我可以长时间投入到自己热爱的事情中。你是最好的。

致 Ayron 和 Dayna——你们永远在我心中——你们对生活的热情和好奇心给了我如此多的灵感。我会永远在你们身边。

**Basarat Ali Syed**（BAS）是澳大利亚墨尔本 Picnic Software（http://picnicsoftware.com/）的高级开发人员，是前端问题的资深专家。他拥有澳大利亚国立大学的计算学硕士学位，并以高成绩毕业于所有课程。他是澳大利亚开发者聚会和会议上的常客，并曾在 ALT.NET、DDD Melbourne、MelbJS 和 Node.js meetup 等活动中发表演讲。他对 Web 技术充满热情。他是 TypeScript 社区的知名成员，并在 DefinitelyTyped 团队工作（https://github.com/DefinitelyTyped）。业余时间，他喜欢健美、骑自行车，并维护着一个帮助同行开发者的 YouTube 频道（http://youtube.com/basaratali）。你可以轻松地在 Twitter @basarat、www.github.com/basarat 和 www.basarat.com 上找到他。

### 为什么订阅？

- 可对 Packt 出版的每一本书进行全文搜索
- 可复制、粘贴、打印和收藏内容
- 按需提供，可通过 Web 浏览器访问

