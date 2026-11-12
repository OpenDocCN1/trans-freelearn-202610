# LightGBM 和 Python 机器学习实战：探索微软的梯度提升框架以优化机器学习





![](img/31b70e4a77c3afbe11032f68ab874b02_0_0.png)

完成本书花费了无数个夜晚和周末，我想感谢我的妻子艾琳，感谢她永恒的支持，没有她的支持，就没有人会读到这些内容。此外，我感谢我的女儿艾米丽，激励我不断进取。

- 安德里奇·范·韦克

## 贡献者

## 关于作者

安德里奇·范·韦克在机器学习研发、构建 AI 驱动解决方案以及 AI 领域咨询方面拥有 15 年的经验。他还作为软件工程师和架构师拥有广泛经验，在企业系统领域有超过十年的行业经验。

他以优异成绩获得比勒陀利亚大学计算机科学硕士学位，专注于神经网络和进化算法。

安德里奇喜欢撰写关于机器学习工程及软件行业的文章。他目前与妻子和女儿居住在南非。

## 关于审稿人

瓦伦丁·舒库洛夫是一所顶尖科技大学的知名客座讲师，他将学术界与现实世界的专业知识完美融合，作为金融科技和电子商务领域的杰出数据科学家。他在构建机器学习驱动解决方案方面的独创性，改变了从科技巨头到初创企业的各种业务。瓦伦丁擅长引入 AI 创新并改进现有系统，确保它们对重要业务指标产生深远影响。他对攻克产品挑战的热情，使他成为利用 ML 提升业务水平的先驱。

最重要的是，衷心感谢我的配偶，在我非凡旅程中始终如一的支持者。

卡扬·M·卡兰吉亚在机器学习、人工智能（AI）和数据技术领域拥有超过 7 年的经验，为他目前在 DrDoctor 的职位带来了丰富的专业知识。在此，作为机器学习工程师，他致力于实施先进的机器学习模型，这些模型直接影响医疗服务改善以及 NHS 的流程优化。过去，他还与 Reliance Industries Limited 等多家跨国公司合作，并为印度政府实施了解决方案。


欢迎阅读《使用 LightGBM 和 Python 进行机器学习：构建生产就绪机器学习系统的实践者指南》。在这本书中，您将踏上一段丰富的旅程，从机器学习的基础原理一直深入到 MLOps 的高级领域。我们探索的基石是 LightGBM，这是一个强大且灵活的梯度提升框架，可应用于广泛的机器学习挑战。

此书专为那些渴望利用 **机器学习**（ML）的力量将原始数据转化为可操作洞察的人而量身定制。无论您是渴望动手实践的 ML 新手，还是希望精通 LightGBM 复杂性的资深数据科学家，本书都适合您。

数字时代为我们提供了数据宝库。然而，挑战往往在于从这些数据中提取有意义的洞察，并在生产环境中部署可扩展、高效且可靠的模型。本书将指导您克服这些挑战。通过深入学习梯度提升、数据科学生命周期以及生产部署的细节，您将掌握一整套全面的技能，以应对不断变化的 ML 领域。

每章的设计都充分考虑了实用性。真实世界的案例研究与理论见解交织在一起，确保您的学习植根于具体应用。我们对 LightGBM 的关注——它有时会被更主流的算法所掩盖——为欣赏和在各种场景中应用梯度提升提供了独特的视角。

对于那些好奇本书与众不同之处的人，答案在于我们务实的方法。我们以超越单纯解释算法或工具而自豪。相反，我们将优先考虑动手实践、案例研究和现实世界挑战，确保您不仅是在阅读，更是在“实践”ML。

在浏览各章时，请记住 ML 的世界广阔且不断发展。尽管本书内容全面，但它只是您在该领域终身学习和探索旅程中的一块垫脚石。在探索 LightGBM、数据科学、MLOps 等世界时，请保持开放的心态，保持好奇心，并准备好动手编码。

## 本书适用人群

_使用 LightGBM 和 Python 进行机器学习：构建生产就绪机器学习系统的实践者指南_ 专为渴望利用 ML 的力量挖掘数据潜力的广泛读者而设计。本书的目标受众包括：

- **ML 初学者：** 刚踏入 ML 领域的个人将发现本书非常有益。它从基础 ML 原理开始，并向他们介绍使用 LightGBM 进行梯度提升，使其成为新手的绝佳入门读物。
- **经验丰富的数据科学家和 ML 从业人员：** 对于已经熟悉 ML 领域但希望加深 LightGBM 和/或 MLOps 知识的人，本书提供了高级见解、技巧和实际应用。
- **渴望了解更多数据科学知识的软件工程师和架构师：** 希望转向数据科学或将 ML 集成到其应用程序中的软件专业人员将发现本书有价值。本书从理论和实践两个角度阐述 ML，强调动手编码和实际应用。
- **MLOps 工程师和 DevOps 专业人员：** 从事 MLOps 领域工作或希望了解生产环境中 ML 模型的部署、扩展和监控的人，将受益于专门介绍 MLOps、管道和部署策略的章节。
- **学者和学生：** 讲授 ML、数据科学或相关课程的教职人员，以及攻读这些领域的学生，都会发现本书既是一本信息丰富的教科书，也是一本实用指南。

必须具备 Python 编程知识。熟悉 Jupyter 笔记本和 Python 环境是加分项。无需事先具备 ML 知识。

简而言之，任何对数据有偏爱、具备 Python 编程背景并渴望使用 LightGBM 探索 ML 多元世界的人，都会发现本书是其知识库中值得添加的宝贵资源。

## 本书内容概览

*第 1 章，机器学习简介，* 开启我们进入 ML 的旅程，从软件工程的视角审视 ML。我们将阐述该领域核心的重要概念，如模型、数据集以及各种学习范式，并通过使用决策树的动手示例确保清晰易懂。

*第 2 章，集成学习——装袋与提升，* 深入探讨集成学习，重点介绍应用于决策树的装袋和提升技术。我们将探讨随机森林、梯度提升决策树等算法，以及 **脱校正遇加性回归树（DART）** 等更高级的概念。

*第 3 章，Python 中的 LightGBM 概述，* 考察 LightGBM，这是一个具有基于树学习器的高级梯度提升框架。重点介绍其独特的创新和对集成学习的改进，我们将引导您了解其 Python API。使用 LightGBM 的全面建模示例（配备高级验证和优化技术），为深入探索数据科学和生产系统 ML 铺平了道路。

*第 4 章，LightGBM、XGBoost 与深度学习的对比，* 将 LightGBM 与两种突出的表格数据建模方法——XGBoost 和 **深度神经网络（DNNs）**（具体为 TabTransformer）——相抗衡。我们将通过对两个数据集的评估来评估每种方法的复杂性、性能和计算成本。本章的精髓在于确定 LightGBM 在更广泛 ML 格局中的竞争力，而非深入研究 XGBoost 或 DNN。

第 5 章，使用 Optuna 进行 LightGBM 参数优化，聚焦于超参数优化这一关键任务，介绍 Optuna 框架作为一种强大的解决方案。涵盖各种优化算法和剪枝超参数空间的策略，本章将引导您通过使用 Optuna 优化 LightGBM 参数的动手示例。

第 6 章，使用 LightGBM 解决现实世界数据科学问题，系统地分解数据科学流程，并将其应用于两个不同的案例研究——一个回归问题和一个分类问题。本章阐明了数据科学生命周期的每个步骤。您将体验使用 LightGBM 进行动手建模，并辅以全面的理论。本章也可作为使用 LightGBM 开展数据科学项目的蓝图。

第 7 章，使用 LightGBM 和 FLAML 进行 AutoML，深入探讨自动化机器学习（AutoML），强调其在简化和加速数据工程及模型开发方面的重要性。我们将介绍 FLAML，这是一个使用高效超参数算法自动化模型选择和微调的知名库。通过一个实际案例研究，您将目睹 FLAML 与 LightGBM 的协同效应，以及改变游戏规则的零样本 AutoML 功能，该功能使调优过程变得多余。

第 8 章，使用 LightGBM 进行机器学习管道和 MLOps，从建模的复杂性转向生产 ML 的世界。它向您介绍 ML 管道，以确保一致的数据处理和模型构建，并涉足 MLOps（DevOps 和 ML 的融合），这对于部署弹性的 ML 系统至关重要。

第 9 章，使用 AWS SageMaker 进行 LightGBM MLOps，将我们的旅程引向 Amazon SageMaker，即 Amazon Web Services 用于构建和维护 ML 解决方案的综合套件。我们将通过深入研究偏差检测、模型可解释性以及自动化、可扩展部署的细微差别等高级领域，加深对 ML 管道的理解。

第 10 章，使用 PostgresML 的 LightGBM 模型，介绍 PostgresML，这是一个独特的 MLOps 平台和 PostgreSQL 数据库扩展，可直接通过 SQL 促进 ML 模型的开发和部署。这种方法虽然与我们所采用的 scikit-learn 编程风格形成对比，但展示了数据库级 ML 的优势，尤其是在数据移动效率和更快推理方面。

第 11 章，使用 LightGBM 进行分布式和基于 GPU 的学习，深入探讨利用分布式计算集群和 GPU 训练 LightGBM 模型的广阔领域。通过利用分布式计算，您将了解如何大幅加速训练工作负载，并管理超出单台机器内存容量的数据集。





## 为了最大限度地利用本书

本书假设你具备一些 Python 编程知识。本书中的 Python 代码都不太复杂，因此即使只了解 Python 的基础知识，也足以让你看懂大部分代码示例。

所有章节的实践示例都使用 Jupyter Notebooks。Jupyter Notebooks 是一种开源工具，允许你创建包含实时代码、可视化和 Markdown 文本的代码笔记本。关于 Jupyter Notebooks 入门的教程可在 `https://realpython.com/jupyter-notebook-introduction/` 和 `https://plotly.com/python/ipython-notebook-tutorial/` 上找到。

| 本书涵盖的软件/硬件 | 操作系统要求 |
|---|---|
| Python 3.10 | Windows、macOS 或 Linux |
| Anaconda 3 | Windows、macOS 或 Linux |
| scikit-learn 1.2.1 | Windows、macOS 或 Linux |
| LightGBM 3.3.5 | Windows、macOS 或 Linux |
| XGBoost 1.7.4 | Windows、macOS 或 Linux |
| Optuna 3.1.1 | Windows、macOS 或 Linux |
| FLAML 1.2.3 | Windows、macOS 或 Linux |
| FastAPI 0.103.1 | Windows、macOS 或 Linux |
| Amazon SageMaker | |
| Docker 23.0.1 | Windows、macOS 或 Linux |
| PostgresML 2.7.0 | Windows、macOS 或 Linux |
| Dask 2023.7.1 | Windows、macOS 或 Linux |

我们建议在设置自己的环境时使用 Anaconda 进行 Python 环境管理。Anaconda 还捆绑了许多数据科学包，因此你无需单独安装它们。Anaconda 可从 `https://www.anaconda.com/download` 下载。值得注意的是，本书附带一个 GitHub 仓库，其中包含一个 Anaconda 环境文件，用于创建运行本书代码示例所需的环境。

**如果你使用的是本书的数字版，我们建议你亲自输入代码，或从本书的 GitHub 仓库获取代码（下一节中提供了链接）。这样做可以帮助你避免与复制和粘贴代码相关的任何潜在错误。**

## 下载示例代码文件

你可以从 GitHub 下载本书的示例代码文件：`https://github.com/PacktPublishing/Practical-Machine-Learning-with-LightGBM-and-Python`。如果代码有更新，将在 GitHub 仓库中进行更新。

我们还提供来自丰富图书和视频目录的其他代码包，网址为 https://github.com/PacktPublishing/。欢迎查看！

## 本书使用的约定

书中使用了若干文本约定。

正文中的代码：用于指示正文中的代码字词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号。例如：“The code is almost identical to our classification example – instead of a classifier, we use DecisionTreeRegressor as our model and calculate mean_absolute_error instead of the F1 score。”

代码块设置如下：

```
import numpy as np
import pandas as pd
from matplotlib import pyplot as plt
import seaborn as sns
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_absolute_error
```

当我们希望引起你对代码块中特定部分的注意时，相关行或项目将以粗体显示：

```
model = DecisionTreeRegressor(random_state=157, max_depth=3, min_samples_split=2)
**model = model.fit(X_train, y_train)**
mean_absolute_error(y_test, model.predict(X_test))
```

命令行输入或输出写作如下：

```
conda create -n your_env_name python=3.9
```

粗体：表示新术语、重要词汇或屏幕上看到的词汇。例如，菜单或对话框中的单词以粗体显示。例如：“因此，**数据准备和清洗**是机器学习过程的重要组成部分。”

> **提示或重要说明**
>
> 此类块中会出现此类内容。

## 联系我们

我们始终欢迎读者的反馈。

**一般反馈：** 如果你对本书的任何方面有疑问，请发送电子邮件至 customercare@packtpub.com，并在邮件主题中注明书名。

**勘误：** 虽然我们已竭力确保内容的准确性，但错误难免会发生。如果你在本书中发现错误，我们将不胜感激地接受你的反馈。请访问 www.packtpub.com/support/errata 并填写表格。

**盗版：** 如果你在互联网上发现我们作品的任何非法副本，我们将不胜感激地接受你提供的位置地址或网站名称。请使用包含材料链接的邮件联系 copyright@packt.com。

**如果你有兴趣成为作者：** 如果你在某个主题上有专业知识，并且有兴趣撰写或参与编写书籍，请访问 authors.packtpub.com。

## 分享你的想法

读完 *使用 LightGBM 和 Python 进行机器学习* 后，我们很乐意听取你的想法！请点击此处前往本书的 Amazon 评论页面并分享你的反馈。

你的评论对我们和技术社区非常重要，并将帮助我们确保提供优质内容。

## 下载本书的免费 PDF 副本

感谢你购买本书！

你是否喜欢随时随地阅读，但无法随身携带纸质书？你购买的电子书是否与你选择的设备不兼容？

别担心，现在每本 Packt 书籍都附赠该书的无 DRM PDF 版本，且无需额外付费。

随时随地、在任何设备上阅读。直接从你喜爱的技术书籍中搜索、复制和粘贴代码到你的应用程序中。

福利不仅如此，你每天还可以在收件箱中独家获取折扣、新闻通讯和精彩免费内容。

按照以下简单步骤获取福利：

1. 扫描二维码或访问以下链接

![](img/31b70e4a77c3afbe11032f68ab874b02_19_0.png)

https://packt.link/free-ebook/9781800564749

2. 提交购买证明

3. 完成！我们会将免费 PDF 和其他福利直接发送到你的电子邮箱。
