## 1 什么是 ECM？

本章提供了 **Enterprise Content Management (ECM)** 的含义和历史背景。如果你刚开始接触 ECM，本章将为你提供有趣的背景，以便你更好地理解本书的其余部分以及即将到来的 ECM 体验。本章篇幅不长，我们建议你通读全文（如果你是 ECM 方面的老手，那么我们可能不必告诉你本章中哪些部分可以略读）。

当我们于 1998 年加入 FileNet 时，还没有 ECM 这样的术语。每个厂商和分析师都有自己的术语，通常是对文档管理的某种变体。如今，似乎每个经验丰富的 IT 专业人士都必须熟悉 ECM 术语。有时这种熟悉是真实的，有时只是术语的暴风雪。虽然 ECM 如今是标准的行业术语，但除非举例说明，否则它并不总是清楚其含义。本章结束时，你将能够用简单的术语解释它。

本章包含以下主题：

- ECM 的入门定义
- 使用 ECM 的一些动机性用例
- 最重要的 ECM 产品功能
- 历史和新兴的 ECM 相关标准
- 常与 ECM 混淆的事物
- FileNet 历史

### 用三句话概括 ECM

好吧，这个小标题有点 trick。我们认为我们无法用三句话解释 ECM。让我们看看是否可以用三段话来解释它。

ECM 是为组织运营提供信息访问基本控制的自动化。控制不仅仅是限制访问，尽管这是其中的重要部分。它更多的是关于在寻找、收集、存储和检索内容方面采取有组织的方法，而不管你最终选择使用哪些具体应用程序来处理这些方面。它被称为 *enterprise* content management，因为其范围不限于单个部门或分支机构。它不仅仅是一个应用程序，而是一个完整的平台，用于支持不同的应用程序、信息来源和流程。在这种情况下，*content* 指的是所谓的非结构化内容——各种各样的文档和对象，它们不具备高度结构化数据的易解析优势。非结构化内容当然也有重要的结构化数据方面，正如我们在后面的章节中会看到的那样，但重点是超越交易性业务数据。

> “企业”一词不一定意味着商业企业组织。ECM 解决方案也在地方、国家和国际政府组织以及志愿和非营利组织中得到广泛部署。这些企业共同的特点是需要管理其内容，通常是在规模上，以满足组织目标。无论这些目标被称为业务目标、合规要求还是其他什么，很明显它们在技术层面转化为相同的事情。
>
> 在本书中，我们将“企业”和“业务”术语互换使用。ECM 概念很少局限于商业实体。

![](img/2295191c450a0c86b2d82df6cec04526_28_0.png)

ECM 的愿景是利用强大的 IT 基础设施来利用组织中已在使用的内容。一旦内容处于集中控制之下，就必须使其可供各种用户——技术和非技术——使用，以满足组织的普通和特殊需求。你必须具备这幅图景的两个部分。如果内容不完整、不正确或不易获取，企业就无法可靠地使用内容。另一方面，如果业务用户无法为企业的合法需求访问内容，将内容锁定在 IT 要塞中价值有限。

多年来，ECM 的前身逐渐从完全没有，到本地团队管理内容，到部门级点解决方案，再到真正的 ECM 平台的企业级推广。ECM 的目标是为非结构化内容做与关系数据库很久以前为结构化内容所做的事情一样。

在通用数据库软件广泛使用之前，应用程序自行设计存储交易数据的方法。企业可能会开发可重用的软件组件来满足多个应用程序的需求，但这仍然使数据孤立。应用程序数据存储容器的开发并不是大多数企业的核心业务。当关系数据库出现时，很容易看出使用它们作为标准化平台组件的好处。新的应用程序、实用程序、报告等可以轻松编写，而不会对现有应用程序造成干扰，因为存储和检索方面已委托给数据库。

尽管数据库是 ECM 平台的重要组成部分，但非结构化内容存在关系数据库设计“甜点”之外的额外挑战。例如，通常存在更复杂的参照完整性约束和更细粒度的安全要求。

### 用例

为了立即感受到 ECM 可以解决的一些问题，让我们看几个典型的用例。这些只是流行的场景示例，旨在让你对 ECM 的本质有一个具体的概念。这里当然还有许多未涵盖的其他用例。

#### 中央文档存储库

如今，集中管理业务文档似乎有好处是显而易见的，但并非一直如此。早期的集中化管理系统往往也意味着放弃对文档的控制权给中央机构，而这是否是个好主意并不总是显而易见。现代 ECM 系统专注于集中技术控制（安全存储、可靠备份、高可用性等），同时将信息业务控制权保留在适当的用户手中。

在电子文档的早期，如果你需要查看某份文档的最新版本，你会找到作者并向其索取。该系统有几个弱点。如果你需要从 Floyd 那里获取文档，而 Floyd 生病了、坐飞机或只是无法联系到（Floyd 也可能只是厌倦了被询问），你就无法获取文档。如果你需要从 Floyd 那里获取文档，而 Floyd 自己也找不到他的文档或 Floyd 的硬盘损坏了，你也无法获取文档。





## 什么是 ECM？

文件共享是克服这些问题最早的技术之一。某个人，也许是 IT 人员，也许是当地懂电脑的用户，设置了一个可广泛访问的共享目录。各种用户可以通过将文档放入共享目录来“发布”文档。虽然这解决了部分原始问题，但也带来了其他问题。超出少数用户的使用后，共享目录树的组织可能会变得相当混乱。如果共享目录中存在“同一”文档的多个版本，这尤其成问题。其中一些副本可能是由于作者对文档主副本进行了修订而产生的；另一些则来自不同用户在共享目录中制作自己的安全副本。子文件夹和文件命名的约定很容易制定，但要随着时间的推移保持这些约定需要大量的用户自律。这种技术最大的问题或许是元数据（描述内容的属性）仅限于底层文件系统提供的功能，而这通常仅限于简单的创建和修改记录。

内容管理的早期技术可能缺乏精致，但其背后的动机是合理的。任何规模的组织都需要一个可靠的系统来维护“单一事实来源”。也就是说，你应该能够在需要时获取所需的信息，并且确信自己使用的是不是过时的副本、未发布的草稿或其它非官方版本。ECM 系统在两个主要方面提供帮助：

- 首先，它促进了你随时随地访问信息的技术能力
- 其次，它促进了对当前、过去和进行中版本的记账工作

当这两个主要因素得到可靠保障时，用户 readily 承认 ECM 仓库保存着内容的 master 副本。他们为自己和企业看到即时的好处，随着时间的推移，他们自愿放弃本地内容并将其移交到仓库，效益会成倍增加。

## 合规与治理

如果可以决定对组织而言哪些文档重要就好了。你会识别出它们，给它们适当的关注，并不对其他事物浪费资源。今天，很少有组织能采取这种方法。法律和法规要求企业和其他组织保留有关他们做出的（或未做出）决策的越来越多信息。即使你不受正式监管合规的约束，你可能也发现有必要对业务文档进行控制，以应对可能发生的法庭诉讼的必要性。

仅仅制定某种处理各种类型文档的最佳实践似乎就足够了。也许你还可以制定年度员工签署确认，他们已了解这些最佳实践要求。不幸的是，在大多数情况下，这种方法已不再可接受。在监管机构、审计员、法官和其他外部人士眼中，你不仅必须做适当的记录保存，还必须能够证明你已正确地做了。把事情留给个人责任听起来很棒，但这会让你承担很多风险。

合规和诉讼案件中的主要风险是你的企业行为不当。存在次要风险——你没有遵循保留企业所做之事记录的规则或最佳实践。ECM 解决方案可以通过自动完成大部分记录保存工作来降低这种次要风险。你的仓库不仅会安全保存某特定文档的主副本，还会保存该文档整个修订历史的防篡改副本。随着文档在其生命周期中经历各个阶段，安全访问可以进行调整。最后，当文档达到使用寿命终点时，它可以从仓库中自动且明确地清除。

## 以文档为中心的流程

内容仓库的早期场景之一是简化处理放入这些仓库的文档的业务流程。这听起来有点循环，但我们指的是“企业有对其运营至关重要的业务流程”。与内容片段相关的自动化处理流程，**业务流程管理（BPM）**，只有在内容本身可靠可用时才可靠。内容仓库很好地发挥了这一作用。

一个常见的例子是抵押贷款经纪人处理大量贷款申请。在典型的贷款处理场景中，在做出最终放贷决定之前，必须从不同来源收集数十份单独的文档，其中一些由申请人查看，一些纯属内部。这些文档包括房产评估、收入验证文件、产权保险单以及支持承销流程的各种内部文件。

仅仅跟踪这些文档的进出状态就已是一项艰巨的任务，其中很大一部分是传真或扫描的图像。再加上将这些文档组装成各种决策步骤的包，你就会开始看到一条非常繁忙的信息流。申请人可能随时打电话提供信息或询问状态。对于客户服务代理来说，追踪文档的实物副本完全不现实。文档使用者往往身处不同的物理位置，彼此相隔从小巷到海洋的距离。

以文档为中心的流程是行业术语，指 BPM 中与路由和处理文档有关的部分。文档通常是某种决策流程的一部分，如抵押贷款示例。在 ECM 解决方案中，文档本身存放在仓库中，而工作流系统包含指向它的链接。这保持了第一种用例中描述的主副本概念的优势，同时也允许对业务流程本身进行完全电子化和自动化处理。实际文档按需访问，工作流通常会根据决策或处理步骤的结果更新仓库中内容的属性。

## 在 ECM 平台中应寻找的内容

关于 ECM 中包含哪些具体内容以及哪些只是“其他东西”，有很多不同的看法。每个供应商都有自己的优先列表，通常基于其自身产品的优势。如果我们要选择 ECM 系统，本节列出了我们绝对会寻找的内容（请忽略我们可能有 IBM 员工折扣的事实）。除第一项外，该列表没有按重要性顺序排列。所有这些内容对任何 ECM 平台都很重要。还有数十种可提及的附加功能，对特定场景很重要。

本节可能看起来有点像推销，因为这些要点大多是 FileNet 产品的优势。我们是基于这一点来选择的吗？我们更愿意认为我们确保产品具备我们独立认为重要的功能。

ECM 领域还有其他几个参与者，按定义，它们大多是 FileNet 和 IBM 的竞争对手。我们让他们自己说话（并写自己的书）。不过，我们要指出的是，其中一些供应商提供的是点解决方案而非综合平台。点解决方案针对某个特定任务、用例或场景进行了优化，但当你的需求超出该范围时，它不一定具备所需的功能。有时答案是将一系列点解决方案组件拼接成一个星座，希望能形成一个运行良好的整体。其他供应商可能提供功能更广泛的产品，但随着越来越多应用程序的添加或组织中越来越多部门参与 ECM，其可扩展性或可用性会受损。我们现在就下台，让你自己得出结论。





## 安全存储库

首要优先事项必须是一个构建良好的存储库，不会丢失放入其中的任何内容。这似乎对于任何现代 IT 系统都是一个显而易见的要求，但仍然存在一些解决方案，其在这方面的保证需要仔细审视。为了防止信息丢失，ECM 存储库将提供诸如系统强制的引用完整性检查、完全事务性的更新，以及防止一个用户的更改意外覆盖另一个用户更改的机制。显然，我们讨论的内容远远超出硬盘盘片健康的范畴。

## 强安全性特性

对 ECM 存储库的访问必须支持与组织中其他地方使用的最佳认证机制相当的认证。同样，它必须支持在单个项目和操作类型级别的授权检查。例如，授予某人更新文档的权限，不应自动授予其删除该文档的权限。拥有删除文档的权限，不应自动意味着你有权删除其他文档。

存储库中项目的权限设置必须具有足够的粒度，以适应特殊场景，但还必须具有可行的默认机制，以便仅仅设置权限不会成为负担。ECM 系统的安全特性不仅必须将恶意用户拒之门外，还必须避免给合法用户设置障碍。

## 平台支持

人们很容易说 ECM 系统必须运行在各种硬件和软件平台上，但更现实的说法是，它必须运行在对你重要的平台上。除非你已经计划使用多种不同平台，否则对多种平台的支持只在随着时间推移基础设施演进时为你提供选择方面有直接作用。间接地，支持多种平台是帮助你判断特定供应商是否具备广泛技术专长的因素之一。

## 可扩展性

任何人都可以像敲打几块石头那样简单地构建出一个在开发环境中表现足够的解决方案。真正的考验，也是真正的需求，是它能在你预期的生产负载及更高负载下表现出色。在达到某个程度之前，你可以通过使用更快的服务器、增加内存等来增加负载。然而，必然会有那么一个临界点，“更大更快硬件”的方法将达到极限。你的 ECM 系统如何应对超越该点的情况？你将需要一种架构，在产品和你自己的部署中，能够跨多台服务器进行扩展。在理想情况下，不应有任何架构因素在实际意义上限制扩展；例如，你应该能够将服务器添加到集群中，以扩展到将来可能遇到的任意负载。

## 可扩展性与附加组件

可以肯定的是，你今天对 ECM 的需求不会与一两年后的需求相同。一个常见的现象是，组织非常喜欢初次体验 ECM 带来的好处，以至于他们希望将早期的成果扩展到越来越广泛的领域。你需要一个当你准备好发展时也能随之成长的 ECM 系统。

## 供应商与合作伙伴生态系统

选择一个来自积极培育丰富的合作伙伴和第三方生态系统的供应商的 ECM 系统。合作伙伴可以提供独特的应用程序、集成服务以及特定业务场景的专业知识。有时这是对供应商自身能力的补充，有时甚至与之竞争。对于使用 ECM 系统的人来说，拥有多种选择是值得的。

## 企业互操作性

你的 ECM 系统应与企业的其余基础设施良好互操作。这意味着诸如使用企业目录进行认证，支持你的数据中心运行方式、备份执行方式，以及高可用性和灾难恢复配置的运维方式。如果你的企业在多个地理区域拥有 IT 基础设施，ECM 系统必须充分支持分布式部署。

## 强大的 API

即使你不打算自己开发任何自定义应用程序，你也需要一个具有功能丰富、支持良好、文档完善且稳定性记录良好的 API 的 ECM 系统。你或第三方可能需要编写一些“胶水”代码来集成其他企业系统，或者你可能有一天决定用你组织特有的东西来增强开箱即用的应用程序。无论如何，强大 API 的存在至少说明了两点——供应商愿意为其产品的必要定制和集成提供机制，并且供应商明白他们正在提供一个平台。应用程序和实现的想法往往超出供应商所提供的范围。即使你从没打算直接使用它们，API 的质量也能让你了解整个 ECM 系统的本质。这也是上述供应商和合作伙伴生态系统中的一个重要因素。

## 通知与触发器

你是否希望在某些类型的文档被创建或更新时收到通知？也许在这种情况下你有想要执行的自动化后续步骤，或者你只是需要有人查看一下。如果你只有一个应用程序与内容存储库交互，那么当事情发生时，你就不需要 ECM 系统提供通知或触发器。你的应用程序只需自行关注并发出通知。随着你对 ECM 的使用增长，你将拥有许多应用程序，不断在所有这些应用程序中重建通知逻辑并不是好的设计。相反，你应该从 ECM 解决方案本身寻求这种功能，并且它应该能够根据你的具体需求进行适当的配置。

## 传统文档管理功能

你可能会希望你的 ECM 系统具备几项传统的文档管理功能。以下是其中一些。

## 版本控制与历史记录

当文档被修订时，你希望能够跟踪所做的更改。你希望能够检索过去的版本。你希望能够找出谁进行了更改、更改时间等等。保留多少过去的版本应由你决定，而且你的需求可能因文档而异或因类型而异。

## 工作流集成

有许多场景需要对文档应用工作流，可能在文档创建或更新时启动。应该很容易向你的工作流系统发出此类调用。这不仅应在功能上简单，还应具备高性能。紧密的集成有助于实现这两者。

## 搜索与导航

应该具备根据任意条件搜索内容的功能。当你已经通过某种方式定位到一个项目时，应该能够轻松导航到相关项目。“相关”的含义在不同上下文中可能不同。有时由应用程序决定，有时由个人用户决定。你不会想要一个只支持预定义关系的系统。

## 审计与报告

出于监管和其他合规原因，以及纯粹的良好管理，你可能需要能够说明谁在何时拥有何种访问权限。也许同样重要的是，你需要能够分辨出谁尝试做什么，但被安全访问检查拒绝。

## 元数据

不同的 ECM 平台对元数据有不同的术语。它指的是关于内容的累积数据——谁拥有它、最后更改时间、大小等等。你应该能够用自己的元数据扩展内置元数据，并且应该提供各种方便的数据类型（例如整数、日期、字符串）。

## ECM 与标准

多年来，人们多次尝试为 ECM 系统创建标准化接口和 API，其中一些尝试早于“ECM”这一术语。ECM 与其他计算机行业领域并无不同，标准的可用性有助于客户和独立软件供应商创建能够与多个供应商产品配合使用的应用程序和附加组件。缺点是，跨供应商标准通常只迎合一组核心的通用功能，或者（更糟的是）提供广泛的可选功能，供应商可以自由选择是否实现。当应用程序编写者想要利用并非每个受支持的 ECM 产品都提供的功能时，这可能给他们带来负担。





This is a brief survey of some of the standards you may see mentioned, but it is not meant to be exhaustive. Although having a longer list of implemented standards is generally better than having fewer, what really matters is whether an ECM solution implements standards that are important to you.

![](img/2295191c450a0c86b2d82df6cec04526_37_0.png)

> Several of these standards are of only historical and contextual interest. We haven't given web links to them, but have tried to use precise terminology and document titles so that you can readily track them down for yourself if you are interested in more detail.

All of the standards on this list were born of high hopes, but those hopes have not always borne fruit. As a disclaimer, we should mention that we personally participated in some of these efforts. As we prepared this list, it was striking to us how many different standards organizations have touched this area.

## AIIM: DMA and ODMA

Perhaps the most widely-known industry organization related to ECM is the Association for Information and Image Management (AIIM). The predecessor organization that became AIIM was founded in 1943. AIIM's mission is to promote standards, provide education, foster best practices, and generally serve as a clearinghouse for ECM-related matters. It also hosts several recurring ECM-related conferences.

Starting in the mid-1990s, AIIM served as the secretariat coordinating the development of two related standards — DMA and ODMA.

## DMA

The Document Management Alliance (DMA) was the name of a group of cooperating organizations that developed an AIIM-sponsored standard. All major players in the document management industry at the time participated to one degree or another in DMA.

The standard, released in 1997, was also named DMA and was intended to provide two major things — an architectural model for how a document management system would interact with other components, and a set of standardized interfaces. Although DMA compliance is seldom mentioned in requirements specifications these days, it continues to have influence in many current ECM products in terminology and architectural concepts.

## JCR

*Content Repository for Java Technology API* was published in 2005 as JSR 170 under Sun's Java Community Process. It is commonly referred to as Java Content Repository (JCR). A follow-on version of JCR was published in 2009 as JSR-283, *Content Repository for Java Technology API Version 2.0*. JCR is (obviously) Java-specific technology. It specifies a set of standard interfaces that a repository vendor can implement to provide JCR access to a repository.

## CMIS

A cross-vendor initiative, **Content Management Interoperability Services (CMIS)**, was announced publicly in September 2008. Many ECM solution vendors announced, then or since, intentions to implement CMIS access to their repositories. In fact, there are already many CMIS implementations available. CMIS itself underwent standardization at the **Organization for the Advancement of Structured Information Standards (OASIS)**. The final standard was ratified in May 2010, and we can expect to see several more vendor implementations (or updates).

CMIS exposes access to content repositories as a collection of related RESTful APIs and web services for common ECM needs. That makes it technology-neutral for the calling clients. It's also well-suited to a modern design paradigm of calling into an adapter layer for a specific repository. The hope is that CMIS can serve as that repository-specific adapter layer for most everyday purposes and need only be supplemented for truly unusual operations. We're far from the first to say that CMIS hopes to be for ECM what SQL is for databases.

## What ECM is not

Let's digress into a discussion of a few things that ECM is not. We're making this digression because these are common points of confusion.

## ECM is not CMS

There are a lot of software products available, both open source and commercial, that fall into the category of **Content Management Systems (CMS)**. You can find references to hundreds or thousands of them with a simple web search. There's a good chance that you are already familiar with one or more of these, and there is even a good chance that you think one or more of them is fantastic. We won't disagree with that. We've used a few open source and commercial CMS implementations, and some of them are really quite good at what they do.

## What is ECM?

CMS is not ECM, although it is not unusual for an ECM platform to have components very similar to those of a CMS. In such cases, you might be able to use your ECM platform as a CMS, but you will not generally be able to use a CMS as an ECM platform.

A typical CMS consists of a single or small collection of applications and a backing database. Almost all are self-contained systems aimed at organizations who want to have a secure and controlled process for publishing material on websites. The applications consist of web-based content editors and page layout engines. There will be built-in collaborative workflow for a content approval cycle, typically with some kind of role-based permissions system.

The most well-known examples of CMS are highly-tuned applications for managing the lifecycles of web content with a minimum of technical knowledge for the assorted writers, editors, and approvers. The difference between these CMS applications and an ECM platform is in those very words—a CMS is often more like a single application than a system of anything. There may or may not be points of integration with other applications.

## ECM is not a database

It's not a particularly unusual reaction for someone to look at all the things that make up a typical ECM platform and conclude that they could do it faster, simpler, cheaper, or better by just creating a relational database with pointers to content files in a filesystem and a few web pages to act as the frontend application. It often seems to them that much of the complexity and size of ECM is self-induced. If only things were limited to the simple things actually needed, the whole thing would be a lot smaller and tidier. Perhaps you, the reader, are thinking that very thing.

Well, why should you not do that? If you are something of an ECM visionary with enough like-minded colleagues, you very well might find success in doing it. Otherwise, you are statistically very likely to spend a lot of effort building things that are eventually replaced by an ECM system when the maintenance burden and backlog of things to do become too much to bear. If you're a stubborn person, please don't take this as a challenge! Our real aim is to get you to your goals as soon as possible without making you travel through the purifying fire of a gnarled custom implementation.

If you have no other document management system in your organization, this sort of database application will be greeted warmly. The first thing you know, someone will ask you to add a feature for keeping track of multiple versions of a document as it gets revised from time to time. Someone else will ask you for a mechanism for sharing a particular document with a specific set of people. Yet another someone will ask if they can organize the documents in something that looks like a folder structure so that they are easier to find. Someone will ask if you can automatically notify someone when certain kinds of documents are modified. Early success will lead to quite an imaginative list of features to implement. Although you may have thought of some of them at the beginning, you probably didn't have time to implement them, so they just went onto your "to do" list. Sooner or later, the same people who were slapping you on the back to congratulate you for your early efforts will be ready to wring your neck for not yet implementing the things they asked for.

This custom approach is certainly possible and relatively straightforward for almost any given single document management task. In fact, it's quite educational to do so because it can help you understand your own needs more clearly. For example, it's easy to write a web application that allows many people to upload document content for storage in a secure location and with which they can later download those documents. A single database table with only a few columns can do all of the bookkeeping of document ownership, keywords, on-disk location, and so on.

Just as most organizations would not seriously contemplate creating their own relational database system from scratch, the ECM landscape is rich enough that it seldom makes sense to build it yourself. Of course, once you have a target ECM platform, it often makes sense to develop your own custom applications on top of it, just as it is routine to implement database applications on top of an off-the-shelf DBMS.

## ECM is not source code management

It's hard to escape the parallels between some fundamental ECM platform features and those offered by **source code management (SCM)** systems. The feature of a secure, centralized repository of multiple-versioned documents comes immediately to mind, as do several others. Software development organizations certainly regard the management of source files as critically as business users regard the management of spreadsheets (in our experience, experienced professional software developers tend to regard it even more critically, perhaps because they understand the impact of making colossal mistakes).

Is it feasible to build ECM out of SCM? It's probably technically possible, assuming you select an SCM system that has the scalability, security, and other features you are looking for. It's probably also a lot of work, though.





两种系统设计中心是不同的。对于大多数组织而言，ECM 系统处理的文档量比 SCM 处理的文档量大几个数量级。SCM 系统供应商或第三方提供的工具非常适合软件开发制品的生命周期，但对于典型的业务用户来说，它们可能不存在或不合适。例如，SCM 很可能与开发人员和测试人员使用的 **集成开发环境 (IDE)** 工具具有合理的集成，但它不太可能与办公生产力应用程序有任何集成。

再次，你会发现自己从零开始创建或定制工具和功能。在 SCM 之上实现 ECM 并非不可能，但它并不是合适的工具做合适的工作。同样，在 ECM 平台之上实现 SCM 可能需要相当多的工作，因为 SCM 的许多功能都在应用程序中（或看起来像 ECM 的应用程序）。

## 一点 FileNet 历史

由于你正在阅读一本关于 FileNet 产品的书，我们认为你可能会对公司本身的一些背景感兴趣。

FileNet 成立于 1982 年，第一款产品是利用当时新出现的光盘技术的成像解决方案。由于当时行业的状况，FileNet 的首批产品比在今天的环境中想象的要更完整和自成体系。它们包括扫描仪、存储模块和用户工作站的定制硬件。它们还包括定制的操作系统软件、连接各个组件的定制网络协议，以及一个名为 **WorkFlo** 的创新应用程序，该应用程序最终演变为我们现在通用的工作流。

随着行业的发展，FileNet 的产品也在发展。虽然保持了在成像解决方案方面的行业领导地位传统，但公司进入了文档管理领域。1995 年，FileNet 收购了文档管理公司 Saros，并在 20 世纪 90 年代后期推出了 Panagon，这是一个集成产品套件，包括经典成像、文档管理和工作流。与早期相比，这些产品不依赖于专有硬件和低级软件。

2001 年，FileNet 宣布了一个名为 Brightspire 的新产品线。Brightspire 是一个全新的技术栈，其 **内容引擎 (CE)** 完全基于 Microsoft 平台和技术，尽管它也包含 Java API。几年之内，平台组件被重命名为 P8，内容相关产品被重命名为 FileNet Content Manager。CM 4.0 版本在引入 **内容引擎多平台 (CEMP)** 方面具有重要意义，CEMP 是 CE 作为基于 J2EE 的应用程序的重写。

FileNet 于 2006 年被 IBM 收购，产品线被称为 IBM FileNet Content Manager。正如你所见，经过几次演变，CM 是一个成熟的产品，其内部演进，而不是来自机会主义收购的无关部分的拼凑。

截至本文撰写时，现场的主流版本是 IBM FileNet Content Manager 4.5.1，本书其余部分基于该版本。CM 5.0 刚刚发布，我们将在后面的章节中提及其中一些功能。

## 摘要

在本章中，我们带你快速浏览了 ECM，包括消除流行的误解。我们介绍了文档管理的一些历史基础以及过去和现在的标准化工作。我们介绍了你在任何 ECM 平台中应寻找的基本功能。

从这一点开始，我们将立即进入将 FileNet 系统投入运行的实际问题。*第 2 章至第 6 章* 将指导你安装一个完整的独立 CM 系统，并概述主要应用程序。
