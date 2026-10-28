## 致谢

**作者**
Vanessa S. Olsen

**编辑团队负责人**
Akshara Aware

**审阅者**
Mary Cooch

**项目团队负责人**
Priya Mukherji

**收购编辑**
Sarah Cullington

**项目协调员**
Zainab Bagasrawala

**开发编辑**
Mehul Shetty

**校对**
Chris Smith

**技术编辑**
Aaron Rosario

**制作协调员**
Melwyn D'sa

**索引**
Hemangini Bari

**封面设计**
Melwyn D'sa

## 关于作者

**Vanessa S. Olsen** 是一名言语治疗师。她在治疗中心、学校和医院工作已有五年多。她一直在治疗语言障碍和学习障碍时应用现代技术，并帮助学校让学生留在最不限制的环境中。具体来说，她一直将 Moodle 作为电子学习平台、Alice 以及其他工具与 Web 2.0 和通用现代硬件（如游戏手柄、笔迹素描、触摸屏、上网本和操纵杆）的使用相结合。她喜欢帮助儿童和青少年提高技能。

她与丈夫 Gaston 和他们的儿子 Kevin 一起生活。当她不工作时，她将时间奉献给家人和爱好。她喜欢用冷瓷制作模型、游泳，并研究新的技术和技术以应用于她的治疗。

您可以通过 vanesaolsen@gmail.com 和 olsenvanesa@live.com 联系她。

## 致谢

在撰写本书期间，我很幸运能够与 Packt Publishing Ltd 的优秀团队合作，他们的贡献极大地改善了本书的呈现。Sarah Cullington 帮助我将想法转变为最终的书籍并塑造了本初学者指南。Zainab Bagasrawala 帮助我遵循有组织的进度。Mehul Shetty 就文本、格式和流程提供了许多明智的建议。读者会注意到他的出色工作。Aaron Rosario 确保所有练习都是准确的，并为最终草案增添了极大的价值。

我谨感谢我的审阅者 Mary Cooch (http://www.moodleblog.org)，感谢她彻底的审阅和富有洞察力的评论。我能够将她在多年使用 Moodle、Hot Potatoes 和虚拟学习环境方面的知识和智慧融入其中。练习和图片包含了 Mary 的宝贵反馈。

特别感谢我的丈夫 Gaston Hillar，他几年前建议我在治疗环境中使用新技术，并激励我开展这个新项目。此外，我还要感谢我的小姨子 Silvina Hillar，她也以教学专长提供了帮助；我的儿子 Kevin 和我的侄子 Nicolas，他们以爱意丰富了我的生活；我的朋友们，以及我的岳父。

## 关于审阅者

**Mary Cooch**（在线昵称为 **Moodlefairy**）是一名教师、VLE 培训师，专门从事 Moodle，并是 Packt 的 *Moodle 1.9 For Teaching 7-14 Year Olds* 和 *Moodle 2.0 First Look* 的作者。她在英国兰开夏郡普雷斯顿的 Our Lady's Catholic High School 工作，可通过 mcooc@olchs.lancs.sch.uk 联系进行培训和咨询。

> 致我家里的家人，以及学校里的 Mark；感谢你们的支持。

我谨将此书献给我儿子 Kevin

# 本书所需准备

本书中练习的一些重要前提条件如下：

- Moodle 1.9.x（Moodle 1.9.5或更高版本，不是Moodle 2.0）
- Microsoft Word
- Inkscape 0.47
- JClic for Moodle
- GIMP
- Edraw Max 5.1
- Wondershare PPT to Video 6

# 本书读者对象

如果你是一名对Moodle了解甚少的特殊教育（SEN）教师或治疗师，并且愿意以Moodle 1.9为后台平台来利用Web 2.0的各种可能性，那么这本书就是为你准备的。

# 体例说明

在本书中，你会看到多个频繁出现的标题。

为了清晰说明如何完成某个步骤或任务，我们使用：

# 操作实践——标题

1. 操作1
2. 操作2
3. 操作3

操作说明通常需要一些额外解释才能让人理解，因此后面会跟随：

# 刚才发生了什么？

该标题解释了你刚刚完成的任务或操作的工作原理。

你还会在本书中找到其他学习辅助内容，包括：

这些是简短的单项选择题，旨在帮助你检验自己的理解。

这些内容设置实际挑战，并为你提供一些关于如何将所学知识进行实验的想法。

你还会看到多种文本样式，用于区分不同类型的信息。以下是一些样式示例及其含义。

文中的代码显示如下：“我们可以通过使用include指令来包含其他上下文。”

代码块设置如下：

```
```html
<h2 style="color:blue">
<em>There is a big blackboard and a big eraser</em>
</h2>
```
```

当我们希望引起你对代码块中某一部分的注意时，相关行或项目会以粗体显示：

```
```html
<h2 style="color:blue">
<em>There is a big blackboard and a big eraser</em>
</h2>
```
```

新术语和重要词语以粗体显示。你在屏幕上看到的文字，例如菜单或对话框中的文字，会以如下形式出现在文本中：“点击“下一步”按钮会进入下一个屏幕。”

警告或重要说明会出现在这样的方框中。

提示和技巧显示如下。

# 读者反馈

我们始终欢迎读者的反馈。请告诉我们你对这本书的看法——你喜欢哪些内容，或者可能不喜欢哪些内容。读者反馈对我们开发真正能让读者受益的图书非常重要。

如需向我们发送一般性反馈，只需发送电子邮件至feedback@packtpub.com，并在邮件主题中注明书名。

如果你需要某本书，并希望我们出版，请通过www.packtpub.com上的“SUGGEST A TITLE”表单发送说明，或发送电子邮件至suggest@packtpub.com。

如果你在某方面有专长，并且有兴趣撰写或参与编写一本书，请参阅我们在www.packtpub.com/authors上的作者指南。

# 客户支持

既然你现在已经是Packt图书的拥有者，我们提供多项服务来帮助你充分利用所购图书。

下载本书的示例代码
你可以从http://www.PacktPub.com的账户中下载所购买的所有Packt图书的示例代码文件。如果你在其他地方购买了这本书，可以访问http://www.PacktPub.com/support并注册，以便将文件直接通过电子邮件发送给你。

## 勘误

尽管我们已尽一切努力确保内容的准确性，但错误仍难免发生。如果你在我们的某本书中发现错误——可能是文本或代码中的错误——我们将不胜感激，希望你能向我们报告。这样做可以帮助其他读者免于困惑，并帮助我们改进本书的后续版本。如果你发现任何勘误，请访问http://www.packtpub.com/support，选择你的图书，点击“let us know”链接，并输入勘误详情。一旦你的勘误得到验证，你的提交将被接受，勘误将上传到我们的网站，或添加到该标题的“勘误”部分下的现有勘误列表中。你可以通过从http://www.packtpub.com/support选择你的标题来查看任何现有勘误。

## 盗版

互联网上对版权材料的盗版是所有媒体持续存在的问题。在Packt，我们非常重视对我们版权和许可的保护。如果你在互联网上以任何形式发现我们的作品的非法副本，请立即向我们提供位置地址或网站名称，以便我们采取补救措施。

请通过copyright@packtpub.com联系我们，并提供疑似盗版材料的链接。

我们感谢你帮助保护我们的作者以及我们为你提供有价值内容的能力。

## 问题

如果你在本书的任何方面遇到问题，可以通过questions@packtpub.com联系我们，我们将尽力解决。




