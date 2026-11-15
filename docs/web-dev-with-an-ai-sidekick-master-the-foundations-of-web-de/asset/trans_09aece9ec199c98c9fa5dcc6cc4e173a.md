
图 1.2：Web 开发中语言、框架和工具的角色

## 如何使用本书

技术书籍并非旨在以严格线性的方式阅读。与小说不同，您可以根据自己的兴趣、目标和现有经验自由跳转阅读。一些读者会立刻产生构建完整项目的冲动，可能会想直接跳到后面的章节，那里会将这些工作整合在一起。

这种直觉是可以理解的，我鼓励您去探索。然而，如果您这样做，会很快遇到一个现实：Web 开发依赖于多层知识。在前几章中可能显得缓慢或孤立的概念，在真实应用中结合使用时变得至关重要。本书的结构反映了这一进阶过程。

正如您在章节细分中所见，本书的前两部分专注于逐个学习各项技术。这是刻意为之。每种语言、框架和工具都单独介绍，以便您在没有不必要的复杂性干扰下理解其角色、优势及行为方式。试图一次性学完所有内容往往会导致困惑和浅层理解。逐个学习工具能让后续的项目实践少一些困惑。

本书的最后一部分将这些要素汇聚成一个完整的项目。这是前期投入收获回报的时刻。您不再是毫无上下文地复制粘贴代码，而更有可能理解自己在构建什么，以及为什么它会以那种方式工作。

同样重要的是要认识到，书籍只是学习过程的一部分。真正的进步源于阅读、动手实践、实验以及接触不同观点的结合。不可能写出一本涵盖 Web 开发所有知识的书。而且大多数读者通过多种教学技巧能学得更透彻。视频、文档和社区讨论都发挥着作用。最重要的是，您自己的坚持和好奇心将决定您能走多远。

学习构建软件是一项长期的工程。它需要时间、重复和耐心。本书旨在为您提供结构，以便您的时间花得值得。您不再需要拼凑几十个免费但零散的教程和文章，而是遵循一条连贯的路径，每一章都建立在上一章的基础之上。

以适合您的方式学习材料，但要理解本书为何这样组织。如果您花时间打下坚实的基础，后续阶段会让您感到更有成就感，也少很多挫败感。

本书代码文件可在本书的 GitHub 仓库中获取：[https://github.com/markjprice/web-dev-ai/tree/main/Code](https://links.uk.defend.egress.com/Warning?crId=6a06d4338e5d5f5540437bbb&Domain=packt.com&Threat=eNpzrShJLcpLzAEADmkDRA%3D%3D&Lang=en&Base64Url=eNoNyckRgCAQBMCIcP5-jYRjChYKodYF09f-djGbzwlksbLCEUdH99rqVInEy-ASt_MCU_IvuXGNxA89oBRA&@OriginalLink=github.com)。如果您对本书有意见，请在亚马逊上写差评前先联系我。作者无法回复亚马逊评论，因此我无法直接联系您以解决问题并提供帮助，也无法倾听您的反馈并在下一版中改进。请在本书的 Discord 频道提问：https://discord.gg/rdEsrVbbC，或在本书的 GitHub 仓库提出问题：[https://github.com/markjprice/web-dev-ai/issues](https://links.uk.defend.egress.com/Warning?crId=6a06d4338e5d5f5540437bbb&Domain=packt.com&Threat=eNpzrShJLcpLzAEADmkDRA%3D%3D&Lang=en&Base64Url=eNoFwksOABAMBcAT8fZu49NQ0hAtri8zzWxpACpbO8nnKZC4R1-bM-FRcoWuiwxWPaQfplIRrg%3D%3D&@OriginalLink=github.com)。

### 善用本书的 GitHub 仓库

我为本书创建了一个 GitHub 仓库，用于以下用途：

* 存放本书的解决方案代码，以便在印刷出版后继续维护
* 提供扩展书籍内容的额外材料，如勘误修正、小幅改进、有用链接列表，以及因篇幅限制无法收录印刷书中的可选章节
* 为读者提供一个就书籍相关问题联系我的场所
* 为了方便在命令提示符或终端输入命令，我列出了所有可轻松复制粘贴的命令：https://github.com/markjprice/web-dev-ai/blob/main/docs/commandlines.md

在尝试任何编码任务之前，我强烈建议您查看勘误、改进、出版后变更和常见错误页面。您可以在以下链接找到它们：[https://github.com/markjprice/web-dev-ai/blob/main/docs/errata/README.md](https://github.com/markjprice/web-dev-ai/blob/main/docs/errata/README.md)。

### 反馈书籍问题

如果您在遵循本书说明时遇到困难，或发现文本或解决方案代码中有错误，请在 GitHub 仓库中提出 Issue：

1. 使用您常用的浏览器访问以下链接：[https://github.com/markjprice/web-dev-ai/issues](https://links.uk.defend.egress.com/Warning?crId=6a06d4338e5d5f5540437bbb&Domain=packt.com&Threat=eNpzrShJLcpLzAEADmkDRA%3D%3D&Lang=en&Base64Url=eNrLKCkpKLbS10_PLMkoTdJLzs_Vz00sys4qKMpMTtUHAK67C0I%3D&@OriginalLink=github.com)。

2. 点击 `New Issue`。

3. 输入尽可能多的细节以帮助我诊断问题。例如：

* 具体的章节标题、页码和步骤编号
* 您认为相关且必要的代码和配置
* 预期行为和实际行为的描述
* 截图（您可以将图片文件拖放到 Issue 框中）

我无法总是立即回复 Issue。但我希望所有读者都能通过我的书获得成功，所以如果我能在不太麻烦的情况下帮助您（和其他人），我会乐意提供帮助。

### 给我反馈

如果您想给我关于本书的更多一般性反馈，请发邮件至 markjprice@gmail.com 或在 Discord 的书籍频道提问。您可以匿名提供反馈，如果您希望得到我的回复，可以提供电子邮件地址。我仅会使用该邮箱地址回复您的反馈。

请使用此邀请加入我和其他读者的 Discord：https://discord.gg/rdEsrVbbC。我喜欢听取读者对本书的喜爱之处、改进建议，以及他们如何与 AI 助手协作，所以请别害羞。请与我联系！

提前感谢您深思熟虑且具有建设性的反馈。了解如何写好问题或提出 Issue：[https://github.com/markjprice/markjprice/blob/main/articles/issues.md](https://links.uk.defend.egress.com/Warning?crId=6a06d4338e5d5f5540437bbb&Domain=packt.com&Threat=eNpzrShJLcpLzAEADmkDRA%3D%3D&Lang=en&Base64Url=eNpNx0ESgCAIBdAbyb7bCDFJSjp8vH8t273XMhcOosuybS4ynbxGv1eY6J88Jn-3h2qkyVCQAVtR_HwBle8aXA%3D%3D&@OriginalLink=github.com)。

### 我所有书籍共享的内容

我使用个人 GitHub 仓库存储与我所有书籍相关的共享内容。有用的章节包括以下内容：

* 常见问题解答：[https://github.com/markjprice/markjprice/blob/main/FAQs.md](https://links.uk.defend.egress.com/Warning?crId=6a06d4338e5d5f5540437bbb&Domain=packt.com&Threat=eNpzrShJLcpLzAEADmkDRA%3D%3D&Lang=en&Base64Url=eNrLKCkpKLbS10_PLMkoTdJLzs_Vz00sys4qKMpMTkVmJuXkJwH5mXn6bo6BxXq5KQCGAxWF&@OriginalLink=github.com)
