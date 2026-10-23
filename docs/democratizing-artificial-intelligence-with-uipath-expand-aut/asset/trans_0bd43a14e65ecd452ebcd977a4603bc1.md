

## 评估统计

| 指标 | 值 |
|---|---|
| accuracy | 0.53353 |
| precision | 0.52848 |
| recall | 0.53353 |
| matthews correlation | 0.41685 |
| fscore | 0.5275 |
| auroc | 0.8411 |

Figure 7.8 – 示例评估报告

Figure 7.8 是文本分类模型评估的一个示例。我们通常希望 Fscore 达到大于 70%。在这种情况下，我们低于 70%，因此我们应该检查训练和评估数据集的正确性，并研究编辑模型参数或选择完全不同的模型。

一旦我们选择了模型并成功评估它，我们就可以使用测试数据集对模型进行测试。该数据集应完全无偏，并且包含模型之前从未见过的新数据。用于测试的最佳数据类型是年轻数据，最好来自我们的生产环境。

## 执行 UAT

在之前的章节中，我们讨论了在各自独立的 silo 中测试 RPA 和 ML。这在自动化生命周期的开发阶段效果很好，但在部署到生产环境之前，这两种技术应该有一个融合。这个融合应该发生在自动化生命周期的 UAT 阶段。

UAT 是一种测试类型，用于让最终用户或业务对自动化的性能给予最终批准。这种测试在将自动化部署到生产环境之前进行，向最终用户展示自动化在给定一组场景下的预期表现。在此测试期间，我们不关注选择器等特定错误，而是关注端到端流程，确保自动化的输出满足最终用户/业务的要求。

下图说明了 UAT 如何融入自动化生命周期：

![](img/05268897c8ac5d80eb3fead1cc8a822a_202_0.png)

Figure 7.9 – 自动化生命周期中的 UAT

为了执行 UAT，在最终测试之前有几个入口点需要作为标准考虑。这些入口标准包括以下内容：

- **开发测试已完成**：自动化开发应经过测试（单元、集成和系统测试），并且在验收测试之前应已通过端到端测试。
- **同行评审已完成**：自动化开发应由另一名开发人员进行同行评审，以符合最佳实践。
- **环境已准备好**：UAT 通常发生在与开发环境不同的环境中。该环境可以包括带有测试数据的 UAT 应用程序和 UAT UiRobot。
- **自动化已迁移**：自动化应在 UAT 测试之前发布到开发编排器、提交到源代码控制并迁移到 UAT 编排器。
- **UAT 测试计划已创建**：应由流程负责人或**主题专家（SME）**创建包含所有必要测试场景和预期结果的测试计划。
- **UAT 数据已收集**：与测试计划一起，应收集用于测试的 UAT 数据。通常，最近的生产数据最适合进行 UAT。

有关 UAT 的更多信息，您可以参考以下链接：

```
https://www.guru99.com/user-acceptance-testing.html
```

一旦所有入口标准都完成，UAT 测试就可以开始。执行 UAT 可以有几种形式，具体取决于业务是否运行 UAT 测试或开发团队为业务运行 UAT 测试以供审查。进行 UAT 的一种简单方法是让 SME 和开发团队一起开会，逐一运行每个测试场景。SME 可以监督测试，而开发人员可以从 UAT 编排器为每个测试场景启动自动化。

为 UAT 完成定义退出标准很重要。退出标准最好在 UAT 测试计划文档中定义，以便所有相关方都知道 UAT 完成的所需标准。您可以在以下屏幕截图中看到 UAT 测试计划文档的示例：

| # | 场景 | 预期结果 | Case ID | 状态 |
|---|---|---|---|---|
| 1 | - Market Data Rejects > 5% of total Integrated<br>- Market_Data_Bypass is not true | The robot:<br>- References Market Data portion of output email<br>- Sends Email:<br>  - To: team@company.com<br>  - Subject: UIP Alert: Market Data Rejects Over 5%<br>  - Body: Today's Kyriba Status Report shows a high amount of Rejections.<br>- END PROCESS | 1 | APPROVED |
| 2 | - Market Data Rejects > 5% of total Integrated<br>- Market_Data_Bypass is true | The robot:<br>- References Market Data portion of output email<br>- All other functions as expected; formats and distributes email as expected | 2 | APPROVED |
| 3 | - Mark to Market Report Missing | The robot:<br>- References Market Data portion of output email<br>- Filters for 'mtm' template<br>- Runs MTM Reporting using RPA _ FX MTM Generation template<br>- All other functions as expected; formats and distributes email as expected | 3 | APPROVED |
| 4 | - Mark to Market Report is 'Warning' | The robot:<br>- References Market Data portion of output email<br>- Filters for 'mtm' template<br>- Sends Email:<br>  - To: team@company.com<br>  - Subject: UIP Alert Check MTM Status<br>  - Body: 'Mark to Market status is: Warning - please investigate'<br>- END PROCESS | 4 | APPROVED |
| 5 | - Mark to Market Report is 'Error' | The robot:<br>- References Market Data portion of output email<br>- Filters for 'mtm' template<br>- Sends Email:<br>  - To: team@company.com<br>  - Subject: UIP Alert Check MTM Status<br>  - Body: 'Mark to Market status is: Error - please investigate'<br>- END PROCESS | 5 | APPROVED |

![](img/05268897c8ac5d80eb3fead1cc8a822a_203_0.png)

退出标准可能取决于组织，但通常，UAT 退出标准包括以下内容：

- No defects open
- Successful completion of UAT test scenarios
- Documented sign-off of UAT

为了满足这些退出标准，业务分析师、UAT 测试人员或 SME 应该记录每个已完成测试场景的结果。根据组织的不同，每个测试场景可以通过屏幕截图、视频录制或仅对自动化输出进行签核来记录。一旦满足退出标准，UAT 就可以完全完成，并且自动化可以部署到生产环境。

## 关闭反馈循环

随着认知自动化从 UAT 转移到生产环境，它可能会遇到训练、评估和测试测试集中未见过的数据。我们期望部署的模型能够基于所进行的训练处理新数据，但有时模型可能会返回不令人满意的结果，或者根据它遇到的新数据存在进一步改进模型性能的机会。

这就是关闭反馈循环可以在 ML 模型性能中发挥重要作用的地方。通过对 Document Understanding 或 AI Center ML 技能关闭反馈循环，我们可以捕获未见过的数据点，使用人工向 ML 技能发送反馈并持续用新数据训练该技能。您可以在以下屏幕截图中看到关闭反馈循环的表示：

![](img/05268897c8ac5d80eb3fead1cc8a822a_204_0.png)

Figure 7.11 – 关闭反馈循环

使用 UiPath，开发人员可以使用置信度阈值来允许自动化继续使用 ML 技能的输出，或提示用户通过使用 Attended Automation（Validation Station）或使用 Action Center 输入。此反馈循环现在使我们能够使用新鲜、相关的数据持续测试和完善我们的模型。

## 第 3 节：使用 UiPath Document Understanding、AI Center 和 Druid 构建

在本节中，我们将使用 UiPath Document Understanding、AI Center 和 Druid 连接构建实际用例，以将 UiPath 自动化与认知能力联系起来。

本节包含以下章节：

- Chapter 8, Use Case 1 – Receipt Processing with Document Understanding
- Chapter 9, Use Case 2 – Email Classification with AI Center
- Chapter 10, Use Case 3 – Chatbots with Druid
- Chapter 11, AI Center Advanced Topics

