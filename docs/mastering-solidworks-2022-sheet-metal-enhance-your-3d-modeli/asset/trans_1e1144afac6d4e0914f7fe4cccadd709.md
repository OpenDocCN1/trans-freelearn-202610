

## 联系我们

我们非常欢迎读者的反馈。

**一般反馈：** 如果您对本书的任何方面有疑问，请发送电子邮件至 `customercare@packtpub.com`，并在邮件主题中提及书名。

**勘误：** 尽管我们已尽一切努力确保内容的准确性，但错误难免。如果您在本書中发现错误，我们将不胜感激您向我们报告。请访问 `www.packtpub.com/support/errata` 并填写表格。

**盗版：** 如果您在互联网上发现我们作品的任何形式的非法复制，我们将不胜感激您提供位置地址或网站名称。请通过 `copyright@packt.com` 联系我们并附上材料链接。

**如果您有兴趣成为作者：** 如果您在某个领域具有专业知识，并且有兴趣撰写或为一本书做出贡献，请访问 `authors.packtpub.com`。

## 分享您的想法

在您阅读完 *Mastering SOLIDWORKS Sheet Metal* 后，我们很想听听您的想法！请点击此处直接前往本书在 Amazon 的评论页面并分享您的反馈。

您的评论对我们和技术社区都很重要，并将帮助我们确保提供优质内容。

## Section 1:
Getting Started with
the Sheet Metal Module

每段旅程都始于第一步。我们的第一个目标将是介绍 SolidWorks Sheet Metal 是什么以及我们如何使用它。然后我们将探索创建和使用简单 Sheet Metal 模型和工程图的基础知识。

本节包含以下章节：

- Chapter 1, Sheet Metal Basics – Exploring Sheet Metal Properties and Material Selection
- Chapter 2, Adding Bends Using Edge Flanges
- Chapter 3, Getting Familiar with Basic Tools in Sheet Metal
- Chapter 4, Creating Sheet Metal Drawings and Exporting Files

## 1
Sheet Metal Basics
– Exploring Sheet Metal Properties and Material Selection

SolidWorks 是全球领先的 **三维 (3D) 计算机辅助设计 (CAD)** 建模软件之一，拥有数百万用户。然而，虽然许多人使用 SolidWorks 进行 3D 实体建模，但也有许多更专业的模块用于创建其他类型的模型。SolidWorks Sheet Metal 模块是其中最重要的一项，因为它释放了钣金制造和生产巨大的潜力。

在本章中，我们将探讨钣金是什么以及为什么它很重要。我们将学习如何使用 **Base Flange** 创建 Sheet Metal 模型以及如何编辑模型的 **Sheet Metal 属性。**

我们还将介绍 **Flattening** *parts* 并简要了解最常用于钣金的材料。

到本章结束时，您将了解如何开始 Sheet Metal *parts* 以及如何调整其基本参数，同时对钣金制造和使用也有更广泛的了解。

在本章中，我们将涵盖以下主要主题：

- Introducing Sheet Metal
- Creating a **Base Flange**
- **Sheet Metal** properties
- Other **Base Flange** options and flattening parts
- Considerations when selecting sheet metal materials

### Technical requirements

要完成本节及所有后续章节，您需要一份 SolidWorks 软件程序的副本，并具备基本的操作知识。这包括如何执行诸如开始新的 *parts*、创建和编辑草图和特征以及使用常用工具等操作。对于完全不熟悉 SolidWorks 的读者，建议先打好实体建模的基础，然后再学习本书涵盖的 Sheet Metal 模块。

在本书的课程中，如果某个教程或工具出现问题，请尝试重新检查所有之前的步骤，甚至尝试关闭 *part* 并从头开始。

SolidWorks 中的错误消息通常描述性很强，并且通常会告诉用户确切的问题所在。因此，如果出现错误，请避免在未正确阅读的情况下点击 **OK**，并尝试执行消息中建议的操作。

如果问题仍然存在，请随时通过 Twitter 在线联系我，网址为 www.twitter.com/johnoellison。

### SolidWorks versions

本书中的示例和演示均使用 SolidWorks 2021 制作。SolidWorks 的其他版本在界面和某些选项的标签等方面可能存在非常细微的差异，但总体而言，工作流程应该非常相似。如果您在这些差异上遇到困难，请使用前面提供的详细信息联系我。

### Units of measurement

在整本书中，**毫米** (**mm**) 被用作测量单位，但可以根据读者的偏好进行调整，通过将模型的单位设置为 **英寸** (')，例如，然后在添加尺寸时在 **Smart Dimension** 工具中输入毫米数值后跟 mm。这将自动将毫米值转换为英寸（或您选择的单位类型）。

### Terminology

在整本书中，“sheet metal”（小写）指的是现实世界中通用的行业和制造方法，而“Sheet Metal”（大写）则特指 SolidWorks Sheet Metal 模块。

术语“part”指的是现实生活中的组件或物品，而 *part*（斜体）则特指 SolidWorks *part* 文档。

## Introducing Sheet Metal

钣金零件是指——顾名思义——由金属平板制成的零件。众多不同的制造技术，如弯曲、切割和成型，使这些简单的平板能够转变为复杂的 3D 零件。钣金的流行在现代时代急剧增长，因为它允许设计师和工程师使用广泛可用的材料类型——平板——并使用相对低成本的工具和工艺以工业规模创建复杂产品。

很可能您在家里、车库或工作场所快速环顾四周，就会发现数十件使用钣金技术制作的物品。这些物品的范围从非常简单的物品，如直角支架，到更复杂的产品，如家具，一直到精细的设计，如计算机或电子外壳。钣金还用于制造飞机零件或汽车车身等非常复杂的物品，尽管最先进的成型零件超出了 SolidWorks Sheet Metal 的范围。

![](img/4e9c736de6bac4449096a7b92c28af4c_22_0.png)

Figure 1.1: Examples of sheet metal parts

SolidWorks Sheet Metal 模块的美妙之处在于它允许用户创建这些 3D 形状，然后将其展平为板材，并导出为可用于制造的 **二维 (2D)** 设计。

SolidWorks Sheet Metal 的另一个优点是，尽管名称如此，它不必仅用于纯金属零件。Sheet Metal 可用于创建任何类型的可展平 3D 零件，无论其现实生活中的材料如何。尝试跳出“金属”标题的思维，就能解锁更广泛的用途。例如，Sheet Metal 可用于创建纸板包装。这可以在 3D 中建模，然后在虚拟中展开以便打印，用模切机裁切，然后制成 3D 盒子。Sheet Metal 甚至可用于创建纸艺折纸艺术品等多样化物品。

![](img/4e9c736de6bac4449096a7b92c28af4c_23_0.png)

Figure 1.2: Sheet Metal isn't just for metal but any material, such as this cardboard packaging

Sheet Metal 是一套多样化的制造方法，将把您的建模和设计提升到下一个水平。总结如下：

- Sheet metal uses flat sheets to create 2D or 3D parts.
- Techniques include cutting, bending, and forming.
- SolidWorks Sheet Metal parts don't have to be metal!

在下一节中，我们将了解如何在 SolidWorks 中通过创建 Base Flange 来实际开始制作 Sheet Metal 零件，该 Base Flange 将成为这些零件的基础特征。

## Creating a Base Flange

SolidWorks Sheet Metal 零件可以主要通过两种方式开始——要么创建 Base Flange，要么将现有的 3D 零件转换为 Sheet Metal 零件。本节将介绍 Base Flanges 的使用，这是开始新 Sheet Metal 零件最简单但也最灵活的方式。

