## 5 最佳实践——值得信赖的计算

今天，计算机对个人和组织非常重要且有用。在组织的情况下，信息需要在准确、可靠和受控的方式下流动。任何企业的成功都取决于用于业务运营控制、管理和战略规划的信息的可靠性、可用性和安全性。由于信息需要在安全和私密的方式下可用，ERP 系统的益处只有在信息在需要时以准确、安全和受控的方式提供给客户时才能充分释放。这些特征将使 ERP 成为值得信赖的系统。

Microsoft 将这些品质定义如下：

**可用性**：*我们的产品应在客户需要时始终可用。系统停机应成为过去，因为软件架构支持冗余和自动恢复。自我管理应允许在几乎所有情况下无需用户干预即可恢复服务。*

**安全性**：*我们软件和服务代表客户存储的数据应受到保护，不受伤害，并且仅以适当的方式使用或修改。安全模型应易于开发人员理解并构建到其应用程序中。*

**隐私**：*用户应控制其数据的使用方式。信息使用的政策应让用户清楚。用户应控制何时以及是否接收信息以便最好地利用其时间。用户应能够轻松指定其信息的使用方式，包括控制其发送的电子邮件的使用。*

> — 摘自比尔·盖茨关于可信计算的电子邮件，2002 年 1 月 5 日。

Microsoft 在可信度方面增加了另一个维度，那就是业务诚信。这意味着软件提供商应在与客户的交往中保持响应和透明。这超出了本书的范围。

### 什么是可信计算？

我们已经讨论了可信计算体验的要素。现在我们将从工程角度更详细地讨论可信计算的核心要素；即如何实现可信性。

### 安全

可信计算的主要组成部分是安全，这意味着——使 IT 应用本质上更安全。可以通过以下策略实现安全：

- 使应用程序开发生命周期符合 Microsoft 的安全开发生命周期。
- 从安全角度进行更好的代码分析。
- 通过以下方式缓解威胁和漏洞：
  - 采用最佳的威胁保护、检测和清除机制。
  - 隔离和弹性；即隔离易受攻击的代码并对相互交互的各种计算机进程提供有效控制。
- 强身份验证机制以确保正确的谁、何事、何时信息：
  - 谁：要知道登录用户是谁，以便确定用户的权限，并在需要时在以后阶段进行追踪。
  - 何事：要知道用户执行的操作以及用户可用的权限，以便对系统资产（如数据表）做出访问或拒绝决策。
  - 何时：要知道事件发生的时间，以便对任何安全攻击进行分析以检测、预防和消除安全影响。
- 授权：要知道特定用户组可用的权限类型（只读、读写等）。
- 访问控制：访问控制列表（ACL）应在开始时设计，以便为系统用户分配适当的权限。ACL 不应仅限于文件或文件夹，还应适用于注册表设置、数据库文件、打印机和活动目录中的对象等。关于 ACL 的其他提示如下：
  - 为保护应用程序的敏感文件和文件夹，请在安装期间以编程方式创建 ACL。
  - 明确使用并经常使用拒绝访问控制条目（ACE）。
  - NULL 自主访问控制列表（DACL）可能成为黑客的通道。例如，黑客可以将 NULL DACL 更改为“每个人拒绝访问”，因此请勿使用 NULL DACL 条目。
- 教育和意识是确保以最安全的方式使用或开发 IT 应用的关键策略。以下可用于提高安全意识：
  - 提供 ERP 应用的安全加固指南，以便以最安全的方式实施 ERP。
  - 为所有相关人员提供安全培训，如程序员、测试员、项目经理和 IT 管理员。

### 隐私

隐私的核心要素是用户应感到其信息是保密的。他们不会被迫分享其信息，也不会在未经同意的情况下被获取信息。为满足这一特性，建议遵循以下准则：

- 功能设计以实现隐私，例如：
  - 支持客户控制其个人信息的权利的功能和特性。
  - 在提供个人信息时为客户提供选择加入或选择退出的选项的功能。
- 在设计解决方案时将隐私作为关注点。
- 在开发和部署中遵守 Microsoft 隐私政策。

### 可靠性

可靠性没有衡量标准；不同的人对可靠性有不同的感知。非 IT 专业人员可能将可靠系统定义为“以预期和一致的方式持续运行的系统”。而 IT 专业人员可能将可靠系统定义为“一个可靠的系统，需要最少的维护和中断，且整体系统具有高可用性”。定义上的差异为制定其衡量单位造成了主要障碍。Microsoft 认为可靠性具有以下五个特征：

- 可预测：提供一致的服务。
- 可维护：易于配置和管理。
- 有弹性：尽管条件发生变化仍能工作。
- 可恢复：可在无不利影响的情况下轻松恢复。
- 已验证：在多种环境中运行。

上述品质可通过多点策略实现，该策略不仅确保更高的可靠性，还能持续改进：

- 设计以提高可靠性，例如互操作设计、失效安全设计等。
- 工程以提供更多可靠性，例如代码分析以实现更优的编码逻辑并消除内存泄漏
- 用于了解系统在哪里失败或客户希望如何使用系统等功能。Windows 错误报告就是一个例子

### Microsoft 安全开发生命周期

为了以值得信赖的方式开发、部署和使用软件应用程序，Microsoft 推荐安全开发生命周期。该生命周期基于 SD3+C 方法论。

### SD3+C 方法论

Microsoft 开发了一种方法来应对现实世界的安全问题。它包含四个支柱：

- 安全设计：任何基于 Dynamics AX 的 ERP 解决方案都应以能够保护自身以及正在处理的信息的方式进行设计、架构和实施。以下是使设计安全的几点：
  - ERP 解决方案应设计为仅接受预期输入的用户输入。
  - 危险 API 应以安全的方式处理（如 Microsoft 规定）。
  - 数据或控制命令应在信任边界处进行验证。
  - 整体架构应安全，这可通过在设计阶段进行威胁建模来实现。




- 应使用规定的或安全的设计模式以确保安全编码。
- 应使用加密技术以最大限度降低信息泄露风险。
- 应使用 Dynamics AX 代码访问安全机制，防止不受信任的代码（即未源自 AOT 的代码）调用任何危险的 API。
- 应在每个 API 上应用严格的权限。
- 应通过使用威胁建模的结论来降低漏洞。
- 应遵循与数据授权相关的最佳实践。

- 默认安全：即使 IT 应用以千种方式得到保护，但存在一个安全漏洞，现实世界的软件应用也无法完全安全。该安全漏洞可能被恶意用户利用。一些最佳实践可以在恶意攻击发生时确保最小化损害：
  - ERP 系统或任何其他应用程序应以最少的必要权限运行。
  - 未使用或很少使用的服务应关闭。
  - 使用不广泛的功能应默认禁用。
- 部署中的安全：部署安全有两种方式：即在部署期间应加强安全，并且部署程序应安全可靠以应用补丁。
- 沟通：如果发现任何安全漏洞，保护性措施或变通方法应以清晰且负责任的方式传达给最终用户或管理员。在与最终用户或管理员沟通时，应建议纠正措施，例如应用补丁、部署变通方法以及配置设置更改。

## 安全开发生命周期

可信安全开发生命周期源自瀑布模型。它包含六个阶段。

## 需求阶段

许多人误以为对已开发应用的审查足以确保软件应用的可信度。审查软件应用不仅成本低效，而且无效。在审查已开发应用时在设计中发现的安全问题可能会迫使重新设计和重写代码。

确实，除非相关人员接受过安全培训，否则软件应用无法实现可信度。Microsoft 建议开发团队接受以下强制性安全培训：

- 阅读 Michael Howard 所著的 *编写安全代码，第二版*。
- 阅读 Microsoft 网站上发布的 *Microsoft Dynamics AX 编写安全 X++ 代码* 白皮书。
- 完成以下两门 Microsoft eLearning 安全课程：
  - Clinic 2806：面向开发人员的 Microsoft 安全指导培训
  - Clinic 2807：面向开发人员的 Microsoft 安全指导培训 II
- 阅读 Microsoft 向每位 IBI 解决方案提供商分发的 IBI ISV 认证手册中的威胁建模部分。

在需求阶段完成安全培训将提供机会，使安全成为安全开发生命周期每个阶段的首要关注点。

该阶段还必须规划安全，即编写安全计划、定义与安全相关的缺陷分类和进展。我们将在第 8 章中更详细地讨论缺陷管理。

## 设计阶段

设计阶段在可信应用开发中至关重要。精心设计的结构是软件应用可信度的基础。以下是设计阶段的几个关键活动：

- 安全架构和设计指南定义。以下应是此活动的首要关注点：
  - 识别设计技术，例如应用最小权限原则
  - 识别正确运行对安全至关重要的组件
- 为整个系统定义安全架构
- 最小化攻击面
- 默认安全实现：
  - 记录软件攻击面的元素
  - 识别需要默认安装的功能。
- 对每个组件进行威胁建模：
  - 识别资产
  - 识别信任边界
  - 识别威胁
- 进行风险分析：
  - 评估每个功能以及每个权限级别对安全的影响。
  - 识别对策和风险缓解。对策可能包括在信任边界处加密信息以及在通过信任边界之前进行输入验证。

## 实现阶段

在实现阶段，确定最佳实践以实现可信度的目标。一些此类活动包括：

- 识别设计模式以确保安全性、隐私性和可靠性。
- 定义安全测试。以下可能是一些测试目标：
  - 确保被测应用程序（AUT）不会绕过 Dynamics AX 的标准安全模型。
  - 确保系统对安全威胁具有足够的鲁棒性。
  - 确保用户权限根据各种配置（如用户组、公司、配置键、安全键和许可证代码）得到正确执行。
- 应用 Dynamics AX 最佳实践检查工具并记录偏差。
- 执行代码审查以：
  - 确保编码符合规定的设计模式。
  - 检查源代码以检测并移除潜在的安全漏洞。

## 稳定化阶段

这是软件几乎处于稳定状态（即功能完整）并可作为系统进行审查的阶段。在此阶段，安全推进超出了早期阶段进行的安全审查。在此阶段进行代码审查不仅提供了审查新代码的机会，还提供了从整体新视角审查修改后的代码和核心 Dynamics AX 代码的机会。

在 Dynamics AX 中，Microsoft 建议执行以下活动：

- 审查专门设计用于提高可信度（即可靠性、安全性和隐私性）的特性和功能。安全开发生命周期设计阶段进行的风险分析可用于识别此类特性和功能。
- 对整体代码进行全面审查：即新代码和更新的代码。现有代码的影响也应予以考虑。

## 发布阶段

发布阶段的主要活动是进行最终安全审查（FSR）。该活动在实施或交付产品前几个月进行。FSR 应由独立的安全团队在非安全问题最少时进行。FSR 不仅应考虑范围内产品的健康状况，还应考虑之前报告的问题。之前报告的问题可揭示安全问题的模式以及此类问题尚未发现的其他领域。它还可能有助于理解根本原因并采取纠正和预防措施。

## 支持/维护阶段

软件应用不可能 100% 安全——即使今天是 100% 安全，明天也肯定会因发现新型攻击而变得脆弱。应建立支持系统以应对新发现的安全漏洞。响应应包括：

- 评估漏洞并发布咨询和/或更新。
- 对漏洞进行事后分析，并分析产品中出现类似漏洞的可能性并采取预防措施。
- 向安全团队报告新识别的漏洞，以便为同一产品或其他产品的未来版本添加合适的测试用例。

## 威胁建模

威胁建模是一种安全分析技术，用于识别和记录威胁以实现以下目标：

- 识别风险
- 分析风险
- 识别威胁的可能补救或缓解措施
- 帮助设计、开发和测试团队分别改进设计、安全开发和测试的安全方面

为实现上述目标，威胁建模有三个主要活动：即理解对手的视角、表征安全以及确定、分析和记录威胁。

## 如何进行威胁建模

进行威胁建模时，有必要分析系统架构以找到入口点和资产并将其与信任级别交叉引用。入口点或接口是架构中数据和/或控制流在被考虑进行威胁建模的系统与任何其他系统之间转移的位置。所有入口点或接口必须被识别和记录，而不论接口处的信任级别如何。这里的信任级别是指外部实体合法使用这些接口应拥有的权限。

这些接口包括以下内容：

- 打开的套接字
- 远程过程调用（RPC）接口
- Web 服务接口
- 从文件系统读取的数据

在识别入口点后，应识别恶意用户可能滥用的资产（例如操纵、窃取等）。通常受影响最大的资产是接口点处的实体，但它们也可以是其他事物，例如进程令牌、未加密的密码等。所有可能成为威胁目标的资产都应被识别并与信任级别交叉引用。这里的信任级别是指允许访问或以某种方式影响资产所需的权限级别。



After identifying entry points or interfaces, the next step of threat modeling is characterizing the security of the system, which involves binding the threat model, gathering information about dependencies that are security critical, and understanding the internal component structure working of the system. Once this is done, usage scenarios are studied in order to understand how the system will be used. This will help understand the use scenarios beyond the security architecture. After use scenarios, the dependencies are studied, as some of the dependencies on other systems may affect the security of the system under consideration for threat modeling.

在识别出入口点或接口之后，威胁建模的下一步是描述系统的安全性，这涉及绑定威胁模型、收集安全关键的依赖项信息，以及理解系统的内部组件结构和工作方式。完成此步骤后，会研究使用场景以了解系统将如何被使用。这有助于理解超出安全架构的使用场景。在研究使用场景之后，会研究依赖项，因为对其他系统的某些依赖可能会影响正在进行威胁建模的系统的安全性。

After collecting all the information, the system is modeled as a data flow diagram or control flow diagram to understand the system action for various use scenarios and data or control flow across the trust boundaries, etc. The objective of this activity is to know the following:

收集所有信息后，系统被建模为数据流图或控制流图，以便了解各种使用场景下的系统行为以及跨信任边界的数据或控制流等。此活动的目标是了解以下内容：

- How are protected resources affected?
- 受保护资源如何受到影响？
- Where could an external entity manipulate an asset?
- 外部实体可能在何处操纵资产？
- What kind processing occurs beyond entry points?
- 入口点之外进行何种处理？
- How is the information used beyond entry points?
- 入口点之外信息如何被使用？
- What processing or action is done on behalf of external entities?
- 代表外部实体执行了何种处理或操作？

Now all the threats should be documented with all the information such as description of the threat, the target of the attack, the risk of the attack, the possible technique for the attack, and strategy to manage the risk. Now every threat has enough details to review these threats and prioritize them. Some of them may not be great enough to address. The DREAD model may be used to prioritize the threats. DREAD is discussed in Chapter 8.

现在所有威胁都应记录下来，并包含所有信息，例如威胁描述、攻击目标、攻击风险、可能的攻击技术以及管理风险的策略。现在每个威胁都有足够的细节来审查这些威胁并对其进行优先级排序。其中一些可能不足以处理。DREAD 模型可用于对威胁进行优先级排序。DREAD 在第 8 章中讨论。

## Summary

## 摘要

In this chapter we discussed trustworthy computing, the components of trustworthy computing, and how we can achieve trustworthiness. We also discussed phased activities in the security development lifecycle.

本章我们讨论了可信计算、可信计算的组件以及如何实现可信性。我们还讨论了安全开发生命周期中的分阶段活动。

Last but not the least, we discussed threat modeling, which can be considered as the foundation for trustworthy computing.

最后但并非最不重要的是，我们讨论了威胁建模，它可以被视为可信计算的基础。

## 6 Testing

## 6 测试

In the last few chapters we discussed the characteristics of an ERP system and Dynamics AX as an ideal ERP system. Since we need to customize it for every implementation, it is imperative that we preserve the qualities that make Dynamics AX an ideal ERP system. We also discussed the best practices that should be followed during customization to preserve the qualities of Dynamics AX.

在最近几章中，我们讨论了 ERP 系统的特征以及 Dynamics AX 作为理想 ERP 系统的特征。由于我们需要为每次实施进行定制，因此必须保留使 Dynamics AX 成为理想 ERP 系统的品质。我们还讨论了在定制过程中应遵循的最佳实践，以保留 Dynamics AX 的品质。

Now we need to verify if the best practices are followed during customization. In this chapter we will discuss various types of testing, which will cover verification for most of the best practices. IBI references are provided so that readers working with IBI-compliant projects can easily demonstrate IBI compliance. For each type of testing we will discuss the following:

现在我们需要验证在定制过程中是否遵循了最佳实践。本章将讨论各种类型的测试，这将涵盖大多数最佳实践的验证。提供了 IBI 参考，以便从事符合 IBI 标准的项目的读者能够轻松证明其符合 IBI 标准。对于每种类型的测试，我们将讨论以下内容：

- Test objectives
- 测试目标
- Technique
- 技术
- Expected results
- 预期结果
- Completion criterion
- 完成标准
- Special considerations
- 特殊考虑
- Responsibility (i.e. most suitable person to conduct given testing)
- 责任（即最适合执行给定测试的人员）
- Deliverables
- 交付物
- IBI reference
- IBI 参考

## Unit Testing

## 单元测试

As in any project, unit testing is the first level of testing where most of the defects should be caught. The objective of this testing is to verify that the labels, validations, boundary conditions, flow, and calculations are coded as per the defined standards and requirements.

与任何项目一样，单元测试是测试的第一层级，其中应捕获大多数缺陷。此测试的目标是验证标签、验证、边界条件、流程和计算是否按照定义的标准和要求进行编码。

## Testing

## 测试

The effectiveness of the unit testing is very critical not only for the quality of deliverable code, but also to minimize the total efforts of the project team and ensure the on time delivery of the code being achieved. The unit testing is done only by developers. The following table summarizes unit testing:

单元测试的有效性对于交付代码的质量至关重要，同时也有助于最大限度地减少项目团队的总体工作量并确保代码按时交付。单元测试仅由开发人员执行。下表总结了单元测试：

| Item | Details |
|------|---------|
| Specifications for Unit Testing | |
| Specifications for Unit Testing | 单元测试规范 |
| Technique | Trustworthiness test cases, memory management test cases, and functional test cases (validations, conditions, boundary conditions, flow, and calculations) should be verified by the developer. A checklist can be prepared for each kind of code and a unit test document can be prepared for the code under unit testing. A unit test document should be prepared that contains test cases as well as test results. |
| Technique | 技术 |
| Trustworthiness test cases, memory management test cases, and functional test cases (validations, conditions, boundary conditions, flow, and calculations) should be verified by the developer. A checklist can be prepared for each kind of code and a unit test document can be prepared for the code under unit testing. A unit test document should be prepared that contains test cases as well as test results. | 可信性测试用例、内存管理测试用例以及功能测试用例（验证、条件、边界条件、流程和计算）应由开发人员验证。可以为每种类型的代码准备检查清单，并为正在进行单元测试的代码准备单元测试文档。应准备一份包含测试用例和测试结果的单元测试文档。 |
| Expected Results | The application behaves as per the unit test cases. |
| Expected Results | 预期结果 |
| The application behaves as per the unit test cases. | 应用程序的行为符合单元测试用例。 |
| Completion Criteria | All the newly written code is tested. The unit test document is filled. |
| Completion Criteria | 完成标准 |
| All the newly written code is tested. The unit test document is filled. | 所有新编写的代码都经过测试。单元测试文档已填写完毕。 |
| Special Considerations | Dynamics AX 4.0 has a unit test framework. It should be utilized. Code coverage must be analyzed for test cases in unit testing. |
| Special Considerations | 特殊考虑 |
| Dynamics AX 4.0 has a unit test framework. It should be utilized. Code coverage must be analyzed for test cases in unit testing. | Dynamics AX 4.0 具有单元测试框架。应加以利用。必须对单元测试中的测试用例进行代码覆盖率分析。 |
| Responsibility | The developer who have developed the piece of code. |
| Responsibility | 责任 |
| The developer who have developed the piece of code. | 开发该代码片段的开发人员。 |
| Deliverables | Unit test document. |
| Deliverables | 交付物 |
| Unit test document. | 单元测试文档。 |
| IBI Reference | None. |
| IBI Reference | IBI 参考 |
| None. | 无。 |

## Compliance Check to Coding Standards

## 符合编码标准检查

The objective of this test is to ensure adherence to Dynamics AX's best practices of coding in the unit under test. Besides that, conformance to the naming conventions and commenting is also in the scope of this testing. The following table summarizes this testing.

此测试的目标是确保被测单元遵循 Dynamics AX 的编码最佳实践。除此之外，命名约定和注释的符合性也在本次测试的范围内。下表总结了此测试。

**Specifications for Compliance Check to Coding Standards**

**符合编码标准检查规范**

| Technique | Dynamics AX's best-practice tool should be utilized to detect such errors, but it should be ensured that this tool is used in default configuration. 

All naming conventions are reviewed after source code **Titlecase Update** through the Add-Ins menu.

Most of the Microsoft Business Solutions providers have some kind of automated mechanism to mark a comment as soon as a new code block is written or an existing code block is modified.

Errors found are reported in:
- 'Compliance check to coding standards' for deviations from best practices of coding;
- Review and Authorization form for other defects found in this testing. |
| Technique | 技术 |
| Dynamics AX's best-practice tool should be utilized to detect such errors, but it should be ensured that this tool is used in default configuration. 

All naming conventions are reviewed after source code **Titlecase Update** through the Add-Ins menu.

Most of the Microsoft Business Solutions providers have some kind of automated mechanism to mark a comment as soon as a new code block is written or an existing code block is modified.

Errors found are reported in:
- 'Compliance check to coding standards' for deviations from best practices of coding;
- Review and Authorization form for other defects found in this testing. | 应利用 Dynamics AX 的最佳实践工具来检测此类错误，但应确保该工具以默认配置使用。

所有命名约定在通过“Add-Ins”菜单执行源代码 **Titlecase Update** 后进行审查。

大多数 Microsoft Business Solutions 提供商都具有某种自动机制，可在编写新代码块或修改现有代码块时立即标记注释。

发现的错误报告如下：
- 编码最佳实践的偏差报告在“符合编码标准检查”中；
- 本次测试中发现的其他缺陷报告在审查和授权表单中。 |
| Expected Results | Error-free results when running Dynamics AX best-practice tool. Some warnings may be acceptable at the discretion of the Lead Developer. |
| Expected Results | 预期结果 |
| Error-free results when running Dynamics AX best-practice tool. Some warnings may be acceptable at the discretion of the Lead Developer. | 运行 Dynamics AX 最佳实践工具时结果无错误。某些警告可在主导开发人员的酌情决定下接受。 |
| Completion Criteria | All best practice errors have been either removed or negotiated with Team Leads and documented accordingly.

All issues related to naming conventions have been fixed.

All errors found are recorded in the review and authorization form. |
| Completion Criteria | 完成标准 |
| All best practice errors have been either removed or negotiated with Team Leads and documented accordingly.

All issues related to naming conventions have been fixed.

All errors found are recorded in the review and authorization form. | 所有最佳实践错误均已删除或与团队负责人协商并相应记录。

所有与命名约定相关的问题均已修复。

所有发现的错误均已记录在审查和授权表单中。 |
| Special Considerations | None. |
| Special Considerations | 特殊考虑 |
| None. | 无。 |
| Responsibility | Peer to Developer or Lead Developer. |
| Responsibility | 责任 |
| Peer to Developer or Lead Developer. | 开发人员的同行或主导开发人员。 |
| IBI reference | 2.2 |
| IBI reference | IBI 参考 |
| 2.2 | 2.2 |

## Compliance Check to Design and Architecture Guidelines

## 符合设计和架构指南检查

This test ensures adherence to design concepts and standards prescribed in the Dynamics AX Developer's Guide, Dynamics AX Developer's Best Practice Handbook, and Dynamics AX User Assistance Best Practice Handbook. The following table summarizes the specifications:

此测试确保遵循 Dynamics AX 开发人员指南、Dynamics AX 开发人员最佳实践手册和 Dynamics AX 用户协助最佳实践手册中规定的设计概念和标准。下表总结了规范：

**Specifications for the Compliance Check to Design and Architecture Guidelines**

**符合设计和架构指南检查规范**

| Technique | The application objects should be reviewed to check their compliance with the guidelines and design patterns.

All errors found are recorded in the review and authorization form. |
| Technique | 技术 |
| The application objects should be reviewed to check their compliance with the guidelines and design patterns.

All errors found are recorded in the review and authorization form. | 应审查应用程序对象以检查其是否符合指南和设计模式。

所有发现的错误均记录在审查和授权表单中。 |
| Expected Results | All objects found compliant to prescribed standards and guidelines. |
| Expected Results | 预期结果 |
| All objects found compliant to prescribed standards and guidelines. | 所有对象均符合规定的标准和指南。 |
| Completion Criteria | Objects found compliant to prescribed standards and guidelines. |
| Completion Criteria | 完成标准 |
| Objects found compliant to prescribed standards and guidelines. | 对象符合规定的标准和指南。 |
| Special Considerations | The design patterns document used as reference must be reviewed (by a competent person or team) before starting this test. |
| Special Considerations | 特殊考虑 |
| The design patterns document used as reference must be reviewed (by a competent person or team) before starting this test. | 开始此测试前，必须审查用作参考的设计模式文档（由有能力的人员或团队审查）。 |
| Responsibility | Lead Developer. |
| Responsibility | 责任 |
| Lead Developer. | 主导开发人员。 |
| IBI Reference | 2.3 |
| IBI Reference | IBI 参考 |
| 2.3 | 2.3 |

## Help Navigation (User Assistance) Testing

## 帮助导航（用户协助）测试

The objectives of this testing are as follows:

本次测试的目标如下：

- To ensure that every form has corresponding help and that it is accessible through the F1 key.
- 确保每个表单都有相应的帮助，并且可以通过 F1 键访问。
- To ensure that the file format for help files is .chm.
- 确保帮助文件的格式为 .chm。

The following table summarizes the testing.

下表总结了测试。

**Specifications for Help Navigation (User Assistance) Testing**

**帮助导航（用户协助）测试规范**

| Technique | Verification for each form by pressing the F1 key. The help text displayed should be as per the stated requirement.

While defining the help requirements, the following should be taken into consideration:
- The online help must be easy to understand and must tell the user how to perform specific tasks.
- It must provide a user experience that is consistent in terms of the writing style and depth of information. Ideally this can be covered under Functional testing if help has been taken care of, else a separate test must be performed for help navigation when help is added after the functional testing.

A User assistance test document should be prepared, which will form the basis for testing. |
| Technique | 技术 |
| Verification for each form by pressing the F1 key. The help text displayed should be as per the stated requirement.

While defining the help requirements, the following should be taken into consideration:
- The online help must be easy to understand and must tell the user how to perform specific tasks.
- It must provide a user experience that is consistent in terms of the writing style and depth of information. Ideally this can be covered under Functional testing if help has been taken care of, else a separate test must be performed for help navigation when help is added after the functional testing.

A User assistance test document should be prepared, which will form the basis for testing. | 通过按 F1 键对每个表单进行验证。显示的帮助文本应符合所述要求。

在定义帮助要求时，应考虑以下事项：
- 在线帮助必须易于理解，并必须告诉用户如何执行特定任务。
- 它必须提供在写作风格和信息深度方面一致的用户体验。理想情况下，如果已处理帮助，这可以涵盖在功能测试下，否则在功能测试后添加帮助时，必须单独执行帮助导航测试。

应准备一份用户协助测试文档，这将构成测试的基础。 |
| Expected Results | Pressing F1 while an application window is open must result in a launch of a contextually-correct help window.

The help window must comply with Dynamics AX help standards. |
| Expected Results | 预期结果 |
| Pressing F1 while an application window is open must result in a launch of a contextually-correct help window.

The help window must comply with Dynamics AX help standards. | 在应用程序窗口打开时按 F1 必须启动上下文正确的帮助窗口。

帮助窗口必须符合 Dynamics AX 帮助标准。 |
| Completion Criteria | All forms in the code under test have been tested and all help documentations are as per Dynamics AX help standards. |
| Completion Criteria | 完成标准 |
| All forms in the code under test have been tested and all help documentations are as per Dynamics AX help standards. | 被测代码中的所有表单均已测试，且所有帮助文档均符合 Dynamics AX 帮助标准。 |
| Special Considerations | Performing this testing along with Functional testing will reduce the efforts required. |
| Special Considerations | 特殊考虑 |
| Performing this testing along with Functional testing will reduce the efforts required. | 与功能测试一起执行此测试将减少所需工作量。 |
| Responsibility | Tester. |
| Responsibility | 责任 |
| Tester. | 测试员。 |
| IBI Reference | 4.1 |
| IBI Reference | IBI 参考 |
| 4.1 | 4.1 |



## 用户体验和可用性测试

本测试的目标是确保以下内容：

- 用户界面保持适当的一致性
- 遵守 Windows 用户界面指南
- 遵守 Dynamics AX UI 指南

下表总结了本测试的规范。

### 用户体验和可用性测试规范

|  |  |
|---|---|
| 技术 | 由测试工程师进行手动验证。应使用通用测试包（GUI 合规性测试文档）执行此测试。遇到的问题在缺陷工具中报告。 |
| 预期结果 | 观察到适当的一致性，并符合 Dynamics AX 用户界面标准。 |
| 完成标准 | 代码下测试中的所有表单均已按照 Microsoft Dynamics AX GUI 标准进行验证。 |
| 特别考虑 | 此测试必须在分辨率为 1024*768 像素的 SVGA 显示器上进行。 |
| 责任 | 测试员。 |
| IBI 参考 | 5.1 |

## 商业智能或报告标准验证

本测试的目标是验证是否遵守 Dynamics AX 开发人员手册中规定的报告指南，例如：

- 使用 Dynamics AX 报表向导
- 根据实施指南使用 SQL Server 报表服务

下表总结了此测试

### 商业智能/报告标准验证规范

|  |  |
|---|---|
| 技术 | 通过代码审查进行手动验证。 |
| 预期结果 | 遵守 Dynamics AX 开发人员指南中提到的报告指南。 |
| 完成标准 | 代码下测试中的所有报表均已验证。遇到的问题在审查和授权表单中报告。 |
| 特别考虑 | 应考虑报表的用途。如果测试中的报表用于某些监管目的，并且应与监管机构发布的格式相同，可以记录例外情况。 |
| 责任 | 主开发人员。 |
| IBI 参考 | 6.1 |

## 性能测试

本测试的目标是验证安装测试中应用程序后，核心 Dynamics AX 流程不会出现显著的性能下降。性能下降预计不会超过 5%。下表总结了此测试。

### 性能测试规范

|  |  |
|---|---|
| 技术 | 应使用 Dynamics AX 基准工具包进行性能测试。应执行标准 Dynamics AX 的性能测试以建立性能基线。应使用测试中应用程序再次运行相同的测试，并将结果与基线值进行比较。应检查 Dynamics AX 事件日志中有关性能的错误和警告。应为要测试的业务流程编写测试脚本。应选择受定制影响最大且处于应用程序主要业务流程中的业务流程。发现的问题记录在审查和授权表单中。 |
| 预期结果 | 在将此解决方案安装到核心 Dynamics AX 之前和之后运行性能基准工具时，不会出现显著（>5%）的性能下降。 |
| 完成标准 | 性能下降不超过 5%。 |
| 特别考虑 | 在开始性能测试之前，必须计划并审查应用程序测试演示数据。 |
| 责任 | 测试员。 |
| IBI 参考 | 7.1 |

此测试仅适用于 Microsoft 已发布 Dynamics AX 基准套件的国家/地区。

## 安全测试

任何软件应用程序如果不安全就无法发挥作用。根据 Microsoft 的愿景，IT 应用程序应像我们家中的水电设备一样安全。Dynamics AX 实施的安全测试目标应包括以下内容：

- 确保测试中应用程序 (AUT) 不会绕过标准安全模型
- 确保系统足够健壮以应对安全威胁
- 对威胁建模、安全措施等的分析
- 最终代码审查，以确保没有关键安全问题未解决

下表总结了此测试。

### 安全测试规范

|  |  |
|---|---|
| 技术 | 应作为此测试的一部分执行以下验证：在 **管理 | 设置 | 安全** 菜单中，AUT 必须在模块的树视图列表中有一个节点。对用户组设置无访问权限不应允许查看与 AUT 相关的任何功能、表单或菜单项的部分。应编写安全测试计划以定义测试范围、缺陷分类、根本原因分类等。应向测试员提供安全检查清单，测试时应填写。测试结果应记录在 Excel 表格中，该表格还将包含错误分类和根本原因分类。 |
| 预期结果 | - 访问控制安全的工作方式与标准 Dynamics AX 产品相同。 - 针对威胁建模中识别的每个安全威胁，都找到了对策。 |
| 完成标准 | - 节点和树结构的验证。 - 从安全菜单中删除 AUT 的验证。 |
| 特别考虑 | 应使用 Dynamics AX 独特的挑战检查对各个组的访问权限；例如，如果访问权限设置为 NO，所有菜单项、按钮、表单等都应不可见。 |
| 责任 | 测试员。 |
| IBI 参考 | 8.1 |

## 授权测试

授权测试是一种安全测试。此测试非常重要，并且需要补充安全测试以实现可信度的目标。此测试确保 Dynamics AX 中根据以下配置强制执行用户权限：

- 用户组
- 公司
- 配置代码和安全密钥
- 许可证代码

下表总结了授权测试。

### 授权测试规范

|  |  |
|---|---|
| 技术 | 应测试以下场景： - 设置为 **查看级别** 的安全密钥允许用户查看记录。 - 顶级安全密钥被禁用；用户不应有权访问该项目。 - 设置为 **创建级别** 的安全密钥允许用户创建新记录。 - 当配置密钥关闭时，功能被禁用。 - 当其安全访问设置为 **无访问** 时，表单控件不可见。 - 如果表中的字段设置为 **无访问**，则该字段不会出现在表单上。 - 用户无法看到他们无权访问的表单。 - 用户无法看到他们无权访问的报表。 |
| 预期结果 | 各种权限控制手段的组合应以标准 Dynamics AX 的方式运行。 |
| 完成标准 | 所有场景均已执行。 |
| 特别考虑 | 设计测试场景时必须注意以下特殊考虑： - 对于权限有限的用户，必须检查所有条件（即 **无访问**、**编辑**、**创建**、**查看**、**完全控制**）。 - 属于多个用户组（权限冲突）的用户必须进行测试。 - 属于多个公司的用户在不同公司可能具有不同的权限。设计测试场景时必须特别考虑这一点。 - Dynamics AX 系统具有有限的许可证代码，例如，具有管理员权限的用户可能根据提供的许可证无法访问 AR 或 AP。 |
| 责任 | 业务分析师。 |
| IBI 参考 | 8.6 |

## 全球化测试

本测试的目标包括验证遵守在任何国家/地区无需翻译即可运行的规则（即正确处理日期/时间、货币、字符串格式）以及在不同（受支持的）Windows 语言版本上运行的能力。它还应验证是否遵循 Dynamics AX 层约定。下表总结了此测试。

### 全球化测试规范

|  |  |
|---|---|
| 技术 | Dynamics AX 最佳实践工具不应显示任何与标签文件相关的错误或任何其他表明日期/时间、货币或字符串格式被硬编码的错误。 |
| 预期结果 | - 确保应用程序可在任何文化/区域设置中运行。 - 系统应在不同（受支持的）Windows 语言版本上运行。 - 遵守 Dynamics AX 全球化约定（即按层）。 |
| 完成标准 | Dynamics AX 最佳实践工具不显示与全球化相关的任何错误。 |
| 特别考虑 | 第一个检查点应在 Dynamics AX 最佳实践合规性检查期间涵盖。 |
| 责任 | 测试员。 |
| IBI 参考 | 10.1, 10.2, 10.3 |

## 本地化测试

以下应为 Dynamics AX 实施本地化测试的目标：

- 根据国家/地区设置启用和禁用适当国家/地区功能的配置密钥
- 手动验证以检查所有特定于国家/地区的本地化是否在 DIS 层中
- 在执行 Dynamics AX 最佳实践工具时，检查 Dynamics AX 事件日志中指示代码中硬编码字符串的错误



## 测试

下表总结了此项测试。

| 本地化测试规范 | |
|---|---|
| 技术 | - 在执行 Dynamics AX 最佳实践检查工具时，检查 Dynamics AX 事件日志（或最佳实践工具）是否存在指示代码中存在硬编码字符串的错误。<br>- 手动检查配置键，并确保由独立软件供应商（ISV）发布的应用程序包含应用程序/定制实现需要实现的区域设置的配置键。 |
| 完成标准 | - Dynamics AX 最佳实践检查工具不包含与文本字符串与代码分离相关的任何错误。<br>- Dynamics AX 事件日志仅包含测试团队接受的警告。 |
| 预期结果 | Dynamics AX 最佳实践检查工具不应显示任何错误消息。<br>除非在文档中给出说明，否则 Dynamics AX 最佳实践检查工具不应显示任何警告消息。<br>ISV 发布的配置键表单应根据定制包含区域设置的配置键。 |
| 特别考虑 | - Dynamics AX 最佳实践检查工具应处于默认配置。<br>- SysBPCheckIgnore 宏不应包含任何类型的条目。 |
| 责任 | 测试员。 |
| IBI 参考 | 11.1, 11.4 |

## 平台兼容性测试

此项测试用于验证被测应用程序在受支持的平台和数据库上成功运行。下表总结了此项测试。

| 平台兼容性测试规范 | |
|---|---|
| 技术 | 在每个平台上，应用程序被安装、打开，并检查部分功能是否正常运行。 |
|  | 应填写审查和授权表以记录结果。 |
| 预期结果 | 解决方案在受支持的环境中正确安装并运行。 |
| 完成标准 | 在嵌入式 Excel 表中提到的所有平台上成功执行。 |
| 特别考虑 | 需要配备所有必需平台的测试实验室。 |
| 责任 | 测试员。 |
| IBI 参考 | 12.1 |

## 安装测试

此项测试的目标是确保安装过程顺利。如果安装无法完成，则新安装的项目将被卸载，并且不会影响已安装的 Dynamics AX。目标可能包括以下内容。

- 确保核心 Dynamics AX 产品兼容的安装程序
- 确保安装程序正确注册 DLL 和 ActiveX 控件
- 确保安装程序检查正确的 Dynamics AX 版本和服务包级别
- 确保安装程序检查必需的 Dynamics AX 模块是否存在
- 确保配置设置是固有的或已创建
- 确保安装程序提供所有依赖项，即安装所需的文件

## 测试

下表总结了此项测试。

| 安装测试规范 | |
|---|---|
| 技术 | 安装测试文档应准备好，并且测试应根据该文档执行。<br>AUT 应使用其安装指南进行安装，之后应验证执行情况。<br>每个 DLL 和 ActiveX 控件的共享计数应在安装或卸载或重新安装之前和之后记录。还应检查注册表是否已使用 DLL 和 ActiveX 控件的正确版本进行更新。<br>安装过程中可能会使用不合适的 Dynamics AX 版本。安装过程应在安装早期阶段检查 Dynamics AX 的正确版本。<br>应考虑每个因素，以确保安装程序检查所有依赖项，如果任何依赖项不可用，安装程序应中止，而不会对核心 Dynamics AX 造成任何影响。<br>还应检查安装程序是否不会要求从 Internet 等下载任何文件。<br>确保安装程序通过从配置键中删除必需模块来检查必需的 Dynamics AX 模块。对每个必需模块应重复此测试。 |
| 预期结果 | 安装程序符合本测试目标中提到的准则。 |
| 完成标准 | AUT 根据安装测试文档成功安装并运行。 |
| 特别考虑 | 仅在以下情况后才能开始安装测试：<br>- 安装指南可用<br>- 提供了 AUT 创建的注册表记录信息<br>- 提供了包含在 AUT 安装中的所有系统和应用程序组件的信息 |
| 责任 | 测试员。 |
| IBI 参考 | 13.1, 13.2, 13.3, 13.4, 13.5, 13.6 |

## 备份和恢复测试

此项测试的目标是验证备份和恢复数据以及 AUT 的能力。通常 X++ 应用程序与标准 Dynamics AX 产品共享同一数据库，因此定制被视为标准 Dynamics AX 的扩展。在这种情况下，标准 Dynamics AX 的备份和恢复过程将有效，因此无需为此目的提供单独的机制。当然，无需测试标准 Dynamics AX 提供的备份和恢复机制。

如果定制的 X++ 应用程序具有单独的数据库，则适用此测试。下表总结了此项测试。

| 备份和恢复测试规范 | |
|---|---|
| 技术 | 执行的备份和恢复过程序列应将应用程序恢复到相同状态。<br>应准备备份和恢复测试文档以指定测试用例和预期结果。 |
| 预期结果 | 执行的备份和恢复过程序列将应用程序恢复到相同状态。 |
| 完成标准 | 如果应用程序可以通过备份和恢复过程序列恢复到原始状态。 |
| 特别考虑 | 应用程序备份和数据备份过程是两件不同的事情，因此应分别测试。如果测试与使用单独数据库的 X++ 应用程序相关，则仅应测试数据备份和恢复机制，而如果是 .NET 应用程序，则两者都需要测试。 |
| 责任 | 测试员。 |
| IBI 参考 | 14.1 |

## 可扩展性测试

此项测试的目标是验证 AUT 的每个元素在 AOT 文档中是否有对应的帮助文本节点。下表总结了此项测试。

| 可扩展性测试规范 | |
|---|---|
| 技术 | 手动验证 AOT 文档中的每个帮助文本节点。 |
| 预期结果 | 所有（新和定制的）AOT 元素在 AOT 文档中都有相应的帮助文本。 |
| 完成标准 | 已验证所有 AOT 元素（仅与 AUT 相关）的帮助文本节点存在。 |
| 特别考虑 | 无。 |
| 责任 | 测试员。 |
| IBI 参考 | 15.1 |

## 功能测试

此项测试的目标是验证所有所述功能需求是否按预期方式完成。下表总结了此项测试。

| 功能测试规范 | |
|---|---|
| 技术 | 对 AUT 进行手动测试。对于复杂功能，会准备详细的测试用例文档，作为功能测试的基础。<br>对于其他功能，会准备测试脚本，并根据该脚本进行测试。<br>遇到的 issue 会记录在缺陷跟踪工具中，并填写审查和授权表。 |
| 预期结果 | 系统按测试用例或测试脚本运行。 |
| 完成标准 | 100% 测试用例执行并通过。 |
| 特别考虑 | 应考虑所有正向和负向测试用例。<br>还应考虑基于场景的测试。 |
| 责任 | 业务分析师。 |
| IBI 参考 | 无。 |



## 回归测试

此测试的目标是验证集成后的应用程序是否运行良好，并且新功能没有对现有功能产生不利影响。下面的表格总结了此测试。

回归测试规范

| 项目 | 描述 |
|---|---|
| 技术 | 使用为一致性验证测试编写的测试脚本进行手工测试。 |
| 预期结果 | 之前测试的功能按预期工作。 |
| 完成标准 | 所有一致性验证测试的测试脚本或用例均已成功执行。 |
| 特殊考虑 | 必须仔细选择主要功能和贡献功能。 |
| 责任 | 测试员。 |
| IBI 参考 | 3.5。 |

## SDL 验证

此活动具有以下需要验证的目标：

- 确保已实施安全开发生命周期（SDL），并且正确遵循
- 审查所有开放的安全相关缺陷，并确保没有关键安全缺陷处于开放状态
- 审查应用程序以确定是否由标准工具和程序遗漏了任何安全漏洞

## 测试

下面的表格总结了规范。

SDL 验证规范

| 项目 | 描述 |
|---|---|
| 技术 | 使用为一致性验证测试编写的测试脚本进行手工测试。手工代码审查。 |
| 预期结果 | - 没有处于开放状态的安全相关问题。<br>- 确认所有 SDL 活动均已正确执行：<br>- 需求阶段 识别安全目标和挑战并据此制定方法<br>- 设计阶段 定义安全架构和设计指南 记录软件攻击面元素 进行威胁建模 定义补充交付标准<br>- 实现阶段： 应用编码和测试标准 应用安全测试工具 应用静态分析编码标准 进行代码审查<br>- 验证阶段 重点代码审查 将代码作为一个应用进行安全审查<br>- 发布阶段： 最终安全审查 |
| 完成标准 | 各阶段的各项活动均已完成。活动的成果已记录并妥善处理。 |
| 特殊考虑 | 在任何阶段发现的安全问题都应记录在缺陷管理工具中，尤其是在为安全相关问题专门设置的所有字段中。这将确保适当的分析、优先级排序等。 |
| 责任 | 测试员。 |
| IBI 参考 | 8.7 |

## 摘要

在本章中，我们研究了所有推荐的测试类型、其目标、执行技术、预期结果、完成标准以及每个由 IBI 或作者推荐的测试类型的特殊考虑（如有）。这将确定所执行各种测试类型的范围和愿景。

每种测试类型也映射到相应的 IBI 参考。这将使 IBI 合规性监控变得容易。

对于任何类型的测试，都必须在软件测试的适当阶段执行。各种测试阶段、其进入和退出标准等将在下一章中讨论。
