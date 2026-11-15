
然而，仍有许多有趣的方向可用于扩展应用。

练习规格驱动工作流的最佳方法是持续添加小型、定义明确的功能。

以下是几个适合作为练习的想法。

## 练习想法

### 交互式地图位置问题

受访者不再从下拉列表中选择国家，而是可以在地图上选择他们的位置。

想象一个问题，例如：*您从哪里参与？*

界面可以显示一个地球仪或世界地图，用户点击他们的位置。

回答将存储纬度和经度坐标。调查分析随后可以在地理上显示回答，展示不同地区参与情况的聚类。

**提示**：设计一个使用地图界面的地理调查回答规格。

### 拖放排序问题

一些调查要求用户按偏好顺序对项目进行排序。

例如：*按您喜欢使用这些编程语言的程度对它们进行排序。*

拖放界面可以让受访者直观地重新排序项目：

1. Python
2. JavaScript
3. Go
4. Rust

结果将把排序顺序存储在数据库中。

这种问题类型引入了有趣的数据建模挑战。

**提示**：为调查应用中的拖放排序问题设计规格，并解释排序问题涉及的数据建模挑战。

### 调查分支逻辑

更高级的调查会根据之前的回答显示不同的问题。

示例：*您拥有汽车吗？*

- 是 → 显示关于驾驶习惯的问题
- 否 → 跳至公共交通问题

此功能在调查引擎中引入了条件逻辑，并迫使您仔细思考问题和回答的结构方式。

**提示**：为向调查系统添加分支逻辑设计规格。

### 实时回答仪表板

调查创建者可能希望在参与者作答时实时看到回答更新。

这可以使用 WebSockets、Django Channels 或实时更新图表来实现。

实时观看回答累积会让应用程序感觉更加动态。

**提示**：解释如何使用 WebSockets 创建实时调查回答仪表板。

### 调查主题和样式

另一个视觉改进将允许调查创建者自定义其调查的外观。

可能的功能包括：

- 配色主题
- 自定义徽标
- 浅色和深色模式

这将给您一个练习 CSS 自定义属性、主题类和浅/深色样式的理由。

**提示**：请展示如何为 Django Web 应用程序添加主题支持。

## 练习、参考和资源

### 检查您的理解

回答以下问题：

1. 什么是规格驱动开发？为什么在使用 AI 编码助手时它很有用？
2. 为什么在使用 AI 工具时，编写规格比手动编码时更重要？
3. 良好的功能规格的关键部分有哪些？
4. 为什么非目标是规格的重要组成部分？
5. 什么使验收标准有效？
6. 测试如何将规格转化为可执行的契约？
7. Spec Kit 在这个工作流中的作用是什么？
8. Spec Kit 工作流的主要阶段有哪些？
9. 为什么规格驱动开发可能被认为对于小变更是大材小用？
10. 为什么即使测试通过，您也应该审查 AI 生成的代码？
11. 您要求 AI 工具在没有规格的情况下“添加一个有用的功能”。结果包含了您不想要的额外功能。为什么会发生这种情况？
12. 您编写了规格但跳过了非目标部分。实现变得过于复杂。哪里出错了？
13. 您的验收标准说“UI 应该用户友好”。为什么这是个问题？
14. 任务列表包含一个大项目：“实现 URL 短链接”。为什么这是个问题？
15. 您在未审查规格或计划的情况下运行 `/speckit.implement`。可能的结果是什么？
16. 您完成实现且测试通过，但跳过了手动测试。还存在什么风险？

答案可在本书的 GitHub 存储库中找到，链接如下：

[https://github.com/markjprice/web-dev-ai/blob/main/docs/Appendix-A.pdf](https://github.com/markjprice/web-dev-ai/blob/main/docs/Appendix-A.pdf)

### 链接

以下是一些有用的链接。

- 规格驱动开发指南：[https://github.com/github/spec-kit/blob/main/spec-driven.md](https://github.com/github/spec-kit/blob/main/spec-driven.md)
- GitHub 工程博客：与 AI 一起的规格驱动开发：[https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/)
- 使用 Markdown 作为编程接口的规格驱动开发：[https://github.blog/ai-and-ml/generative-ai/spec-driven-development-using-markdown-as-a-programming-language-when-building-with-ai/](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-using-markdown-as-a-programming-language-when-building-with-ai/)
- Martin Fowler：探索规格驱动开发：[https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html](https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html)
- Django 重定向快捷方式文档：[https://docs.djangoproject.com/en/stable/topics/http/shortcuts/#redirect](https://docs.djangoproject.com/en/stable/topics/http/shortcuts/#redirect)
- Django 重定向指南：[https://realpython.com/django-redirects/](https://realpython.com/django-redirects/)

## 总结

在本章中，您使用规格驱动开发为现有的 Django 项目添加了一个新功能。

您看到了书面规格如何给 AI 编码助手提供更清晰的边界，特别是当功能影响模型、视图、路由、模板和测试时。

您学习了如何定义目标、非目标、用户故事、验收标准、数据模型变更和验证步骤。

您还看到了 Spec Kit 等工具如何将该规格转化为一个分阶段的过程：项目原则、功能规格、实现计划、任务列表、实现、审查和改进。

最重要的是，您练习了 AI 无法为您完成的开发部分：决定想要什么行为、什么行为超出范围，以及如何证明系统工作正常。

具体来说，您学习了以下内容：

- 什么是规格驱动开发，以及为什么它对 AI 编码助手有用
- 如何构建功能规格
- 如何使用书面规格扩展现有的 Django 项目
- 如何为 TallyApp 添加 URL 短链接功能
- 如何验证实现是否满足定义的验收标准
- 小型、定义明确的功能如何使大型系统更容易演进

在下一章中，您将学习如何将 Web 项目部署到主机服务器，使其公开可用，以便其他人可以访问它。

## 获取本书的 PDF 版本和更多内容

扫描二维码（或访问 packtpub.com/unlock）。按名称搜索本书，确认版本，然后按照页面上的步骤操作。

![](Images/B34553_14_04.png)![](Images/B34553_11_12.png)

*注意：请保留发票。直接从 Packt 购买不需要发票。*

# 第 15 章：部署到公共服务器
