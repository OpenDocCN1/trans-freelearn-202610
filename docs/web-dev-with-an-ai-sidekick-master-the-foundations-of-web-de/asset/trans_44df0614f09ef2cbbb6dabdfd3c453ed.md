
# Django 使用测试客户端进行集成测试

```python
from django.contrib.auth import get_user_model
from django.test import TestCase
from django.urls import reverse
from surveys.models import Survey

User = get_user_model()

class SurveyCreatePostIntegrationTests(TestCase):
    @classmethod
    def setUpTestData(cls):
        cls.user = User.objects.create_user(
            username="creator",
            email="creator@example.com",
            password="pass12345",
        )

    def test_post_creates_survey_and_redirects(self):
        self.client.force_login(self.user)
        url = reverse("surveys:create")
        response = self.client.post(
            url,
            data={
                "title": "Conference Feedback",
                "slug": "conference-feedback",
                "is_published": False,
            },
            follow=False,
        )
        self.assertEqual(response.status_code, 302)
        created = Survey.objects.get(slug="conference-feedback")
        self.assertEqual(created.title, "Conference Feedback")
        self.assertFalse(created.is_published)
        # Optionally verify where we redirect to
        expected = reverse("surveys:detail", kwargs={"slug": created.slug})
        self.assertRedirects(
            response,
            expected,
            fetch_redirect_response=False,
        )
```

如果您想在重定向后验证最终页面内容，请传递 `follow=True` 并对最终响应进行断言。Django 文档描述了 `follow=True` 和 `redirect_chain` 属性：

```python
response = self.client.post(url, data=payload, follow=True)
self.assertEqual(response.status_code, 200)
self.assertGreaterEqual(len(response.redirect_chain), 1)
self.assertContains(response, "Conference Feedback")
```

## 实用指南

保持集成测试稳定：

* 首选命名 URL（`reverse()`）而非硬编码路径。这能减少重构时的脆弱性。
* 对于跨测试共享的昂贵设置，使用 `setUpTestData`；对于您打算修改的每个测试的状态，使用 `setUp`。
* 仅断言足以证明行为的最小集合。例如，断言重定向和数据库行存在，而不是断言整个 HTML 输出。
* 如果测试失败，按以下顺序诊断：
    1. 是否命中了正确的 URL 名称和参数？
    2. 当您期望 `200` 时是否收到了重定向（`302`）？
    3. 认证状态是否存在（`force_login` 或 `login`）？
    4. 您的数据库设置是否与视图预期的一致？

> **提示**：请解释如何在 Django 中正确测试数据库约束和唯一索引，以及竞态条件或并发问题。

## 集成测试的结束与端到端测试的开始

测试客户端永远不会运行 JavaScript，它永远不会验证按钮在真实浏览器中是否可点击，也不会发现布局问题。这是基于 Playwright 的端到端测试的工作。但对于 Django 中的大多数服务端工作流，集成测试能在耗时范围内给您最好的反馈。

现在让我们转到端到端测试，也称为用户界面测试。

# 端到端 Web 用户界面测试

单元测试和 Django 的测试客户端能给您快速反馈，但它们仍无法证明 TallyApp 在真实浏览器中能正常工作。Web UI 测试位于测试金字塔的顶端：您运行应用程序，打开真实的浏览器引擎，并自动化用户执行的相同操作，如点击链接、填写表单、提交以及验证屏幕上出现的内容。

本节使用 Playwright，这是一个快速、可靠且支持 Chromium、Firefox 和 WebKit 的浏览器自动化工具。我们将用它来测试 TallyApp 的少量重要端到端用户旅程。

## UI 测试的用途

UI 测试最适合用于绝不能中断的关键流程：

* **认证流程**：
    * 未登录用户访问受保护页面时会被重定向到登录页
    * 用户可以登录和登出
* **核心产品流程**：
    * 已登录用户可以创建调查
    * 新调查出现在调查列表中
    * 用户可以编辑和删除自己的调查
* **授权边界**：
    * 用户 A 无法编辑或删除用户 B 的调查（通过尝试直接导航来测试）

避免对所有功能都使用 UI 测试。它们比单元测试慢，且维护稳定性需要更多工作。您的目标是一套小型测试套件，从用户角度证明应用程序正常工作。

## 为什么选择 Playwright 而不是 Selenium

历史上，Selenium 一直是最流行的 Web UI 测试框架。Selenium 可以工作，但 Playwright 通常更适合现代项目，因为：

* **内置自动等待**。它会等待元素就绪，从而减少不稳定测试。
* **定位器具有弹性**。它们会重新查询并跟踪元素，而不是一次性查找然后祈祷。
* **出色的调试功能**。它可以录制屏幕截图、视频和追踪记录。
* **跨浏览器**。一个 **应用程序编程接口** (**API**) 统一驱动 Chromium、Firefox 和 WebKit。
* **更友好地支持 AI 编码代理**。Playwright **模型上下文协议** (**MCP**) 服务器 (`playwright-mcp`) 与 GitHub Copilot 集成良好。

> **提示**：请从架构和可靠性方面比较 Playwright 和 Selenium。

## 让您的 UI 易于测试

如果您养成以下两个习惯，UI 测试的可靠性将大幅提升：

* **优先使用可访问性选择器**：Playwright 可以通过角色、标签和可见文本定位元素。这鼓励编写良好的 HTML，并使测试更不易碎。
* **谨慎使用测试 ID**：为少数关键元素添加 `data-testid="..."` 属性。这可以防止样式更改破坏测试。

Django 模板中的示例：

```html
<button type="submit" data-testid="save-survey">Save</button>
```

谨慎使用此方法。大多数情况下，优先使用标签和角色。

## 为 Django 项目选择 Playwright 风格

Playwright 支持两种常见方法：

1. **Python + `pytest`**。天然适合 Django/Python 仓库。测试看起来像 Python 测试，并可与其他测试工具一起运行。
2. **TypeScript 运行器 (Playwright Test)**。如果您的团队已经大量使用 TypeScript，这非常出色。TallyApp 不需要这个。

对于这个以 Django 为主的书籍项目，Python 配合 `pytest` 是最简单且最一致的选择。

## 安装 Playwright

您需要两样东西：

* Python Playwright 包
* Playwright 使用的实际浏览器二进制文件

典型设置命令（在虚拟环境中运行）：

```bash
python -m pip install playwright pytest
python -m playwright install
```

可选但推荐，用于更好的断言和等待助手：

```bash
python -m pip install pytest-playwright
```

如果您已经对单元测试使用 `pytest`，这非常契合。如果没有，您仍可以单独运行 Playwright 测试，但 `pytest` 能让事情保持整洁。

## 在 Git 仓库中组织 UI 测试

整洁的结构是将 UI 测试与 Django 单元测试分开：

* `tests/` 用于单元测试和集成测试
* `e2e/` 用于 Web 用户界面测试

示例：

* `e2e/test_auth_flow.py`：网站认证的测试
* `e2e/test_surveys_crud.py`：调查数据库 CRUD 操作的测试

这可以避免对哪些测试需要运行中的服务器和浏览器产生混淆。

## 为 UI 测试运行 Django

UI 测试需要一个运行中的服务器。您有两种合理的方法：

1. **手动运行 Django 开发服务器**。在一个终端启动服务器，然后在另一个终端运行 Playwright 测试：

```bash
python manage.py runserver
```
