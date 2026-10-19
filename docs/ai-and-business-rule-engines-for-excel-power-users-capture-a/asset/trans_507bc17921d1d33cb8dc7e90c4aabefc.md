

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_331_0.png)

图12.5 – 组合机器学习与决策模型

5. 请注意，这个文件夹中还有其他几个模型，但我们选择了 `KiePMMLRegression.dmn`，因为它与我们第11章训练的模型最接近。该决策服务有三个输入字段，然后调用内嵌的机器学习模型，基于输入进行预测。

> **VS Code 与 KIE Sandbox 的区别**
> 在 VS Code 中运行决策模型与 KIE Sandbox 中有所不同。例如，没有 **Run... as Form** 按钮来试用模型。取而代之的是，我们可以编写 Java 代码来调用模型（本章稍后我们将展示一个使用 Red Piranha 项目的示例），或者使用我们在第9章和附录B中遇到的场景测试工具。

6. 场景测试是更简单的选择，特别是在 `KiePMMLRegressionTest.scesim` 文件中已经设置好了一个场景。你可以通过左侧的文件夹结构（`test/resources/`）进行导航，也可以按 Ctrl + P 并输入文件名快速跳转到该文件。场景测试非常简单——浅蓝色的前三列是输入值，第四列是我们期望的预测值。如图12.6所示：

| 场景描述 | 给定：fld1（值） | 给定：fld2（值） | 给定：fld3（值） | 期望：决策（值） |
|---|---:|---:|---|---:|
| 第一种情况 | 3.0 | 2.0 | "y" | 52.5 |
| 第二种情况 | 3.0 | 2.0 | "x" | 46.5 |
| 第三种情况 | 4.0 | 3.0 | "y" | 89.5 |
| 第四种情况 | 4.0 | 3.0 | "x" | 83.5 |

图12.6 – 示例场景测试

7. **图12.7** 展示了以 PMML 保存的机器学习模型的一个片段，Kogito 将在测试中运行该模型。与其他分类器一样，PMML 可以非常紧凑，文件中大部分内容是输入和输出字段的详细定义。

```xml
<RegressionModel algorithmName="linearRegression" functionName="regression">
  <MiningSchema>
    <MiningField name="fld1"/>
    <MiningField name="fld2"/>
    <MiningField name="fld3"/>
    <MiningField name="fld4" usageType="predicted"/>
  </MiningSchema>
  <Output>
    <OutputField name="result" targetField="fld4" />
  </Output>
  <RegressionTable intercept="0.5">
    <NumericPredictor coefficient="5" exponent="2" name="fld1"/>
    <NumericPredictor coefficient="2" exponent="1" name="fld2"/>
    <CategoricalPredictor coefficient="-3" name="fld3" value="x"/>
    <CategoricalPredictor coefficient="3" name="fld3" value="y"/>
  </RegressionTable>
```

图12.7 – PMML 回归模型片段

8. 在终端中，我们需要导航到演示机器学习与决策模型组合的特定示例。最快的方式是在终端（屏幕右下角的大文本框区域）输入 `cd kogito-quarkus-examples/dmn-quarkus-example/`。你可以在*图12.5*中看到它。

9. 在运行测试之前，请确认我们位于正确的文件夹中。在终端中输入 `ls`，检查返回的文件列表中是否包含 `pom.xml` 构建文件。

10. 运行我们的组合决策模型就像我们在*附录B*中演示的场景测试一样。输入 `mvn clean test` 来执行此模型中已有的场景测试。

11. 与之前的示例一样，首次运行时，许多支持库将被下载到镜像中。随着每个测试的运行和通过（这也是我们所预期的），你会收到更新。

12. 你将看到运行的多个测试的摘要，以及详细报告（位于 **Surefire** 报告文件中，这些文件将保存到新创建的 Target 文件夹中）。

正如预期的那样，一切都很顺利。Kogito 使用测试中的参数执行了我们的组合模型，输出与我们场景每一行的预期结果完全匹配。

> **这里没有意外**
> 
> KIE 团队在发布代码之前会自动运行这些测试，所以如果任何测试失败，那才是令人意外的。
> 
> 不过，偶尔会出现 Codespaces 比 KIE 团队更早升级到最新版本 Maven 的情况。这通常不应该导致问题，但偶尔确实会引发 Maven 构建错误。如果你不幸遇到这个问题，你有以下选择：
> 
> - 等待。团队升级到最新版本的 Maven 后会解决问题。通常这很快——几天而不是几周。
> - 在你的笔记本电脑上安装 VS Code。虽然这很简单，而且我们在*附录B*中给出了提示，但你可能需要技术支持。
> - 更改 Codespaces 使用的 Maven 版本。通常，你会希望使用比当前安装版本稍旧一些的版本。说明见 `https://maven.apache.org/install.html`，但同样，我们建议寻求更有技术经验的同事的支持。

## 接下来做什么——探索其他模型并构建自己的模型

既然我们已经更加适应了这种更具技术性的规则编辑环境，你可能会想阅读以下文章，其中包含了对我们刚刚运行的示例的更多详细信息：`https://blog.kie.org/2021/01/how-to-use-a-pmml-file-in-dmn-editor-vscode.html`。

你可能还想进一步探索测试项目。这个文件夹中还有其他值得探索的决策模型和单元测试。最后，值得拿一个你自己训练的机器学习模型，按照示例让 KIE 和 Kogito 来执行它。

我们承诺了两种更简单的模型执行方式，可能更适合那些不想使用 VS Code 的读者。让我们快速了解一下 Business Central，它与我们刚刚运行的示例非常相似。

## 在 Business Central 中执行机器学习与规则模型

在第9章中，我们看到 Business Central 有许多我们当时没有时间去探索的功能。其中之一就是编辑和执行组合机器学习与规则模型的能力。由于其方法（基于场景模拟测试工具和决策模型编辑器）与我们在上一节中使用 VS Code 所介绍的内容非常相似，我们就不提供详细的操作步骤了。

如果你对这个领域感兴趣，或者想要一些有用信息来帮助你在 VS Code 中更好地执行模型，以下几点可能有助于你进一步探索：

- - Red Hat 有两篇关于 Business Central 中 PMML 模型的好文章，从 `https://developers.redhat.com/blog/2021/01/14/knowledge-meets-machine-learning-for-smarter-decisions-part-1#artificial_intelligence__machine_learning__and_data_science` 开始。
- - KIE 团队在 YouTube 上有一个很棒的视频，覆盖了类似主题——搜索 `[KIELive#42] The Intelligent Decisions journey with DMN and PMML`。
- - Drools 文档中的 Pragmatic AI 部分包含了一个使用 Business Central 作为编辑器的复杂房屋贷款示例的详细演练：`https://docs.drools.org/8.29.0.Final/drools-docs/docs-website/drools/pragmatic-ai/index.html`。

Business Central 和 VS Code 在线方法有很多共同点。我们也值得介绍一个完全不同的替代方案，它更侧重于工作流，许多人会觉得这是最容易的。

## 替代方案——在 Power Automate 流中集成规则和机器学习

我们组合规则和机器学习决策服务的另一种方法非常简单，基于我们在*第8章*中介绍的 Power Automate 工具。*图12.8* 展示了我们设想的流程：

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_334_0.png)

图12.8 – 集成了机器学习与规则的 Power Automate 流

这个流程可以在 Power Automate Online 或 Power Automate Desktop 中实现。无论哪种方式，该流程在概念上都非常简单，只有五个步骤：

1. 我们以正常方式启动流程——此流程有一个手动触发器，但它也可以轻松地是在线表单提交。
2. 在第二步中，我们从 Power Automate 调用我们的机器学习模型（并且只调用机器学习模型）。
3. 我们将机器学习模型的结果更新到 Excel 中。
4. 我们使用第6、7和8章中介绍的任一种技术，单独调用基于规则的决策服务。我们确保传入所有信息，包括之前从机器学习模型收集到的结果。
5. 我们用最终决策服务调用的结果更新 Excel。

> **别担心——我们很快就会介绍如何做到这一点**
> *第2步是我们目前唯一还没展示的步骤——它相当简单，你会在下一节中看到。*

如果我们对巧克力棒推荐服务采用这种方法，第2步可能会向美国客户推荐花生脆棒，而第4步则进行基于规则的健康检查，以避免对任何有过敏症的人造成伤害。

许多 Excel 高级用户发现这种 Power Automate 方法比使用 VS Code 更容易——不仅因为工具在他们掌控之中，而且将事情分成简单、独立的步骤也更容易理解和测试。既然我们已经知道如何实现其他步骤，让我们专注于如何从 Power Automate 调用机器学习模型。

## 从 Power Automate 调用机器学习模型

从 Power Automate 操作中调用机器学习模型的方式有很多种。我们在本章和前一章中大量使用了 Python，而 Power Automate 对 Python 有很好的支持，因此我们将采用这种方法。尽管有很多扩展脚本的方式，但我们会让 Python 脚本保持简单。当你在本书的 GitHub 网站上查看 `12_use_existing_pmml_model.py` 这个脚本时，它会如图12.9所示：

