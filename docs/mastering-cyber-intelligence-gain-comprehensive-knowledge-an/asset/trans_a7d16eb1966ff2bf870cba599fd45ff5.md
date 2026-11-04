
## Note 4 – 展望

Cybersecurity operations can be unpredictable and complex because adversaries can create advanced attack methods (for example, **Advanced Persistent Threats – APTs**). -> 网络安全运营可能难以预测且复杂，因为对手可以创建先进的攻击方法（例如，**高级持续性威胁 – APTs**）。
A CTI analyst should always be on the lookout to ensure that their consumers (customers or organizations) do not become victims – they must be aware of what is happening or could happen – **knowing the threat landscape**. -> CTI 分析师应始终保持警惕，以确保其消费者（客户或组织）不会成为受害者——他们必须了解正在发生或可能发生的事情——**了解威胁态势**。
Threat intelligence is based on evidence collected from different sources. -> 威胁情报基于从不同来源收集的证据。
Therefore, it is crucial to leverage the data and assess the defense mechanism. -> 因此，充分利用数据并评估防御机制至关重要。
For example, the CTI team or analyst can use the collected intelligence data (in the form of published reports, malware data, collective intelligence data, and more) to explore past threats and predict future threats. -> 例如，CTI 团队或分析师可以使用收集到的情报数据（以已发布报告、恶意软件数据、集体情报数据等形式）来探索过去的威胁并预测未来的威胁。
In *Chapter 9, AI Application in Cyber Threat Analytics*, we look at some methods used for proactive threat defense. -> 在*第 9 章，网络威胁分析中的人工智能应用*中，我们探讨了一些用于主动威胁防御的方法。

With **machine learning** (**ML**) and **artificial intelligence** (**AI**), organizations can use advanced analytics models to predict threats and attacks. -> 借助**机器学习**（**ML**）和**人工智能**（**AI**），组织可以使用高级分析模型来预测威胁和攻击。
However, AI and ML should not be considered standalone technologies for threat intelligence. -> 然而，AI 和 ML 不应被视为威胁情报的独立技术。
They must be combined with threat analytics and human expertise to be effective. -> 它们必须与威胁分析和人类专业知识相结合才能发挥效用。
Policy makers can leverage the threat landscape and the predicted threats to implement stronger security and data protection policies. -> 政策制定者可以利用威胁态势和预测的威胁来实施更强的安全和数据保护政策。
And organizations can leverage CTI's advanced analytics capabilities to address current and future threats through the following question: *How is threat intelligence protecting the organization from current and future threats?* -> 而组织可以通过以下问题利用 CTI 的高级分析能力来应对当前和未来的威胁：*威胁情报如何保护组织免受当前和未来威胁？*
A CTI analyst addresses an organization's short-term and long-term threat intelligence goals by answering that question. -> CTI 分析师通过回答该问题来解决组织的短期和长期威胁情报目标。

## Note 5 – 事实与来源

Threat intelligence relies on *evidence*. -> 威胁情报依赖于*证据*。
Evidence comes from data collected from different sources. -> 证据来自从不同来源收集的数据。
The integrity of a CTI program depends on the data source's credibility. -> CTI 计划的完整性取决于数据源的可信度。
Although assumptions and judgment can be incorporated in threat intelligence analytics, conclusive arguments must be based on facts. -> 尽管假设和判断可以纳入威胁情报分析中，但结论性论证必须基于事实。
Therefore, the CTI team must have a process to validate all data sources used in a CTI program. -> 因此，CTI 团队必须拥有验证 CTI 计划中使用的所有数据源的流程。
Due to the complexity of CTI operations, the data handling process must be transparent to identify missing components that can lead to analytics gaps. -> 由于 CTI 运营的复杂性，数据处理流程必须透明，以便识别可能导致分析差距的缺失组件。
The transparency of facts and sources increases an organization's confidence and trust in the CTI output result. -> 事实和来源的透明度增加了组织对 CTI 输出结果的信心和信任。
In other words, provide as many sources of information as possible. -> 换句话说，提供尽可能多的信息来源。

The CTI team (or analyst) must use industry best practices to handle and validate data sources. -> CTI 团队（或分析师）必须使用行业最佳实践来处理和验证数据源。
Policy makers should use the *Facts and sourcing* tradecraft note to document the best practices analysts should use for effective intelligence solutions. -> 政策制定者应使用*事实与来源* tradecraft 注释来记录分析师应为有效情报解决方案使用的最佳实践。
The original tradecraft notes (as in the CIA's compendium of analytic tradecraft notes) provide guidelines that can be used to authenticate intelligence sources and help intelligence analysts avoid interpretation errors. -> 原始 tradecraft 注释（如 CIA 的分析 tradecraft 注释汇编中所述）提供了可用于验证情报来源并帮助情报分析师避免解释错误的指南。
Some of those guidelines are adapted to CTI as follows: -> 其中一些指南已改编为适用于 CTI，如下所示：

- **Precision in what is known from the data:** A CTI analyst should (1) know the threat information available in a data source. They should report any information gaps internally and to the data source provider. Information can be adversaries **Tactics, Techniques, Procedures (TTPs)**, or threat-specific indicators (IP addresses, domains, hashes, and so on). (2) Select the correct data source for the threat intelligence program. -> - **从数据中已知内容的精确性：** CTI 分析师应 (1) 了解数据源中可用的威胁信息。他们应内部报告任何信息缺口并通知数据源提供者。信息可以是对手的**战术、技术和程序 (TTPs)**，或特定威胁指标（IP地址、域名、哈希值等）。(2) 为威胁情报计划选择正确的数据源。
- **Distinction of factual data components:** A CTI analyst should (1) identify valuable indicators in the collected data. These factual indicators can directly relate to adversaries' activities. (2) Understand the part of the data source that is not factual. For example, TTPs can uncover an adversary's capability and opportunity. But their true intent is unlikely to be identified through a threat intelligence shared report. An adversary's intent is often tracked over time. -> - **事实数据组件的区分：** CTI 分析师应 (1) 识别收集数据中的有价值指标。这些事实指标可以直接与对手的活动相关。(2) 了解数据源中非事实的部分。例如，TTPs 可以揭示对手的能力和机会。但其真实意图不太可能通过共享的威胁情报报告来识别。对手的意图通常会随时间推移而被追踪。
- **Possibility of deception:** Adversaries can manipulate indicators and information they share about themselves to confuse and mislead CTI analysts. For example, threat actors (or adversaries) can include random IP addresses, domains, and URLs in tools to hide the legitimate indicators. If facts are built on misleading information, the CTI result will be erroneous. Therefore, a CTI analyst should benchmark shared facts and information using different sources and platforms. Ensure data facts remain consistent across all sources. -> - **欺骗的可能性：** 对手可以操纵他们分享的指标和信息，以混淆和误导 CTI 分析师。例如，威胁行为者（或对手）可以在工具中包含随机 IP 地址、域名和 URL 来隐藏合法指标。如果事实建立在误导性信息之上，CTI 结果将出现错误。因此，CTI 分析师应使用不同来源和平台对共享的事实和信息进行基准比较。确保数据事实在所有来源中保持一致。
- **Collaboration on sharing intelligence:** Threat intelligence is a collaborative program. A CTI analyst should emphasize the importance of CTI sharing. Policy makers should also create policies that encourage external dissemination of threat intelligence, such as the best way to collectively fight cybercrimes and protect organizations. -> - **情报共享的协作：** 威胁情报是一个协作项目。CTI 分析师应强调 CTI 共享的重要性。政策制定者还应制定鼓励威胁情报外部传播的政策，例如集体打击网络犯罪和保护组织的最佳方式。

The preceding guidelines describe a CTI analyst's position face-to-face with information and data. CTI analysts must develop a strong character of assessing shared threat intelligence information (reports, data feeds, and so on). -> 前述指南描述了 CTI 分析师面对信息和数据时的立场。CTI 分析师必须培养评估共享威胁情报信息（报告、数据馈送等）的坚强品格。

## Note 6 – 分析专长

Analytic expertise identifies the *skills* to demonstrate intelligence knowledge. -> 分析专长确定了展示情报知识的*技能*。
The CTI team must showcase their knowledge and expertise on CTI matters. -> CTI 团队必须展示他们在 CTI 方面的知识和专业知识。
The expertise is demonstrated through professional experience, research, and certifications. -> 专业知识通过专业经验、研究和认证来体现。
Professional experience includes past projects and success stories. -> 专业经验包括过去的项目和成功案例。
Intelligence expertise is not linked to personal traits only, but also professionalism toward the organization's interests and goals (such as the on-time delivery of results and constant collaboration with the company's teams). -> 情报专业知识不仅与个人特质相关，还与对组织利益和目标的专业性相关（如按时交付成果并与公司团队保持持续协作）。
When an organization must make its security decisions based on your threat intelligence analysis skills, you must demonstrate your knowledge in the area and use facts to do so. -> 当组织必须基于你的威胁情报分析技能做出安全决策时，你必须展示你在该领域的知识并据此使用事实。

The demonstration of expertise builds credibility for the threat intelligence analyst. -> 展示专业知识为威胁情报分析师建立了可信度。
Policy makers through sound policies, need to ensure that the right people are selected for CTI programs. -> 政策制定者通过健全的政策，需要确保选对合适的人选加入 CTI 项目。
Organizations should use analytic expertise-related policies in the selection of CTI providers. -> 组织应在选择 CTI 提供商时使用与分析专长相关的政策。
The main question that should drive the appointment of a CTI provider is *how knowledgeable and skillful our prospective intel provider is?* -> 决定任命 CTI 提供商的主要问题是*我们潜在的情报提供商有多有知识和技能？*
An organization must evaluate a CTI provider's expertise based on the following four points: -> 组织必须根据以下四个要点评估 CTI 提供商的专业知识：

- 1. Interpretation and assessment of technical data -> - 1. 对技术数据的解释和评估
- 2. Reliability of the data to support the threat intelligence program and conclusive analytics -> - 2. 数据支持威胁情报计划和结论性分析的可靠性
- 3. Handling of ambiguity in the threat intelligence process -> - 3. 处理威胁情报过程中的模糊性
- 4. A demonstration, through use cases, of how threat intelligence will impact decision making -> - 4. 通过用例展示威胁情报将如何影响决策制定

There should be no limit in the amount of expertise an organization expects when selecting the right threat intelligence analyst or provider – the more knowledgeable the analyst, the better. -> 在选择合适的威胁情报分析师或提供商时，组织对专业知识的期望不应有限制——分析师知识越丰富越好。

## Note 7 – 有效摘要

The *Effective summary* tradecraft note identifies the skills to present intelligence results. -> *有效摘要* tradecraft 注释确定了呈现情报结果的技能。
A threat intelligence summary is a recommendation for what to do next. -> 威胁情报摘要是对下一步该做什么的建议。
An effective summary should not be boring. It must be *detailed* and *informative*. -> 有效的摘要不应枯燥。它必须是*详细*且*信息丰富*的。
Therefore, a threat intelligence analyst should write threat intelligence reports for all stakeholders focusing on new findings and the next steps. -> 因此，威胁情报分析师应为所有利益相关者撰写威胁情报报告，重点关注新发现和下一步行动。

On the contrary, we should ensure that the audience is hooked on it. -> 相反，我们应确保受众对其产生兴趣。
We must consider the audience's needs and time constraints. -> 我们必须考虑受众的需求和时间限制。
And, if possible, who they can share it with (for example, technical people or strategic people). -> 并且，如果可能的话，他们可以与谁分享（例如，技术人员或战略人员）。
Threat intelligence analysts face challenges in drafting appropriate content for consumers. -> 威胁情报分析师在为消费者起草适当内容时面临挑战。
However, the most important elements to consider should be the following: -> 然而，应考虑的最重要要素应如下：

- 1. Presenting and highlighting the key takeaways of the CTI program -> - 1. 呈现并突出 CTI 计划的关键要点
- 2. Using data, facts, and evidence to support the proposed takeaways -> - 2. 使用数据、事实和证据支持拟议的要点

Policy makers can rely on experienced threat intelligence analysts to create standard documentation styles to facilitate information sharing. -> 政策制定者可以依靠经验丰富的威胁情报分析师创建标准文档样式以促进信息共享。
Reporting is covered in Chapter 14, Threat Intelligence Reporting and Dissemination. -> 报告内容在第 14 章《威胁情报报告与传播》中有所涵盖。
Intelligence reports must be actionable. -> 情报报告必须具有可操作性。

## Note 8 – 实施分析

The Implementation analysis tradecraft note identifies the skills required to evaluate alternatives to achieve the business objectives. -> 实施分析 tradecraft 注释确定了评估实现业务目标的替代方案所需的技能。
It clarifies the tactics that the organization or the intelligence consumers should adopt. -> 它阐明了组织或情报消费者应采取的策略。
The threat intelligence analyst and the organization should work together to implement the correct strategy. -> 威胁情报分析师和组织应共同合作实施正确的策略。

A CTI analyst assesses and provides the results to the consumer, who then selects the right tactic to meet the business needs (take the appropriate actions based on the CTI program output). -> CTI 分析师评估并向消费者提供结果，消费者随后选择合适的策略以满足业务需求（根据 CTI 计划输出采取适当行动）。
For example, implementation analysis can identify the channels and techniques used by the adversaries and highlight how the consumer (organization or individual) is or is not vulnerable to those techniques. -> 例如，实施分析可以识别对手使用的渠道和技术，并突出消费者（组织或个人）对这些技术的脆弱性或不脆弱性。
So, implementation analysis provides the best evidence in understanding how the adversaries operate – it exposes the adversaries. -> 因此，实施分析提供了理解对手如何运作的最佳证据——它暴露了对手。
Therefore, a CTI analyst should give a high priority to implementation analysis. -> 因此，CTI 分析师应高度重视实施分析。

## Note 9 – 结论

The Conclusions tradecraft note identifies the skills required to decide on the final output. -> 结论 tradecraft 注释确定了决定最终输出所需的技能。
It presents the findings after an intense and complex threat intelligence process. -> 它在经过激烈且复杂的威胁情报流程后呈现发现结果。
The conclusions are essential as they define the primary value of threat intelligence programs. -> 结论至关重要，因为它们定义了威胁情报计划的主要价值。
CTI analysts must be precise and confident in their conclusions by separating important elements from other standard output elements. -> CTI 分析师必须通过将重要元素与其他标准输出元素区分开来，对其结论保持精确和自信。
Analysts must expose the threat patterns discovered in the data. -> 分析师必须揭示数据中发现的威胁模式。
A threat intelligence analyst's expertise is critical in the conclusions, especially when handling inconsistencies. -> 威胁情报分析师的专业知识在结论中至关重要，尤其是在处理不一致情况时。
Based on the threat intelligence output, they must identify the missing pieces (gaps) in the organization's (consumer) defense system. -> 基于威胁情报输出，他们必须识别组织（消费者）防御系统中的缺失部分（差距）。
Organizations must expect clear and precise threat intelligence verdicts to use that to make critical decisions. -> 组织必须期望清晰而精确的威胁情报裁决，以便据此做出关键决策。
A typical conclusion statement could be as follows: -> 一个典型的结论陈述可以如下：
