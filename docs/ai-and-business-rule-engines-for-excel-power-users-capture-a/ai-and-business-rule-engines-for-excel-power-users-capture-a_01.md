## 序言

近年来，人们对 **人工智能（AI）** 的兴趣呈爆炸式增长，这让我比以往任何时候都更加坚信：基于规则的人工智能方法，连同机器学习，是 Excel 用户最容易上手的路径。这一信念基于我和我的团队与像您这样的业务用户一起工作的经验，他们希望以易于共享和执行的方式捕获业务知识和规则。目前，社区对新一代工具尤为兴奋，这些工具让决策模型与机器学习模型的联合部署比以往任何时候都更加容易。

距我的第一份代码贡献被 Drools 项目团队接受已经过去约 15 年——这是一个全新的 **Drools 规则语言（DRL）** 解析器，正好赶上 Drools 5 问世。自那时起，我有幸与业界最杰出的工程师们一起学习和工作，并有幸与一个令人惊叹的开源社区合作。

Drools 已经是一个被广泛使用且成熟的开源规则引擎。社区即将发布 Guvnor——一套支持用户管理业务规则生命周期的集成工具，覆盖从编写、打包到部署的全过程。

但此后发生的事情远不止于此。Drools 引擎不仅自身不断发展，还通过引入 PHREAK 算法推动了整个规则引擎和专家系统领域的进步。Drools 也是最早引入统一编程模型的项目之一，该模型让业务规则和工作流并肩运行，共享相同的概念和 API。

除了运行时引擎，工具也急剧扩展。Guvnor 演变为 Business Central，一个完整的**业务规则管理系统（BRMS）**。Business Central 曾有一个雄心勃勃的目标：提供一个用户友好的界面，让开发者和业务实践者能够协作；围绕这一目标取得了进展，为未来的改进奠定了坚实基础。

与所有这些进步并行的是，社区本身也在演进——从单一的 Drools 项目发展为一个领先开源项目家族，这些项目在提供业务自动化和人工智能解决方案方面发挥着重要作用，名为 KIE（Knowledge Is Everything，发音与英语单词 key (/kē/) 相同）。

业务规则和 BRMS 领域的一个转折点是 OMG（**对象管理组织**）引入了**决策模型与表示法（DMN）**规范。这一强大的规范定义了决策模型的图形表示法，以及用于决策建模和运行时执行的一套语义。

如今，DMN 已被广泛公认为决策建模的领先标准，并被世界各地的组织用以管理其决策流程。此外，在 DMN 标准力量的推动下，与 AI 相关的决策管理学科比以往任何时候都更受欢迎。DMN 的另一个优点是厂商中立，人们可以从不受限于任何特定厂商的从业者那里找到各种各样的培训材料、书籍、方法论和其他内容。

Drools 在规范首次发布几年后引入了对 DMN 的官方运行时支持。自那时起，团队加入了 OMG，以帮助塑造规范的未来，并将对业务规则和决策高层表示的所有投入都转向 DMN。随着这一战略的落地以及 DMN 规范日益流行，KIE 社区已准备好开启新一轮创新周期……

Kogito 最初是一项倡议，后来也成为一个项目。彼时，所有 KIE 技术都已容器化，所有投资都以云优先为出发点。然而，团队感到有必要更进一步：我们需要一项新的倡议，以云原生的方式重新思考业务自动化。

这一倡议还催生了一套新工具。首先，**业务流程模型与表示法（BPMN）**和 DMN 编辑器从 Business Central 中抽取出来，并作为 VS Code 的扩展提供。团队并未止步——我们决心让 KIE 技术更易访问，于是推出了 dmn .new，它包含一个运行器，带来一种灵感源自电子表格的动态数据输入建模体验。

这正是 Paul 的新书派上用场的地方：Paul 是社区资深成员，擅长用通俗易懂的语言，为从业者和高级 Excel 用户解读业务规则、决策管理和 AI 的领域。通过本书的内容，您将为自己的技能库增添新的强大工具。

很少有技术能在没有持续创新记录的情况下长期存活并保持其相关性。这个社区的成功很大程度上归功于其营造温暖、友好的环境的能力。Mark Proctor 是长期的朋友和导师，在领导社区方面发挥了不可或缺的作用。

KIE 社区有幸吸引、培养并留住了许多其他有才华的人，首先是 Edson Tirelli 和 Kris Verlaenen，他们为社区的成功奠定了基础；紧随其后的是 Mario Fusco、Maciej Swiderski 和我，我们继续拓展社区并带来新的想法和视角。如今，新一代优秀人才正在崛起，包括 Eder Ignatowicz、Tiago Bento、Matteo Mortari、Daniele Zonca、Enrique Gonzales、Tibor Zimányi 和 Ricardo Zanini，他们代表着社区的未来，并将在未来许多年里助力社区继续蓬勃发展。

就在此刻，随着 IBM 加入 Red Hat 共同管理社区，KIE 社区正在开启一个激动人心的新篇章。我个人非常兴奋能分享这一新篇章。KIE 正在迁往新家：Apache 软件基金会。在 Apache，IBM、Red Hat 和更广泛的社区（包括您）将有机会一起进一步拓展业务自动化和 AI 的边界！

Alex Porcelli

IBM Process Automation and Decision Manager Open Edition 首席架构师

IBM Business Automation





## 贡献者

### 关于作者

**Paul Browne** 目前是一名产品和项目经理，任职于按交易量计算的欧洲最大风投公司。最近，他领导了一个由三家合作机构组成的团队，该团队因生态系统发展而获得 EFMD 奖，培训了超过 100 家欧元区公司共同实施数字解决方案。这延续了 Paul 此前在爱尔兰、英国、美国、荷兰、比利时和新西兰的公共、金融和生命科学领域领导业务和技术项目的经验。

Paul 的资格包括来自 Ulster University 的欧洲商业和法语学位，以及近期的财务（ACCA）、采购（CIPS）和商业战略（ESMT）认证。他拥有微软、甲骨文和 IBM 的技术认证，并持有都柏林大学学院的先进软件工程硕士学位。

Paul 也是 Packt Publishing 出版的 *JBoss Drools Business Rules* 的作者，并在第三级讲授 Excel 和 Visual Basic。

*我谨向 Drools、KIE 和 Kogito 社区，以及 Red Hat、IBM 和 Microsoft 的团队表示感谢，他们提供了出色的 AI 和业务自动化解决方案，并在撰写本书期间给予持续支持。*

### 关于审阅者

**Karina Varela** 在企业技术领域工作超过 10 年，专注于云解决方案、Java 技术和开源文化。

她的扎实知识建立在开发、架构、交付和生产应用程序及产品故障排除的实践经验之上。曾在业界最大的开源公司 Red Hat 工作，她不仅对开源模式和开源社区有深入理解，还在全球多个行业交付架构和关键任务软件方面发挥了重要作用。她还获得了 Red Hat 冠军认可。

Karina 在 Java 社区中享有盛誉，因其文章和博客、书籍、在国际技术会议上的演讲、开源贡献以及积极培育和管理 SouJava 等社区而为人所知。

**Filippe Spolti** 是一名软件工程师，也是开源和云的爱好者。日常工作中，他使用 Kogito Engine for Serverless Workflows。他在 OpenShift V3 早期阶段工作，帮助将中间件产品带入云端。他还曾担任技术支持工程师，帮助客户解决问题并为多个开源项目做出贡献，最近是 CNCF Serverless。他在巴西的本地会议上担任演讲者和轨道协调员。他拥有信息系统学士学位，并在软件工程和信息安全方面有专业化，并撰写了书籍 *WildFly: New Feature*，并帮助其他作者成功出版书籍。

> *分享知识既令人欣慰，也需要奉献和一些远离家人朋友的时间。我感谢他们在过去几年给予我的支持。*

**Nishant Ravle** 是一名 IT 专业人士，拥有超过 16 年的 IT 经验，涉及 BPM、SOA 和中间件技术。他在 Oracle 和 IBM 产品栈方面有广泛的工作经验。目前，他从事软件咨询和企业及在线培训工作。

Nishant 的技术专长涵盖业务流程管理和面向服务架构领域的核心技能和专业知识，使用 IBM BPM、Oracle BPM、jBPM、Camunda BPM 和 Activiti BPM 在英国、挪威、阿曼、马来西亚、南非、迪拜、肯尼亚和卡塔尔的现场任务中开发 BPM 流程，进行需求收集、开发、咨询、架构设计和培训。

**Niall Browne** 在 Excel 和网页技术方面有广泛的工作经验。他在都柏林城市大学学习数学和心理学，对教练和体育分析有浓厚兴趣。最近，他是 Drogheda The Mill 团队的一员，与 PFS 共同创建了一个金融支付项目。



















































































































## 前言

> *想象一个没有 Excel 的世界。对我来说那是不可能的。*
> 
> *萨蒂亚·纳德拉，微软 CEO*

我们大多数人都同意微软 CEO 萨蒂亚·纳德拉的看法。如果你把类似的产品也算进去（而谷歌没有公布 Sheets 的确切数字），那么有超过十亿人离不开 Excel 或类似的电子表格来组织我们的日常工作以及职业和个人生活。

但你是这十亿人组成的群体中的特殊一员。你是 Excel 高级用户群体中的一员，不断挑战这个工具的极限。你是同事们遇到最棘手的 Excel 问题时求助的人，而你能解决这些问题。你知道事情一定能在 Excel 中完成，即使需要深入网络的某个角落才能找到答案。你的关键知识支撑着企业的运转——所有这些都保存在可靠的 Excel 文件格式中。

如果你在读这本书，你清楚自己遇到了问题。Excel 将永远是你工作生活的核心部分——但你知道它可以变得更好。你已经记不清花多少小时去寻找别人在表格中引入的错误。你厌倦了向同事解释表格实际上是如何工作的，而且你怀疑他们没有在听，只是为了避免承担维护它们的负担。在一个 AI 无处不在的世界里，你当前的 Excel 解决方案可能需要太多的人工干预。

这本书建议在现有 Excel 知识的基础上，寻找一种更好的方式。

# 本书适合的读者

虽然会有十亿人从这本书中受益，但写作时主要考虑了三类人群：

- 已达到 Microsoft Office 365 极限并想知道下一步的 Excel 高级用户，尤其是随着 AI 工具变得越来越强大。
- 寻找工具来捕获知识并将其部署到企业级系统中的业务分析师和知识工作者。
- 正在加入使用决策模型的现有规则、AI 或工作流项目的人，因为很多工具可能已经设置好了；他们会发现 DMN 和 FEEL 的介绍特别有用。





其他人也会觉得这本书很有用，例如正在寻找预测模型规则化安全网的机器学习专家。这本书对那些希望以更结构化的方法实现 AI 和业务逻辑的开发者也很有价值，无论他们使用什么语言。

## 何时阅读本书

什么时候读？快速的答案是现在。越来越多的企业正在转向线上并采用数字工具——这还不包括 AI 对大多数办公室工作的冲击浪潮。

Excel 是这一转变的绝佳起点，但迟早，你需要通往更强大解决方案的途径。本书为你提供了一条清晰的途径。它还有一个额外的好处：以 when ... then 格式捕捉你的知识，可以将业务知识与技术实现分离，使你的投资更有可能延续到未来。

## 本书涵盖内容

第 1 章，Wrestling with Excel? You Are Not Alone，概述了你在 Excel 中可能遇到的各种问题，但向你保证你并非唯一遇到这些问题的人，并展示了基于 AI 的规则方法如何提供帮助。

第 2 章，Choosing an AI and Business Rules Engine – Why KIE and Drools?，展示了为什么 KIE 项目提供的规则引擎和 AI 工具是 Excel 用户的最佳选择，并欢迎你加入由 IBM 和 Red Hat 支持的开放社区，在其中你可以获得持续支持。

第 3 章，Your First Business Rule with the Online KIE Sandbox，深入讲解如何编写业务规则并在在线 KIE 沙盒中使用经典的 Hello World 示例创建你的第一个决策模型。

第 4 章，More Decision Models, Business Rules, and Decision Tables，扩展了你对决策模型的知识，并构建了一个可运行的巧克力店示例，以展示更多可用工具。

第 5 章，Sharing and Deploying Decision Models Using OpenShift Cloud and GitHub，探讨如何使决策模型可共享，允许其他人将其在云中运行，并使用在线工具与同事协作设计模型。

第 6 章，Calling Business Rules from Excel Using Power Query，使用 Excel 内置的 Power Query 工具将 Excel 与我们的 AI、规则和决策表连接起来。

第 7 章，Using Business Rules in Excel with Visual Basic, Script Lab, and Office Script，提供了三种基于脚本的替代方法，让你有更多选择来将 Excel 与业务规则连接。

第 8 章，AI and Decision Services within Excel and Power Automate Workflows，介绍了微软的业务自动化工具，并展示了如何使用我们的模型在流程的关键点做出决策。

第 9 章，Advanced Expressions, Decision Models, and Testing，探索了 KIE 内置表达式语言的全部功能，并介绍了场景测试工具，以便我们可以保证决策模型按预期运行。

*第 10 章，Scaling Rules in Business Central with Docker and the Cloud*，展示了 KIE 和 Kogito 决策工具的强大功能，利用容器来增加部署选项和编辑能力。

*第 11 章，Rules-Based AI and Machine Learning – Combining the Best of Both*，介绍了 AI 的另一主要部分，并展示了如何使用 KIE 和 Azure ML Studio 中的工具将机器学习和基于规则的决策模型并排部署。

*第 12 章，What Next? A Look Inside Neural Networks, Enterprise Projects, Advanced Rules, and the Rules Engine*，扩展了前几章的内容，并为未来学习提供了关键领域，以及关于神经网络、企业 Java 项目和合乎伦理、可解释 AI 的实用第一步。

*附录 A，Introduction to Visual Basic for Applications*，介绍了足够多的 **Visual Basic for Applications (VBA)**，以便那些对该语言不熟悉的人能够理解 *第 7 章* 中 VBA 示例。

*附录 B，Testing Using VS Code, Azure, and GitHub Codespaces*，介绍了微软的 **Visual Studio Code (VS Code)** 编辑器——虽然它比本书主要章节中的工具更高级，但许多人会欣赏其在 Azure 和 Codespaces 中进行测试和构建项目的强大功能。

*附录 C，Troubleshooting Docker*，支持 *第 10 章* 中给出的说明；大多数人不需要它，并且会体验到顺利的 Docker 安装——但如果需要，这里有支持是件好事。

## 如何充分利用本书

就像知道如何打开汽车的引擎盖（发动机舱盖）来更换机油和火花塞一样，能够打开 Excel 的引擎盖会给你带来更多力量。本书将逐步带你完成这一过程，并以 Excel 高级用户易于理解的方式解释事物。但请注意，你有时会弄脏手——并非一切都像微软交付 Office 应用那样整洁。

要充分利用本书，需要满足以下要求：

你应该熟悉 Excel——例如，知道什么是电子表格，如何打开文件、输入公式、保存和重命名文件等等。没有必要是高级用户（例如，使用高级公式、Excel 的 Visual Basic 或其他脚本工具），但尝试过这些工具将有助于你欣赏本书中提供的业务规则的全部威力。

你应该非常熟悉使用网络浏览器。虽然本书以不比典型旅行预订网站更复杂的方式描述 Business Central 和 Azure Web Portal，但我们关注页面而非浏览器，因此假定你具备适度的网络素养。

你应该能够将大问题分解为小问题并逐一解决。大多数 Excel 用户在编写公式时会自然地这样做，本书假定你会使用所描述的工具来解决你的问题，而不是期望得到一个完整即用的解决方案。

## 前言

对于 *第 10 章*，你应该知道如何在计算机上安装软件。提供了完整的说明来安装 Docker Desktop，这是一个方便的容器，让你确信示例在你的计算机上与书中完全一样运行。你需要下载并运行图形化安装程序并解决小问题（例如，如果需要管理员密码才能继续安装）。我们也提供了替代方案，因为并非每台计算机的设置都相同，有时事情就是不奏效。

你应该能够寻求帮助。这可能就像搜索常见的错误信息以了解其他人如何解决问题、阅读文档（提供了指针！）以编写自己的业务规则，甚至礼貌地向更技术性的同事或开源社区成员寻求支持。

虽然本书假定你没有任何编码经验，并且始终提供简单、非技术性的路线，但有（额外的）附录可以让更技术性的同事在你想探索这些领域时为你提供支持。

对于一本技术书籍，必须拥有的工具清单出奇地短，因为我们尽可能使用免费的在线工具。

事实上，仅凭网络浏览器就能从本书中获得很多价值。然而，由于 Excel（桌面版和在线版）可通过微软免费试用，我们强烈建议你安装它，因为你将学到更多——尤其是在 *第 6 章* 和 *第 7 章* 中。

| 要求 | 补充说明 |
|---|---|
| 现代网络浏览器和合理的网速互联网连接 | Chrome、Edge 和 Safari 均已确认可用，Firefox 也很可能在不久后兼容。 |
| Microsoft Excel 和 Office 365 | *第 6、7 和 8 章* 中提供了替代选项，但建议你注册 Office 365 和 Power Automate 的免费试用。 |
| 信用卡和手机 | 所用产品均为免费版本，但在注册 OpenShift 和 Azure 时可能需要信用卡和手机来验证身份。 |

最要求硬件规格的章节（在运行工具方面）是 *第 10 章*，我们使用 Docker——过去 5 年内销售的大多数笔记本电脑都能够支持它，并且提供了替代方案（例如改用云）。

**如果你使用的是本书的数字版，我们建议你自己输入代码或从本书的 GitHub 仓库（链接在下一节提供）访问代码。这样做将有助于你避免因复制和粘贴代码而产生的潜在错误。**





## 下载示例代码文件

您可以从 GitHub 下载本书的示例代码文件，地址为 https://github.com/PacktPublishing/Business-Rule-Engines-and-AI-for-Excel-Power-Users。如果代码有更新，将会在 GitHub 仓库中更新。

我们还在 https://github.com/PacktPublishing/ 上提供了来自我们丰富图书和视频目录的其他代码包。快来看看吧！

## 下载彩色图片

我们还提供了一份 PDF 文件，其中包含本书中使用的屏幕截图和图表的彩色图片。您可以在此处下载：https://packt.link/EAEVJ。

## 使用的约定

本书中使用了以下文本约定。

正文中的代码：表示正文中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号。示例：“The formula to match with the first 10,000 customers is Customer.Number < 10000.”

代码块设置如下：

```
let
    // "SourceUrl" with quotes needs to match the named range on
    our Excel sheet. You may need to change {1} to {0} depending on
    when your first line begins
    pSourceUrl = Excel.CurrentWorkbook() { [Name="SourceUrl"] }
    [Content]{1}[Column1]
    in
        pSourceUrl
```

当我们希望引起您对代码块中特定部分的注意时，相关行或项目以粗体显示：

```
let
    // "SourceUrl" with quotes needs to match the named range on
    our Excel sheet. You may need to change {1} to {0} depending on
    when your first line begins
    pSourceUrl = Excel.CurrentWorkbook() { [Name="SourceUrl"] }
    [Content]{1}[Column1]
    in
        pSourceUrl
```

任何命令行输入或输出均按如下方式书写：

```
curl -X 'POST' \
  'https://dmn-dev-sandbox-yu88rl6qu490-crt-openshift-dev.apps.sandbox.x8i5.p1.openshiftapps.com/Customer Recommendations/Product Recommendation Service' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
"Customer": {
  "Number": 0,
  "Name": "string",
  "Date of Birth": "2022-10-23",
  "Country of Residence": "string",
  "Special Requests": "diabetic",
  "Previous Orders": [
    0
  ]
}
}'
```

**粗体**：表示新术语、重要词语或您在屏幕上看到的词语。例如，菜单或对话框中的词语以**粗体**显示。示例：“Click **Done** at the bottom right of the screen to save the code and exit the screen.”

提示或重要说明
显示如下。

## 联系我们

我们非常欢迎读者的反馈。

**一般反馈**：如果您对本书的任何方面有疑问，请发送电子邮件至 `customercare@packtpub.com`，并在邮件主题中提及书名。

**勘误**：尽管我们已尽力确保内容的准确性，但难免会出现错误。如果您在本書中发现错误，我们将非常感谢您向我们报告。请访问 `www.packtpub.com/support/errata` 并填写表格。

## 下载本书的免费 PDF 副本

感谢您购买本书！

您喜欢随时随地阅读，但又无法随身携带印刷书籍吗？
您的电子书购买与您选择的设备不兼容？

不用担心，现在购买每本 Packt 书籍，您都可免费获得一份无 DRM 的 PDF 版本。

在任何地方、任何地点、在任何设备上阅读。直接从您喜爱的专业技术书籍中搜索、复制和粘贴代码到您的应用程序中。

福利还不止于此，您还可以获得每日发送到您收件箱的折扣、新闻通讯和精彩免费内容的独家访问权限

按照以下简单步骤获取福利：

- 1. 扫描二维码或访问以下链接

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_24_0.png)

```
`https://packt.link/free-ebook/9781804619544`
```

- 2. 提交您的购买证明
- 3. 就这样！我们会直接将您的免费 PDF 和其他福利发送到您的电子邮件
