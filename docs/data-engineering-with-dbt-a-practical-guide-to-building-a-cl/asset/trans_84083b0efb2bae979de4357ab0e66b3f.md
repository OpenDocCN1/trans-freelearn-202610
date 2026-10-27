

通过串联 STG 模型、HIST 模型以及 current_from_history 宏，你现在已经拥有了所有拼图碎片，可以构建一个简单且高度 resilient 的加载管道，该管道能够以高效且高度可扩展的方式调整、存储和提供全范围的入站数据以供业务使用。

在上一章中，我们将研究用于摄取数据并在具有更高级需求的情况下存储历史的模式，例如跟踪删除以及每次加载运行中的多次更改。

但在此之前，在下一章中，我们将利用新获得的知识重构我们的示例项目，并使用 save_history 宏创建 HIST 模型，添加更多数据并实现一些业务规则。

## Summary

在本章中，你学习了如何基于经过深思熟虑的键构建模块化管道，这些键反映了实体的期望语义。

你学习了如何组合来自不同系统的实体，以及如何根据其语义以简单有效的方式存储其更改。

凭借本章获得的知识，你现在能够构建现代、可维护且可扩展的数据平台，其中包含数百个实体，以及主表中数亿甚至数十亿行数据。

在下一章中，我们将完成示例项目并用它来讨论在交付真实项目时派上用场的几个更多主题。

## 14

## Enhancing Software Quality

在本章中，你将发现并应用更高级的模式，以在真实项目中提供高质量的结果，并将尝试如何通过重构自信地演进代码。

通过围绕我们示例项目的一些小型用例的选择，你将学习如何以非常高效的方式保存实体更改的历史，如何从源中检测已删除的行，以及如何使用窗口函数利用存储在 HIST 表中的数据来随时间分析数据演变。

在本章的最后一部分，你将创建并应用一个宏，以使用自完成功能的维度正确处理事实中的孤儿键，从而为你的数据集市生成质量更好的事实和维度。

在本章中，你将学习以下主题：
- Refactoring and evolving models
- Implementing real-world code and business rules
- Publishing dependable datasets

## Technical requirements

本章基于前几章讨论的概念、示例项目和宏。

本章的代码示例可在 GitHub 上获取，地址为 https://github.com/PacktPublishing/Data-engineering-with-dbt/tree/main/Chapter_14。

## Refactoring and evolving models

我们务实数据平台的核心思想之一是根据最小惊讶原则生产干净的代码和干净的结构，这反过来促进更快的发展、高度可维护性并降低开发人员压力。

为什么？因为数据库是任何公司中最长寿的应用程序，它们需要长期管理。因此，我们的管道需要随着业务变化、源系统变化，有时也为了合规或其他法律原因而变化和适应。

也许违反直觉的是，我们的数据产品越成功，我们收到的添加更多功能的要求就越多。所有更改的时间和成本直接受制于必须应用更改的代码的质量。

在这一漫长旅程中，我们将有两项活动来保持项目相关性：
- **Evolution:** 这涉及添加新功能并保持现有功能按预期工作。不要把这句话的后半部分视为理所当然。

  演进应利用模块化，在已有内容之上构建新概念，并始终为每个概念提供唯一的真相。
- **Refactoring:** 重构“*是一种有纪律的技术，用于重构现有代码体，在不改变其外部行为的情况下改变其内部结构*”，正如 Refactoring.com 主页上 Martin Fowler 所说，他是我崇拜的编码英雄之一。

  重构的核心是进行多次小的更改，保持项目正常运行，在每次小更改后进行测试，最终用保持按预期工作的更好代码替换初始代码。

在本章中，我们将首先重构现有代码，用我们最初使用快照的实现替换为在上一章中介绍的并作为长期解决方案更偏好的 HIST 表实现。

本章后面，我们将添加一些更多功能，这些功能都经过了第一个可工作但粗糙的版本，然后重构为更好的解决方案。

你现在已经知道我们需要使用以开始将示例项目转变为简单、微小、不完整但逼真的项目所需的所有核心概念。它永远无法接近真实的东西，但我们希望我们拥有的东西是做得很好的。

## Dealing with technical debt

在演进代码时，第一个目标和正常程序是创建一个可行的解决方案，并且包含检查我们交付内容的核心测试。

在正常情况下，最佳解决方案是在正常开发工作中不断清除少量技术债务，保持正常态度，使我们碰巧工作的代码保持良好状态。这通常被称为“童子军规则”，它规定如果你能做到，就应该让停留的地方比你找到时更好。

在本节中，我们了解到重构是交付项目的重要组成部分。那么，你的重构目标应该是什么？我们认为易读的代码、干净的架构和性能足够好的查询是重要的。

## Tools for refactoring in dbt projects

让我们看看我们在项目中重构的基本工具：
- **Code readability and organization:** 我们通过为列、CTE 和模型提供良好的名称使代码更易于维护，并通过引入一次只执行一个步骤的 CTE 以及将大型模型拆分为一次实现一个目标的较小模型来改善其组织。
- **Tests:** 通用测试提供了第一道防线，以保证我们接收和生成的数据质量良好并符合预期。

  单个测试可以提供业务规则正确性的定点验证，检查转换结果是否为预期结果。

  初始解决方案上通过的测试必须在每次重构后继续通过。
- **Data auditing:** 一旦我们使初始解决方案正常工作，我们可以保存已验证的结果，然后在开发环境中保持输入冻结的同时，将其与每次重构步骤的结果进行比较。

  审计更改的过程可通过 `audit helper` 包大大简化：https://hub.getdbt.com/dbt-labs/audit_helper/latest/。

我们已经看到，为了保持项目健康，你必须不断演进它以提供新功能，同时不断重构它以保持代码整洁。

在下一节中，我们将对示例项目中的代码进行一些增强，包括重构，我们将从一个可以改进的解决方案开始。

## Implementing real-world code and business rules

在本节中，我们将提供许多真实代码示例，用真实项目代码替换初始示例代码并实现一些业务逻辑，即业务规则，并添加一些额外数据，展示更改是如何存储的。

第二，我们将用 `REF_ABC_BANK_SECURITY_INFO` 模型中的 `HIST_ABC_BANK_SECURITY_INFO` 表替换 `SNSH_ABC_BANK_SECURITY_INFO` 快照：
1. 在暂存文件夹中创建一个名为 `HIST_ABC_BANK_SECURITY_INFO` 的新文件并添加以下代码：

```
{{ save_history(
    input_rel = ref('STG_ABC_BANK_SECURITY_INFO'),
    key_column = 'SECURITY_HKEY',
    diff_column = 'SECURITY_HDIFF',
) }}
```

输入代码并点击 **Save** 按钮。然后，点击 **Build** 按钮或运行 `dbt build -s HIST_ABC_BANK_SECURITY_INFO` 命令。
2. 编辑 `REF_ABC_BANK_SECURITY_INFO` 模型，使其使用 HIST 表作为输入代替快照。

以下是 `current_from_snapshot` CTE：

```
current_from_snapshot as (
    {{ current_from_snapshot(
        snsh_ref = ref('SNSH_ABC_BANK_SECURITY_INFO'),
    ) }}
)
```

将其替换为 `current_from_history` CTE，如下所示：

```
current_from_history as (
    {{ current_from_history(
        history_rel = ref('HIST_ABC_BANK_SECURITY_INFO'),
        key_column = 'SECURITY_HKEY',
    ) }}
)
```

完成后，保存并构建模型。

你可以在本书关联的存储库中的 *Chapter 14* 材料中找到使用 HIST 模型的完整 REF 文件代码。

我们现在已经创建并部署了两个 HIST 模型，并重构了 REF 模型以使用来自 HIST 模型的数据。现在，血缘图如下所示：

![](img/8868d272a75eee8b407a8e0f3434d10f_484_0.png)

Figure 14.1: Lineage graph after the last refactoring

