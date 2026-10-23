## Elastic 实践者向量搜索：用于搜索、可观测性和安全性的 NLP 解决方案构建工具包，向量搜索使用指南






To the curious minds who seek to learn and grow alongside us—this book is our shared journey.
Written not only to share but also to deepen our understanding, it’s a celebration of the power of exploration in the world of Elastic and engineering. We are grateful for the Elastic engineering team, whose remarkable efforts inspire us every step of the way.

– Bahaaldine Azarmi and Jeff Vestal

### Foreword

Technology should be about solving real problems, not just keeping up with the latest trends. That’s the essence of this book, which focuses on vector search with Elastic. It’s about real-world solutions and meaningful progress, aligning perfectly with our mission at Elastic – to create a platform that truly revolutionizes how we interact with data.

Apache Lucene, the heart of Elasticsearch, embodies this mission. It started as a straightforward search engine and has evolved into a crucial tool for addressing complex challenges. This evolution mirrors our commitment to practical, impactful solutions. With the integration of vector search, we’re not just adding a feature; we’re enhancing how people search and use data, making it more effective and meaningful.

Vector search is vital in delivering exceptional user experiences. It represents a deep understanding of user needs, offering insights that are not just relevant but genuinely useful. Incorporating vector search into Elastic is more than a technical milestone; it’s an expansion of what’s possible, a move towards more intuitive and responsive search capabilities.

This book delves into the practical applications of vector search in Elastic and embodies a broader philosophy. It underscores the importance of search in the age of Generative AI and Large Language Models. This narrative goes beyond the ‘how’ to address the ‘why’ – highlighting our belief in the transformative power of search and our dedication to pushing boundaries to meet and exceed customer expectations.

As you explore these pages, I hope you discover not just valuable information but also inspiration. May this book serve as both a guide and a resource as you navigate today’s challenges and unlock the potential of tomorrow’s opportunities.

Shay Banon

Founder, Elastic

### Contributors

#### About the authors

**Bahaaldine Azarmi**, Global VP Customer Engineering at Elastic, guides companies as they leverage data architecture, distributed systems, machine learning, and generative AI. He leads the customer engineering team, focusing on cloud consumption, and is passionate about sharing knowledge to build and inspire a community skilled in AI.

*For Aurelia, June, Colin-Harper, and Samendar*

**Jeff Vestal** has a rich background spanning over a decade in financial trading firms and extensive experience with Elasticsearch. He offers a unique blend of operational acumen, engineering skill, and machine learning expertise. As a Principal Customer Enterprise Architect, he excels at crafting innovative solutions, leveraging Elasticsearch’s advanced search capabilities, machine learning features, and generative AI integrations, adeptly guiding users to transform complex data challenges into actionable insights.

*For Colin, Sloane, and Maureen*

#### About the reviewers

**Felipe Besson** is an experienced professional deeply devoted to domains of relevance and Machine Learning Engineering. He holds a Master’s degree in Computer Science from the University of Sao Paulo and has a decade of experience in E-commerce Search. As a Relevance Engineer, Felipe focuses on having a good balance of data science and engineering skills to become better in his role and push the boundaries of information retrieval. He also had the opportunity to share some of his experience at great data conferences like Berlin Buzzwords, MICEs, and Haystack, contributing to the practical side of search components. Currently, Felipe drives the Relevance Engineering discipline at Shopify’s Discovery experiences team.

**Jenny Morris** is a software technologist with over 20 years of experience. She is dedicated to developing effective go-to-market strategies that enable customers to leverage technology for their business goals. She loves working closely with customers, understanding their needs, and crafting solutions to help them succeed. With a strong background in application development, Jenny still finds coding to be a source of genuine creative satisfaction.

### 前言

你好，欢迎来到 Elastic 向量搜索的动态世界——在那里，向量搜索技术的精确性和强大功能重新定义了数据分析和检索的边界。在这个快速发展的领域中，Elastic 是一个关键平台，改变了我们在各个领域与大型数据集交互和理解的方式。在本书中，我们将探索向量和嵌入，它们是现代机器学习的核心，并正在彻底改变 Elasticsearch 中的搜索功能。从向量概念的基础介绍到其在网络安全和对话式 AI 中的高级应用，每一章都更深入地进入这个引人入胜的世界。

阅读本书的旅程不仅仅是理解技术细节；更是欣赏这些技术的更广泛影响。我们探讨向量如何增强图像搜索，带来新的精确度和相关性。我们深入了解向量在保护敏感数据中的关键作用，并探索它们在可观测性背景下的日益增长的重要性，在那里它们简化并增强了系统监控。这段旅程的一个关键方面是这些概念的实际应用。本书不仅涉及理论基础，还提供实施这些解决方案的实践见解。无论是调整 Elasticsearch 的性能、管理复杂模型，还是集成检索增强生成等高级功能，各章节都为充分利用 Elastic 向量搜索的潜力提供了全面指南。

本书汲取了丰富的经验和真实应用，不仅是一本技术指南，也是数据搜索和分析未来的窗口。随着数据和技术格局的不断演变，这里分享的见解和知识对于任何希望在搜索和数据分析领域保持领先的人来说都将是无价的。

在阅读本书时，你将了解 Elastic 的功能，并看到向量搜索如何改变我们处理数据的方式。本书面向像你这样的专业数据人士，提供清晰、有用的信息，以提高你在这一令人兴奋领域的技能。

### 本书适合谁

本书非常适合对 Elastic 向量搜索感兴趣的多个群体：

- 数据科学家和分析师：那些希望加深对向量搜索技术的理解并将这些概念应用于复杂数据集的人会发现本书非常有用。它提供了利用向量进行更细致的数据分析和搜索功能的实践见解。
- Elasticsearch 开发者：已经使用 Elasticsearch 的专业人士将发现增强其现有技能的高级技术。本书涵盖从基础概念到网络安全和聊天机器人增强等更高级应用的一切，提供全面的学习路径。
- IT 和数据管理专业人士：对于负责管理和保护数据的人来说，本书提供了关于如何使用向量搜索来 redact 敏感信息和优化数据搜索系统的基本知识。
- 技术爱好者和学生：任何对最新搜索技术及其在各个领域的应用感兴趣的人都会发现本书是一个宝贵的资源。它提供了对向量搜索世界的清晰、易懂的介绍，以及实践应用。

通过阅读本书，你不仅会获得理论知识，还会获得在 Elastic 中实施和创新向量搜索的实践技能，从而增强你个人和专业的 data 管理能力。

### 本书涵盖内容

第 1 章，Vectors and Embeddings 简介，涵盖机器学习中嵌入的基础知识。

第 2 章，使用 Elastic 开始向量搜索，探索 Elastic 中搜索的演进，从传统的基于关键词的方法到先进的向量搜索。

第 3 章，Elastic 中的模型管理和向量考虑，深入探讨在 Elasticsearch 中管理嵌入模型，探索 Hugging Face 的平台、Elastic 的 Eland 库和集成策略。

第 4 章，性能调优——与数据协作，深入探讨使用 ML 模型部署调优和节点容量估计来优化 Elasticsearch 中的向量搜索性能。本章还将涵盖使用 Rally 进行负载测试和排查 kNN 搜索响应时间。

第 5 章，图像搜索，探索图像相似性搜索这一不断发展的领域及其在发现应用中的日益增长的重要性。

第 6 章，使用 Elasticsearch redact 个人身份信息，介绍如何在 Elasticsearch 中构建和定制 PII Redaction Pipeline，这对数据隐私和安全至关重要。

第 7 章，由向量驱动的可观测性新一代，深入探讨在 Elastic 平台上将向量与可观测性集成，重点关注日志分析、指标分析和应用性能监控。

第 8 章，向量和嵌入在加强网络安全中的力量，探索 Elastic Learned Sparse EncodeR (ELSER) 及其在网络安全语义搜索中的作用。它解释了 ELSER 在文本分析和钓鱼检测方面的能力。




第 9 章《使用 Elastic 进行检索增强生成》，深入探讨了 Elastic 中的检索增强生成（RAG），融合了词汇、向量和上下文搜索。

第 10 章《为 ChatGPT 构建 Elastic 插件》，展示了如何利用 Elasticsearch 和 Embedchain 增强 ChatGPT 的上下文感知能力，为最新信息检索创建动态上下文层（DCL）。

### 如何充分利用本书

要充分受益于本书，读者应具备 Elasticsearch 操作的基础知识、基本的 Python 编程技能，以及对搜索概念的一般熟悉程度。这些基础知识将使读者更有效地掌握本书中讨论的高级技术和应用。

| 书中涵盖的软件/硬件 | 操作系统要求 |
|---|---|
| Elasticsearch 8.11+ | Windows、macOS 或 Linux |
| Python 3.9+ |  |
| Jupyter Notebook |  |

如果您使用的是本书的数字版，我们建议您自行输入代码或从本书的 GitHub 仓库访问代码（链接在下一节提供）。这样做将有助于避免因复制和粘贴代码而产生的潜在错误。

### 下载示例代码文件

您可以从 GitHub https://github.com/PacktPublishing/Vector-Search-for-Practitioners-with-Elastic 下载本书的示例代码文件。如果代码有更新，将在 GitHub 仓库中更新。

我们还有来自我们丰富的图书和视频目录的其他代码包，可在 https://github.com/PacktPublishing/ 获取。快来看看吧！

### 使用的约定

本书中使用了多种文本约定。

文中代码：表示文中代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号。示例：“默认管道使用来自 Hugging Face 的 dslim/bert-base-NER 模型。”

代码块如下所示：

```
{ '_source':
  {
    'redacted': '<PER> called in from 001-<PHONE>x1311. Their account
number is <SSN>'
    'status': 'active'
  }
}
```

**粗体**：表示新术语、重要词语或您在屏幕上看到的词语。例如，菜单或对话框中的词语以**粗体**显示。示例：“该库建立在官方 Elasticsearch Python 客户端之上，并扩展了 **pandas API** 到 Elasticsearch。”

**提示或重要说明**
如下所示出现。

### 联系我们

我们始终欢迎读者的反馈。

**一般反馈**：如果您对本书的任何方面有疑问，请发送电子邮件至 `customercare@packtpub.com`，并在邮件主题中提及书名。

**勘误**：尽管我们已尽一切努力确保内容的准确性，但难免会出错。如果您在本書中发现错误，我们将不胜感激您向我们报告。请访问 `www.packtpub.com/support/errata` 并填写表格。

**盗版**：如果您在互联网上以任何形式发现我们作品的非法副本，我们将不胜感激您提供位置地址或网站名称。请发送邮件至 `copyright@packt.com` 并附上材料链接。

**如果您有兴趣成为作者**：如果您在某个领域有专业知识，并且有兴趣撰写或为图书做出贡献，请访问 `authors.packtpub.com`。

## 分享您的想法

读完 *Vector Search for Practitioners with Elastic* 后，我们很想听听您的想法！请点击此处直接前往本书在 Amazon 的评论页面并分享您的反馈。

您的评论对我们和技术社区都很重要，并将帮助我们确保提供优质内容。

## 下载本书的免费 PDF 副本

感谢您购买本书！

您喜欢随时阅读但无法随身携带印刷书籍吗？

您的电子书购买与您选择的设备不兼容？

不用担心，现在每本 Packt 书籍都免费提供一本无 DRM 的 PDF 版本。

在任何地方、任何地点、在任何设备上阅读。直接从您喜爱的图书中搜索、复制和粘贴代码到您的应用程序中。

福利还不止于此，您还可以每天通过电子邮件获得独家折扣、新闻通讯和精彩免费内容

按照以下简单步骤获取福利：

+   1. 扫描二维码或访问下方链接
+   2. 提交您的购买证明
+   3. 就这样！我们会直接将免费 PDF 和其他福利发送到您的电子邮件

![](img/21c3d4250a746422a7489bc2647611d8_18_0.png)

https://packt.link/free-ebook/9781805121022
