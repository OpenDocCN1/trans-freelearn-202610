## 10 包含和附加组件

本书的大部分内容都是关于 IBM FileNet Content Manager（CM）的特性和功能，CM 是 IBM 的一个产品，包含特定的软件组件。许多企业只许可 CM 而没有其他 FileNet 组件。同样多（可能更多）的企业将 CM 与其他组件结合使用，无论是作为不同的产品提供还是作为与 CM 集成的附加组件。

本章概述了这些其他组件中最受欢迎的组件。将本章视为某种销售推销很容易，但我们希望你不要那样看待它。我们并不是试图“追加销售”你在咖啡杯旁的三明治。这些确实是常用的配置，并且在你制定 ECM 策略时每个都值得考虑。

本章包括以下内容的讨论：

- 标准 CM 组件（随 CM 包含的内容）
- 合规管理组件（包括记录管理和电子发现）
- IBM Smart Archive Strategy
- 业务流程管理

我们还讨论了不符合这些主要类别但同样有趣的附加组件。





## 标准 CM 组件

IBM CM 4.5.1 的标准许可包含相当多的组件，我们先来了解这些组件。由于我们已在其他章节详细讨论过其中大多数组件，此处仅为完整性简要列出它们。在少数其他章节未详述的情况下，我们会给出更详细的描述。

该许可会不时修订（通常是为了调整所包含组件的版本号），并且每个产品版本发布时必定会修订，因此如有疑问，建议查看您的许可以了解具体内容。本节信息基于 2010 年 6 月发布的 CM 4.5.1 许可。

> 我们是工程师，不是律师，甚至不是软件许可方面的专家。如果您对 IBM 产品许可的具体条款有任何疑问，我们都不是合适的人选。本节信息仅基于我们自己的理解呈现。是的，我们已经从头到尾阅读了 CM 4.5.1 许可。第 13 章“支持、修复包和故障排除”中包含在线查找 CM 许可信息的方法。

其中几个组件包含受限使用许可。即使有许可限制，使用这组组件开发相当复杂的应用程序和部署也没有问题。但在规划部署到生产环境时，您应评估是否需要这些组件的完全授权、非捆绑版本所具有的功能。

### 服务器组件

某些 FileNet 组件被指定为服务器。由于“服务器”一词在计算机行业中的使用更为普遍，这一区分可能有点模糊不清。例如，可以有理由认为，在 J2EE 应用程序服务器中运行的几乎任何东西，从更普遍的意义上说都是服务器。

- **IBM FileNet Content Engine 4.5.1。** 当然，CE 是 CM 产品中的主服务器。它把“内容”置于企业内容管理（Enterprise Content Management）之中。
- **IBM FileNet Content Search Engine 4.5.1。** CSE 提供全文索引和搜索。CM 许可不包括使用 CSE，除非用于与 CE 集成。
- **IBM FileNet Process Engine 4.5.1。** PE 是 IBM FileNet 业务流程管理（BPM）产品的主服务器。CM 产品中包含 PE，以支持 CM 中可用的预定义工作流定义，如第 6 章“最终用户工具和任务”所述。CM 许可不包括在 PE 上运行其他工作流。

### 应用程序和连接器

接下来我们要看的组件组是两个相关类别的组合。我们将应用程序和连接器合并在一起，因为应用程序在与 CM 服务器组件通信时，有时可能需要连接器或适配器层。

- **FileNet Content Federation Services 4.5.1 和 IBM Content Integrator 8.5.1。** CFS 和 ICI 组件使您能够将其他 IBM ECM 存储库中的内容联合到充当中央目录的 CE 中。（我们在*第 9 章“规划你的部署”*中提到过 CFS。）IBM 存储库包括 IBM Content Manager、IBM Content Manager On Demand、IBM FileNet Image Services 和 IBM FileNet Content Services。有趣的是，还可以联合来自另一个 P8 域的内容（这种需求通常作为合并多个 P8 域时的过渡步骤出现，例如在企业并购期间）。CM 许可不包括连接到非 IBM 存储库，不过其他存储库的连接器（以及用于编写自定义连接器的 SDK）可通过单独许可获得。
- **System Dashboard for Enterprise Content Management 4.5.1。** System Dashboard 提供仪表化工具和图形应用程序，用于监控单个组件以及整个 CM 系统的运行状况和性能。
- **IBM FileNet Application Engine 4.0.2 和 Workplace XT 1.1.4。** 这些基于 Web 的应用程序可以高效地用于常见的文档管理任务，并且还展示了 CM 产品的许多功能。我们在*第 6 章“最终用户工具和任务”*中描述了许多这些功能。
- **IBM FileNet Integration for Microsoft Office 1.1 和 IBM FileNet Application Integration。** FileNet 与常见办公效率应用程序的集成长久以来以“AppInt”（即**应用程序集成**）的昵称著称。有两个不同的版本。
  - 原始包适用于 Office 2003 及之前的 Microsoft Office 版本，可从 Workplace 和 XT 下载。
  - 较新的包**IBM FileNet Integration for Microsoft Office (FIMO)** 用于 Office 2007（及更高版本，尽管 IBM 尚未认证任何更高版本）。它包括与 Office 2007 引入的 Office 功能区栏和其他 UI 面板的完整集成。可从 XT 下载，也可作为独立安装程序使用。
- **WebDAV 服务器。** CM 提供 WebDAV 服务器作为 Application Engine 和 XT 应用程序的一部分。WebDAV 是一种使用 HTTP 进行分布式编写的标准协议，在 IETF RFC-4918《*用于 Web 分布式编写和版本控制的 HTTP 扩展（WebDAV）*》及相关文档中定义。这意味着，当文档驻留在存储库中时，应用程序可以使用 WebDAV 协议执行简单的文档管理任务。Microsoft Windows 资源管理器、Internet Explorer、较新版本的 Office 以及多个非 Microsoft 的编写工具都内置了 WebDAV 客户端。如果可以说 CM 的 WebDAV 支持任何符合 WebDAV 的客户端就好了，但 WebDAV 历来存在客户端实现相互不兼容的问题。如果您计划使用 WebDAV，请务必确认 CM 支持您的特定客户端和版本。另外值得注意的是，与其他集成技术相比，大多数客户端通过 WebDAV 只能执行相当基本的文档管理任务。
- **IBM FileNet Services for Lotus Quickr 1.1.0。** 该组件是一个集成层，使 Lotus Quickr 系列应用能够无缝使用 CM 作为 Quickr 文档和其他工件的存储库。Lotus Quickr 提供丰富且通用的协作环境，该集成层使 CM 能够用作 Quickr 文档内容的存储库。
- **IBM FileNet Connector for SharePoint Web Parts V2.2。** CM 的 Web Parts 连接器是一个集成到 Microsoft SharePoint 安装中的组件，提供对 CM 功能的访问。在 SharePoint 中，您可以创建文档、浏览现有文档，并对存储在 CM 存储库中的内容执行其他文档管理任务。该连接器还允许用户无需离开 SharePoint 环境即可直接启动工作流并查看工作流收件箱。原本驻留在部门 SharePoint 存储库中的内容可以迁移到 CM 存储库，以利用归档和合规工具。

### 环境组件

在本节中，我们将列出捆绑的环境组件。环境组件是安装 CM 组件所依赖的基础设施。

- **WebSphere Application Server 6.1 和 7.0 for FileNet Content Manager 4.5.1。** 这提供了用于托管 CM 组件的 J2EE 应用程序服务器。这是 WAS Base 版，不包含 WAS Network Deployment。CM 许可限制此 WAS 组件用于非 CM 用途。包含 WAS 6.1 和 7.0。除非您有使用 WAS 6.1 的特殊原因，否则我们建议您使用 WAS 7.0。
- **DB2 Workgroup Server Edition 9.5 for FileNet Content Manager 4.5.1。** 这提供了用于存储 CE 对象存储和 PE 工作流工件的 CM 存储库数据的数据库服务器。CM 许可限制此 DB2 组件用于非 CM 用途。

### 举措与场景

参与 ECM 的动机有很多，通往满足企业需求的战略和解决方案的途径也有很多。在接下来的小节中，我们将考虑其中一些场景以及这些场景中合理的组成部分。在大多数情况下，这些场景代表着 IBM 根据观察到的客户需求制定的具体举措，但这几乎不是重点。您需要关注的是这些场景是否适用于您的企业，以及您是否需要制定战略来满足这些领域的需求。

就软件组件而言，许多技术产品组件都可以在多种场景中很好地发挥作用。在这些情况下，我们不再重复描述，而是引用前面的描述。虽然我们会酌情提及，但我们尽量避免将讨论集中在特定的 IBM 产品捆绑包上。这些捆绑包不仅会不时变化，因此当您阅读本书时可能已经不适用，而且我们还想专注于构成这些场景的功能领域。然后您可以选择如何开发您的解决方案。我们相信，您当地的 IBM 客户团队将很乐意与您合作，以最适合您的方式打包这些组件。

> 许多 IBM ECM 产品在一年左右的时间里采用了 IBM InfoSphere 品牌。其中大部分品牌标识已被撤销，“InfoSphere”一词已从产品名称中移除，不过 IBM InfoSphere 在其他产品上仍然是一个充满活力的品牌。您偶尔会看到这些 ECM 产品仍以 IBM InfoSphere 命名。如有疑问，请访问 IBM 网站上的相应产品页面，修改过名称的产品通常会在括号中提及之前的名称。

#### 合规管理

从几年前开始，由于对合规性的担忧，人们对 ECM 的兴趣大幅上升。虽然合规问题传统上仅限于政府机构和少数受监管的业务领域，但美国 2002 年通过的《萨班斯-奥克斯利法案》（以及许多其他国家在相同时间或稍后通过的类似法案）极大地扩展了合规要求的范围，使其基本上覆盖所有上市公司。





## 包含组件与附加组件

合规措施形式多样。在 ECM（企业内容管理）术语中，它通常归结为受某些法律、法规或公认行业标准强制要求的可靠记录保存。IBM 拥有多款 ECM 产品来应对合规的各个方面。

### IBM Enterprise Records (IER)

**IBM Enterprise Records (IER)** 前身为 FileNet Records Manager，与 IT 实践中的**记录管理**密切相关。记录管理听起来像是一个相当平淡的领域，但它实际上是当今 IT 中最严谨、要求最高的领域之一。记录管理的一个主要标准是美国国防部（DoD）标准 5015.02《电子记录管理软件应用设计标准》。尽管这是一项美国军事标准，但许多非军事和非美国政府组织也要求使用该标准。它还为许多非政府组织提供指导。IER 已通过 DoD 5015.02 v3 以及澳大利亚主要标准**维多利亚电子记录策略（VERS）**的认证。还有其他流行的记录管理标准或最佳实践，但并非所有此类标准都有针对软件供应商的正式认证流程。

记录管理软件执行多种功能，但总体而言，这些功能可以概括为以下特征。更详细的内容可在 IER 产品文档和 IBM Redbooks 出版物 *Understanding IBM FileNet Records Manager* 中获取，该出版物可在 **http://www.redbooks.ibm.com/abstracts/sg247623.html** 查阅。

### 声明

将某物**声明**为记录，即正式将其置于记录管理控制之下。无论特定文档对企业有多么重要，声明步骤都是该文档在记录管理意义上成为永久业务记录的节点。

IER 在声明方面有几个有趣的特点。

借助 IBM 所称的 ZeroClick 技术，可以自动完成全部或大部分实际声明。消除人为操作和决策可使记录管理流程更加稳健，同时减轻用户负担。ZeroClick 通过多种机制的组合来实现。在某些情况下，IBM 提供的应用程序（例如 IBM Content Collector）明确感知记录管理，并在其自身逻辑的适当节点透明地执行声明。在其他情况下，CE 事件系统可用于响应文档更新而执行声明。最后，记录声明可以绑定到 PE 管理的工作流中。在所有这些场景中，都有灵活的配置和基于规则的处理，以便您可以根据自己的场景自动声明合适的记录。当然，手动声明记录也始终可用。

与 CM 配合使用的 IER 并不仅限于对直接存储在 CM 存储库中的文档进行记录管理。借助 CM 内置的联合服务，IER 可以管理存储在许多其他类型存储库中的记录，包括来自 IBM 和其他供应商的存储库（非 IBM 存储库需要额外的连接器许可）。

IER 甚至不限于电子文档的记录管理。它还具有管理实物对象的功能：纸质文档、录像带，甚至一盒饼干。（我们不太确定饼干盒的情况，但我们认为当最后一块饼干失踪时，IER 的审计和报告功能在某些家庭场景中会非常有用。）对实物对象的记录管理还必须涉及非技术性的政策执行，因为软件系统无法实际感知实物是否被更改或移动。

### 分类

记录根据**文件计划**进行组织。在 IER 中，文件计划通过创建记录信息对象来实现。这些对象包含一条或多条记录的安全、处置和其他信息。因此，记录管理意义上的文档分类通常更紧密地与文档类型或负责该文档的组织相关联，而不是与文档的实际内容相关联。IER 与其他 IBM 工具协同工作，在自动声明的同时进行自动分类。

### 保护

当文档被声明为记录后，它将不再由作者或其他先前授权的用户直接控制。该记录完全由记录管理系统控制，并由记录管理员管理。在 IER 中，这种控制通过文件计划中的记录信息对象来实现。对于驻留在 CM 中的记录，使用了一种称为“完全代理”的特殊形式的 CE 安全继承。（我们在第 8 章讨论安全继承时描述了一般的代理对象，特别是完全代理。）对于驻留在联合存储库中的记录，具体细节有所不同，但在所有情况下，记录管理员都控制对记录的安全访问。

### 处置

记录管理保护的一个关键方面是，除非通过**处置**流程，否则记录不能被删除。考虑到正式记录管理的初衷，这很容易理解为一个关键要求。如果欺诈者可以简单地删除罪证文件，那么发现欺诈就毫无用处了。

相反，企业通常希望在记录不再具有商业价值时（并且假定已达到监管保留期限）尽快将其删除。IER 包含一项处置功能，当策略驱动的保留期限届满时，可以自动删除记录。保留期限通常依据某个相关日期或事件来定义。例如“创建后五年”和“最后更新后三年”。

> 一个希望通过按计划细致地处置记录来降低诉讼风险的组织，不一定是试图掩盖不当行为。即使是完全无辜的一方，也可能在为自己辩护以应对无端指控时承担巨额费用。脱离上下文的文档可能会被误解或令人难堪。仅仅因为文档对企业不再具有商业价值，并不意味着它对诉讼调查中的人没有价值。

尽管我们到目前为止将处置描述为删除记录，但还有其他几种可能性。记录可能会被提交给记录管理员审查并采取适当行动，可能会从存储库中导出到某种存档存储中，或者可能会受到 IER 中其他可用的处置操作之一的影响。

一个有趣的删除特例是，某些电子记录需要将其内容从系统中彻底清除。这通常涉及用预定的位模式对物理存储进行多次覆盖写入，以便即使对介质进行取证分析也无法恢复内容。

正如企业渴望尽快处置记录以降低成本并限制监管或诉讼风险一样，法律系统也认识到必须保存潜在证据。这同样出于严格的内部原因，例如保存质量控制样本。我们将在下面描述电子发现时进一步讨论这一主题。现在，只需说明 IER 具有为记录标记保留（hold）的功能即可。带有一个或多个保留标记的记录无法从系统中删除，无论是自动还是手动删除。必须先清除保留标记。

> 一些记录管理系统使用术语“冻结（freeze）”来指代我们刚才描述的“保留（hold）”过程。这不应与 CE 中同名的功能（该功能阻止对文档版本属性的所有应用程序更新）混淆。它们是无关的。





## 审计与报告

您希望在记录管理系统中进行审计，原因有两个，它们相关到您可能认为它们是同一件事。首先，出于您自身内部或监管方面的原因，您希望知道您的记录管理解决方案是否按您认为应该的方式运行。其次，如果您在监管程序或法律纠纷中必须提供文件（或声称不存在文件），当您可靠地展示出健全的记录管理实践时，您证据的固有可信度就会得到提升。

例如，假设有人质疑您关于某批记录是在常规记录管理保留政策下销毁（而不是作为世界统治的险恶阴谋的一部分）的说法。有人可能会对您关于保留政策的说法提出质疑，但如果您有记录管理审计记录显示您在一段时间内定期销毁此类记录，那么该说法显然是合法的。

IER 审计是在 CE 审计日志功能之上实现并增强该功能，但 IER 提供了工具和针对常见记录管理场景的预定义配置集。这些可以根据您的具体情况进行定制。

IER 提供的工具之一是记录管理活动的报告功能。该工具提供了多个预定义的报告模板。您可以自定义这些模板，甚至创建全新的报告。

## IBM eDiscovery

**电子取证**（eDiscovery）是在法庭案件、监管程序或类似事项中搜索与之相关的电子记录的法律流程。似乎逻辑上，通过电子信息进行搜索应该比通过文件柜和纸质文件箱进行等效搜索更快、更便宜——更好。

直到您考虑到近年来电子信息量的增长时，似乎才合乎逻辑。贵企业在一天、一个月或一年内会产生多少电子邮件？嗯，我们想也是：这是一大堆。您是否有合规原因需要保留所有这些电子邮件，至少对某些人员保留几年？

不要误会：电子取证仍然是一个昂贵的过程。法院都知道电子信息，并假定您可以有效地管理它并按需提供相关项目。您不能再声称某物“只是电子邮件”，因此您不再拥有副本。电子邮件只是拼图中的一块，您可能还有大量其他电子信息，无论是客户联系记录、员工访谈记录、工程变更记录，还是与您的业务相关的任何内容。

## 包含组件和附加组件

IBM **eDiscovery（eD）** 是一套应用程序套件，可帮助您高效管理 eDiscovery 流程。它可以与 IER 结合使用，也可以单独使用。它可用于内部发现和审计目的，也可用于响应实际的外部强制性法律发现。

eD 的效率体现在时间和成本上。

外部发现指令通常附带严格的时间表，而在这些截止日期内提供完整信息的能力会对您是否被认为具有响应性产生重大影响。您可以疯狂地让所有可用员工去寻找特定信息，但这本身就是一项昂贵的任务。大型企业在任何时候都可能同时有数个甚至数十个处于不同进展阶段的法律案件。在进行一两次 *临时* 发现工作后，您肯定会想要一个更有组织的解决方案。

发现旨在找到与法律案件相关的内部信息。没有组织希望提供超出法律要求的更多信息。宝贵的业务信息有可能流出企业，还有通过发现找到的信息扩大诉讼的风险。限制无关材料的重要性与提供相关材料一样重要。发现结果提供的材料通常由内部法务人员审查，甚至可能由外部律师审查，所有这些都会增加成本。

### IBM eDiscovery Analyzer（eDA）

eDA 顾名思义，是一种分析工具。什么与特定发现请求相关？发现指令会说明发现的主题事项，但它可能不会使用您在寻找相关材料时可能使用的相同术语。通常需要迭代过程来确定正确的搜索词、涉及人员的名单等，然后才能得出可以可靠用于真正响应发现，同时排除无关材料的方案。eDA 是一种图形化工具，可帮助有经验的人员执行该迭代过程。通过精确锁定响应文件并排除无关文件，您可以大大降低文件审查的成本。

### IBM eDiscovery Manager（eDM）

eDM 是一种用于管理实际用于发现的信息的工具。每个单独的发现流程在 eDM 中作为发现案件进行管理。使用的搜索词、手动包含和排除——所有与发现文件实际生产相关的信息类型——都得到管理。这在发现工作彻底性受到质疑时很重要。如果您可以提供实际使用的搜索，并且它们被认为合理，并且，如果您能够以可重复的方式证明这些搜索产生的信息就是您提供的信息，那么来自质疑的风险就会大大降低。

eDM 的另一个重要功能是它会为文件生成自己的法律保留。这在概念上与 IER 使用的保留类似，但 eDM 可以在没有 IER 的情况下执行保留（使用的 CE 机制相关但不相同）。具有法律保留的文件在保留在发现案件中清除之前不能从存储库中删除。如果有来自不同发现案件的多个保留，则必须全部清除后文件才能被删除。

## IBM Content Collector

如果所有内容都是由了解 ECM 存储库及其相关功能的应用程序创建的，一切就会变得简单。至少对 ECM 系统来说是如此，但对用户来说会复杂得多。大量有趣的内容是由其他应用程序创建的，通常直接存储在文件系统或各种类型的非 CM 存储库中的文件里。

IBM Content Collector（ICC）提供了一个灵活、基于规则的应用程序，用于查找这些内容并将其存储在 ECM 存储库中。ICC 可以在许多位置找到内容，并且可以在向 ECM 存储库摄取的途中以及作为摄取的一部分执行可配置的一组处理步骤。除了下面描述的配置中固有的三个源连接器之外，还有 IBM 和第三方提供的其他源连接器，并且 ICC 具有用于编写自定义源连接器的 API。

> ICC 可以被视为旧产品的演进和替代品（FileNet Email Manager 和 IBM CommonStore）执行类似功能，尽管在技术上（以及许可和支持方面）它并不是这两个产品的完全替代品。

### ICC for File Systems

ICC for File Systems 在某种程度上类似于大多数互联网搜索引擎使用的网络蜘蛛。配置好的目录列表会定期扫描以查找新内容。当业务内容存储在常见的共享文件系统中时，这是一个很受欢迎的选择。集中运行的 ICC 进程可以高效地查找内容，即使用户工作站已关闭或不可用。

### ICC for Microsoft SharePoint

ICC for Microsoft SharePoint 扫描 SharePoint 存储库并将选定内容移入 CM 存储库。原始 SharePoint 内容可以被删除、保持原样，或被指向 CM 内容副本的链接替换。当您可以编写规则描述要自动移动的内容时，这是一个不错的解决方案。由于大多数组织使用 SharePoint 的方式非正式且协作性强，您是否可以在组织中这样做取决于很多因素。您应考虑将其与上面提到的 IBM FileNet Connector for SharePoint Web Parts 结合使用，以进行手动选择的迁移。

### ICC for Email

ICC for Email，可用于 Microsoft Exchange 环境或 Lotus Domino 环境，用于归档电子邮件消息和附件。它可以配置为在电子邮件服务器上的主副本上操作，或扫描单个用户邮箱。集中式服务器机制更稳健，因为用户在扫描过程看到它们之前没有机会删除消息。ICC 知道如何找到基本消息及其附件，并在将其存储到存储库时将其视为单独但相关的对象。即使消息已移至存储库，用户仍可通过留下的存根链接访问它。

### 任务连接器

对于上述任何配置，都可以启用处理步骤以执行各种有用任务，ICC 将其称为任务连接器。提供 API 用于编写自定义任务连接器。ICC 任务连接器可与其他 ECM 产品配合使用，以实现自动分类、ZeroClick 记录声明以及我们最喜欢的一项：重复数据删除。




ICC的**重复数据删除**功能可检测到正在摄取相同内容副本的情况。它会自动将多个副本缩减为单个实例。这在电子邮件归档中尤其有价值，因为电子邮件附件存在大量重复。如果您转发带附件的邮件，转发副本将被电子邮件系统视为新邮件。附件（可能占邮件大小的绝大部分）与您收到的副本完全相同。ICC可以检测重复附件并只存储一次。最棒的是，它可以检测到电子邮件附件与之前在某文件系统、SharePoint存储库或其他来源中找到的文档相同。在许多归档使用场景中，内容量很大，重复数据删除可以节省大量硬件和运营成本。

> 重复数据删除的其他名称包括**单实例存储**和**重复抑制**。在CM系统的架构中，其他位置也可以使用重复数据删除。CE服务器可以在单个存储区域内执行重复数据删除，但该功能不适用于固定内容设备。另一方面，CM支持的许多固定内容设备都有自己的本机重复数据删除功能。

## IBM 分类模块

在许多情况下，非结构化内容必须以各种方式进行分类或归类。也许应该以某种方式组织到文件夹中，或者应该填写某些关键字属性。从合规性的角度来看，必须决定特定文档是否满足归档标准。即使没有合规性动机，您的环境中也可能存在许多几乎没有或根本没有业务价值的文档。完全依赖用户来做这些决定成本高昂，而且可能导致不一致，因此自动化帮助是值得欢迎的。

**IBM 分类模块（ICM）** 结合多种技术来做出分类决策。虽然我们在这里是在 ECM 存储库中的内容背景下讨论它，但它也可以作为独立系统运行。它可以对新内容在被摄取到 CM 存储库时进行处理，也可以处理现有内容。

如果您的数据具有一定的结构（例如，纯内部电子邮件与外部电子邮件），ICM 可以据此进行处理。在可以编写业务规则的情况下，这通常是最准确的分类工具，可以节省用户大量精力。规则可以基于关键字、邻近度和模式匹配。

![](img/2295191c450a0c86b2d82df6cec04526_325_0.png)

## 摄取

在整个企业中查找内容不仅仅意味着彻底搜索您能找到的所有硬盘驱动器并复制其中的所有内容。这样做的成本太高，并且会导致您归档对您没有多少价值的内容。我们在本章前面介绍的组件可以帮助您有效地分析和组织内容，同时将其置于企业控制之下。一个好的策略是分析内容、组织内容、剔除重复或不相关的材料，然后以在您继续使用内容时有用的方式存储它。

## 基础设施

在规划实际存储基础设施时，需要考虑很多因素。您需要记录管理、保留管理、高可用性、灾难恢复、世代存储等吗？

在硬件领域，IBM 组件可以与您已有的或计划购买的第三方存储设备配合使用。如果您正在寻找一个自包含、低麻烦的解决方案，IBM Information Archive 就是这样一个解决方案。它作为一种设备，用于存储结构化和非结构化内容，因此您可以很快地在您的设施中启动并运行它。IBM Information Archive 可提供高达 600 多 TB 原始容量的配置。它可以通过用于 IBM Information Archive 的 IBM FileNet P8 Connector 连接到 IBM System Storage Archive Manager (SSAM)，从而用作 CM 的存储解决方案。

IBM 智能归档策略还包括支持基于云的存储的组件，无论是在您自己私有运行的云中还是在 IBM 的云服务中。

## 管理

一旦您的信息存储在安全的基础设施上，出于运营和合规性的原因，您仍然必须安全、灵活地管理它。您希望能够在使用信息时立即获得它，并对其完整性充满信心。本章描述的众多 ECM 组件可以成为您的存储和归档解决方案的组成部分。

## IBM FileNet 业务流程管理器

本书讨论的是 ECM 的内容管理方面，但另一个非常重要的领域是业务流程管理（BPM）。人们过去常谈论自动化工作流，这本身就是一个巨大的成本节约者。BPM 已经远远超出了工作流的范畴，并影响和左右着企业实际完成其使命的方式。通常的重点仍然是成本效率，但也有其他衡量成功的标准，例如服务速度和最终结果的准确性。

在本节中，我们将描述一些通常与 CM 结合使用但更倾向于 BPM 领域的组件。

我们在其他章节中展示了 IBM FileNet BPM 的某些方面。特别是，CM 随附了 Process Engine (PE) 服务器组件以及两个用于文档批准路由的预定义工作流。完整的 BPM 系统还拥有更多功能。BPM 与 CM 双向集成。工作流步骤不仅可以更新 CM 存储库中的内容，还可以使用 CM 事件框架根据内容更新自动启动工作流。

> 除了 IBM FileNet BPM 之外，IBM 还提供其他 BPM 产品：WebSphere Process Server 和 WebSphere Lombardi Edition（以及各自的相關产品）。这三款产品各有优势，适用于特定场景。就我们这里的讨论而言，我们只指出，截至 CM 4.5.1，只有 IBM FileNet BPM 具有上述开箱即用的技术集成。与其他任一 BPM 引擎的集成是可能的，但需要更多的集成工作。

![](img/2295191c450a0c86b2d82df6cec04526_328_0.png)

## IBM FileNet 业务流程框架

**业务流程框架（BPF）** 是一个用于快速开发和部署适用于各种 BPM 场景的最终用户应用程序的框架。它具有基于 Web 的用户界面，可通过配置进行高度自定义，以满足许多企业的需求。它具有可重用和可自定义的组件，有助于轻松显示工作流收件箱、内容附件和其他业务工件。它特别适合基本案例管理应用程序（索赔处理、客户联系记录等）。它甚至包括与 FileNet eForms 的紧密集成，以提供美观且功能强大的用户体验。

## IBM 企业内容管理 Widgets

富互联网应用程序的流行是理所当然的。它们提供了在 Web 环境中构建有吸引力且响应迅速的用户界面的工具。IBM ECM Widgets 产品是一组可重用的用户界面组件，针对各种 BPM 和 CM 功能进行了调整。ECM Widgets 不仅仅是孤立交互的小窗口。它们可以在无需编码的情况下组合在一起，彼此交互，并与兼容的 IBM 和第三方 widget 技术交互。这种组合过程称为连接（wiring），允许快速应用程序开发和部署。甚至非 IT 人员也可以快速为专门任务创建自定义的 widget 页面，甚至可能只为一个人使用。

BPM 还包括由 WebSphere 提供支持的 Business Space，以及以 InfoSphere MashupHub 作为基于 widget 的应用程序基础设施的 IBM Mashup Center。

## 其他组件

这最后一节简要描述了一些未包含在上述章节中的组件。正如俗话所说，它们虽然列在最后，但并非最不重要。

## IBM FileNet 系统监视器

之前，我们描述了 IBM System Dashboard for ECM (SD)，这是 CM 附带的一个应用程序，用于提供有关正在运行的 CM 系统的运行状况和性能的信息。IBM FileNet System Monitor (FSM) 是一个功能完备的系统管理组件。它提供 ECM 组件的实时监控，并具有内置的警报和自动化功能。它包含用于生成各种历史系统运行状况数据报告的功能，并具有一个集成的知识库，其中包含常见异常和纠正措施。知识库可以在本地扩展，以捕获针对您的特定部署详细信息。对于数十种常见问题和维护活动，FSM 提供自动化以节省时间并减少错误。FSM 可以用作独立工具，也可以与几种流行的企业系统管理解决方案之一集成。

## IBM FileNet 影像服务

您可能还记得第 1 章“什么是 ECM？”中的微型历史课，FileNet 一开始是创建光学影像解决方案的产品。影像领域仍然强劲，并且仍然是控制传真和扫描纸质文档的一种极受欢迎的方式。IBM FileNet Image Services (IS) 通常与 IBM FileNet Capture 产品和 BPM 结合使用，是一种高容量且强大的影像解决方案。




For a couple of years, while awaiting OASIS ratification of the CMIS standard, IBM has had a technology preview of a CMIS connector for CM available for download. That CMIS connector is a web servlet listening for requests and translating them into native CE API calls. The productized CMIS connector was shipped in the last quarter of 2010 and is bundled with CM 5.0. It does not support earlier releases of CM.

## Darwin Information Typing Architecture

**Darwin Information Typing Architecture (DITA)** is a system for creating documents in a modular way. The concept is to divide large pieces of content into reusable topics, generally of about only a paragraph or two. Topic titles, keywords, and other structured data are managed with mechanisms that let you combine topics in a variety of different ways without having to copy and modify that content.

DITA was originally created by IBM but is now controlled by an OASIS technical committee. It had its origins in technical documentation—product manuals, programmer documentation, and so on—where the opportunities for content reuse were plentiful. It has seen great success in that area but is also usable for other types of documents. Even where content reuse is not very high, DITA structure can help with organization of complex documents.

There are DITA features in CM that are an expansion of the basic CE compound documents feature. The CE can be used to assemble and render finished documents via the DITA Publishing feature and integration with the popular DITA Open Toolkit. Third party authoring tools, including Quark XML Author for Microsoft Word, can be used to create and edit DITA documents in CM.

## IBM Content Analytics

For decades, sophisticated tools have been available from many vendors for performing *ad hoc* analysis of highly structured database content. IBM Content Analytics (ICA) provides an analogous capability for unstructured content. It provides a graphical user interface for rapidly and flexibly exploring relationships, trends, and gaps in information stored in unstructured data. The intelligence behind ICA recognizes conceptual information through sophisticated textual analysis. It does not merely rely on simplistic keyword matching.

An IBM Redbooks publication, *IBM Content Analytics: Discovering Actionable Insight from Your Content*, goes into great detail about how to set up and operate an ICA environment. You can find it at http://www.redbooks.ibm.com/abstracts/sg247877.html or by searching on the IBM Redbooks website.
