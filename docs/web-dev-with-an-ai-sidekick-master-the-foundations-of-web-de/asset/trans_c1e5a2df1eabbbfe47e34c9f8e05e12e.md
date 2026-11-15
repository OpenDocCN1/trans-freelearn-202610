
```
Write a test that verifies creating a survey fails when the user is not approved. Use self.client and assert the correct redirect or error message.
```

这将 AI 从幻觉行为转变为实现你已定义的行为。

## 使用 AI 解释失败的测试

当测试在 CI 中失败时，将失败输出粘贴到 AI 助手中可能会出奇地有效。

示例提示词：

```
Here is the traceback from my Django test run. Explain what is happening and suggest likely causes. Do not rewrite my code, just diagnose.
```

AI 通常可以：

* 识别出发生了重定向，而不是返回 `200`。
* 注意到对象不存在，因为设置数据不完整。
* 发现 URL 名称与 `reverse()` 用法之间的不匹配。
* 识别出由新模型约束触发的验证错误。

这对初学者尤其有帮助，他们往往难以解读涉及中间件、URL 解析和数据库错误的多层 Django 回溯。

## 使用 AI 重构脆弱的测试

AI 对于清理过于冗长或脆弱的测试也很有用。

假设你向它展示如下测试：

* 硬编码的 URL 路径。
* 重复的用户创建样板代码。
* 手动登录逻辑。
* 多个测试中重复的模型设置。

在代码编辑器中使用代理模式时的示例提示词：

```
Refactor these Django integration tests to reduce duplication. Extract factories or helper methods. Avoid relying on hardcoded URLs.
```

AI 可以：

* 用 `reverse()` 替换字面路径。
* 引入 `make_user` 辅助函数。
* 在不测试登录视图的测试中，使用 `force_login` 代替手动 `POST` 登录。
* 将重复的设置合并到 `setUp` 或 `setUpTestData` 中。

AI 在这里很有用，因为任务是机械性的：在不改变行为的情况下减少重复。

## 浅层 AI 生成测试的风险

最大的危险不是语法错误，而是虚假的信心。

AI 生成测试的常见问题如表 12.3 所示：

| 问题 | 解释 |
| --- | --- |
| 它们仅断言 `status_code == 200`。 | 页面可能返回 `200` 但仍然是错误的。 |
| 它们断言与业务规则无关的文本存在。 | 例如，断言页面上出现了 `Survey`，而不是验证正确的调查标题出现而错误的未出现。 |
| 它们镜像实现而非测试行为。 | 如果你的视图调用 `Survey.objects.filter(is_published=True)`，AI 可能会生成一个断言完全相同查询的测试，而不是断言未发布的调查不可见。 |
| 它们忽略负面情况。 | AI 倾向于生成正向路径测试，除非你显式要求失败模式。 |
| 它们依赖脆弱的细节。 | 硬编码的主键、隐式排序，或假设 ID 从 1 开始。 |
| 它们过度拟合当前代码。 | 如果你稍后重构内部逻辑但保留行为，浅层测试可能会不必要地中断，因为它们测试的是实现细节。 |

表 12.3：AI 生成测试的常见问题

**提示词：** 请展示薄弱的 AI 生成测试示例以及如何改进它们。

## 如何批判性地审查 AI 生成的测试

将 AI 输出视为来自一位热情但缺乏经验的开发者的拉取请求。以同样的怀疑态度审查它。

问自己这些问题：

1. 这个测试证明了什么行为？
2. 如果业务规则被破坏，这个测试会失败吗？
3. 这是在断言有意义的结果，还是仅仅断言页面加载了？
4. 它是在测试实现细节而不是可观察的行为吗？
5. 它是否覆盖了成功和失败两条路径？
6. 它是否依赖关于 ID 或排序的脆弱假设？
7. 设置是否最小化且清晰？

如果你在故意破坏预期行为后测试仍然通过，那它就不是一个好测试。

一个有用的练习是：

1. 接受 AI 生成的测试。
2. 故意暂时破坏功能。
3. 运行测试。
4. 确认它因正确的原因而失败。

如果它不失败，测试就是薄弱的。

## AI 在测试中最有价值的场景

AI 最有助于：

* 生成边缘情况列表
* 起草重复的脚手架代码
* 建议你可能遗漏的额外断言
* 解释复杂的回溯
* 重构测试重复代码

它最不可靠时：

* 解释模糊需求
* 推断安全规则
* 猜测业务逻辑
* 在没有明确约束的情况下从头设计测试策略

测试是可执行的规范。AI 可以帮你更快地编写它们，但它无法决定你的系统应该保证什么。

AI 生成的测试看起来令人印象深刻，运行通过（绿色），却可能什么重要的东西都没断言。

审慎使用，AI 有帮助；懒惰使用，它只会给你更多代码去审查。区别不在于模型，而在于你，人类审查者。

**提示词：** 请解释如何设计提示词以生成更高质量的自动化测试。

# 练习、参考资料和资源

在本节中，你将接受本章所学内容的测试，并获得有用的参考资料和资源链接。

## 检查你的理解

回答以下关于本章涵盖主题的问题：

1. 测试金字塔的目的是什么？
2. 什么造就了一个好的单元测试？
3. Arrange–Act–Assert（准备-执行-断言）模式描述了什么？
4. 为什么不稳定测试被认为比没有测试更糟？
5. Django 的 `TestCase` 类的作用是什么？
6. 在 Django 中，单元测试和集成测试有什么区别？
7. 为什么 UI（端到端）测试应谨慎使用？
8. 使用工厂相对于固定装置的主要优势是什么？
9. 持续集成 (CI) 为项目做什么？
10. AI 生成测试的最大风险是什么？

答案可在本书的 GitHub 仓库中找到，链接如下：

[https://github.com/markjprice/web-dev-ai/blob/main/docs/Appendix-A.pdf](https://github.com/markjprice/web-dev-ai/blob/main/docs/Appendix-A.pdf)

## 链接

Martin Fowler – 测试金字塔。对测试金字塔以及单元、集成和 UI 测试之间权衡的现代阐述。重要的概念基础：

[https://martinfowler.com/articles/practical-test-pyramid.html](https://martinfowler.com/articles/practical-test-pyramid.html)

Google 测试博客 – 测试在厕所和测试策略。来自大规模工程团队的实用指南。对不稳定性和 CI 纪律有深刻见解：

[https://testing.googleblog.com/](https://testing.googleblog.com/)

Django 测试概览。Django 的测试文档异常强大：

[https://docs.djangoproject.com/en/6.0/topics/testing/overview/](https://docs.djangoproject.com/en/6.0/topics/testing/overview/)

`pytest` 文档：

[https://docs.pytest.org/](https://docs.pytest.org/)

`pytest-django` 文档：

[https://pytest-django.readthedocs.io/](https://pytest-django.readthedocs.io/)

Playwright 文档：

[https://playwright.dev/python/docs/intro](https://playwright.dev/python/docs/intro)

# 总结

在本章中，你为 TallyApp 构建了一个实用的测试策略。你学习了为什么测试金字塔倾向于许多快速的单元测试、较少的集成测试，以及仅有少量的浏览器级端到端测试。

你学习了以下内容：
