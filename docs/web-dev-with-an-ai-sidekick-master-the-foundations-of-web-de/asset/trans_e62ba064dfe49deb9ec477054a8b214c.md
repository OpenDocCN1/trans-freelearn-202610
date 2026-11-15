

此设置可行，但需要协调。服务器必须在测试开始前已在运行，如果未运行，失败信息可能会令人困惑，因为 Playwright 只会提示连接失败。在 CI 等自动化环境中，你不会依赖两个手动终端。相反，你会将启动 Django 作为测试运行的一部分，既可以使用测试服务器（如 Django 的 `LiveServerTestCase`），也可以使用能自动启动和关闭服务器的测试运行器。这引出了第二种方案。

1. 从 `pytest` 启动测试服务器。这对 CI 更整洁，但需要稍多一点设置。

对于本书，手动启动开发服务器通常更容易，随后你可以再实现自动化方法。即使在开发期间，一个简单的改进是将两个步骤编写脚本一起执行。例如，你可以使用 Shell 脚本或任务运行器启动 Django，然后在服务器就绪后运行 Playwright 测试。这消除了手动协调两个终端的需要：

```
# 示例（类 Unix 系统）
python manage.py runserver &
pytest e2e
```

这是一个适合学习的简单方法，但在实际项目中，你会使用更健壮的流程，在运行测试前等待服务器就绪。

当新手在未运行服务器的情况下运行 Playwright 测试时，他们会看到连接被拒绝错误、超时和 `page.goto failed` 消息。这些错误没有一个明确地说：“你忘了启动 Django。”

在下面的示例中，假设服务器运行在：`http://127.0.0.1:8000`。

## 第一个 Playwright 测试：登录重定向

创建 `e2e/test_auth_flow.py`：

```
import re
from playwright.sync_api import Page, expect
BASE_URL = "http://127.0.0.1:8000"
def test_protected_page_redirects_to_login(page: Page) -> None:
    page.goto(f"{BASE_URL}/surveys/")
    # 预期我们最终会到达登录页面（URL 模式因项目而异）。
    expect(page).to_have_url(re.compile(r".*/login/?(\?.*)?$"))
    # 且登录表单应可见（根据你的模板调整标签/文本）。
    expect(page.get_by_label("Username")).to_be_visible()
    expect(page.get_by_label("Password")).to_be_visible()
```

注意：

* `expect` 提供自动等待和比手动睡眠更清晰的失败信息。
* 尽可能使用标签。如果你的表单字段没有标签，请添加。这能提高可访问性和可测试性。

**提示**：什么是页面对象模型（Page Object Model），我应该在 Playwright 中使用它吗？

## 测试真实用户旅程：创建调查

UI 测试应演练用户采取的相同步骤。为此，你需要一个已知的用户账户。

你可以通过几种方式创建测试用户：

* 在开发数据库中创建一个专用用户
* 使用设置脚本/固件自动创建用户
* 使用 Django 管理后台创建一次用户

最简单的是：创建一次用户（例如 `mark` / `password123`），在早期 UI 测试中使用它，随后再演进为自动化设置。为学习目的，手动创建测试用户没问题。在实际项目中，E2E 测试应以编程方式创建自己的数据。Django 的 `StaticLiveServerTestCase` 提供了一个带有干净测试数据库的测试服务器，或者你可以编写一个设置脚本，在每次测试运行前创建所需的测试用户。

创建 `e2e/test_surveys_crud.py`：

```
import os
from playwright.sync_api import Page, expect
# 从环境变量读取基础 URL，提供默认值。
BASE_URL = os.environ.get("BASE_URL", "http://127.0.0.1:8000")
USERNAME = "mark"
PASSWORD = "password123"
def login(page: Page) -> None:
    page.goto(f"{BASE_URL}/login/")
    page.get_by_label("Username").fill(USERNAME)
    page.get_by_label("Password").fill(PASSWORD)
    page.get_by_role("button", name="Log in").click()
    expect(page).to_have_url(f"{BASE_URL}/surveys/")
def test_create_survey(page: Page) -> None:
    login(page)
    page.get_by_role("link", name="New survey").click()
    expect(page).to_have_url(f"{BASE_URL}/surveys/new/")
    page.get_by_label("Title").fill("Best Star Trek Captain")
    page.get_by_role("button", name="Save").click()
    # 回到列表页，新调查应出现。
    expect(page).to_have_url(f"{BASE_URL}/surveys/")
    expect(page.get_by_text("Best Star Trek Captain")).to_be_visible()
```

此测试证明：

* 认证工作正常
* 导航工作正常
* 表单渲染和提交工作正常
* 数据出现在 UI 中

调整链接文本、按钮文本和 URL 以匹配你的模板。

## 编辑和删除测试

一旦你有了一个工作的测试，编辑和删除遵循相同模式。保持简短：

```
def test_edit_survey_title(page: Page) -> None:
    login(page)
    page.get_by_text("Best Star Trek Captain").click()  # 或点击 "Edit" 链接
    page.get_by_label("Title").fill("Best Star Trek Series")
    page.get_by_role("button", name="Save").click()
    expect(page.get_by_text("Best Star Trek Series")).to_be_visible()
def test_delete_survey(page: Page) -> None:
    login(page)
    page.get_by_text("Best Star Trek Series").click()
    page.get_by_role("button", name="Delete").click()
    expect(page.get_by_text("Best Star Trek Series")).not_to_be_visible()
```

如果你的 UI 使用确认对话框，Playwright 可以处理它们，但对新手友好的方法是在 Django 中实现一个简单的确认页面，而不是浏览器模态框。

## 保持 UI 测试可靠

大多数不稳定的 UI 测试源于两个原因：不稳定的选择器和时机假设。

* 使用稳定的选择器：
    * 优先使用 `get_by_role`、`get_by_label` 和可见的按钮/链接名称
    * 仅对少数关键控件使用 `data-testid`
* 绝不使用睡眠：
    * 避免 `time.sleep(...)`
    * 使用 `expect(...)`，它会等待直到条件为 `true` 或超时
* 重置状态。UI 测试不应依赖之前的测试顺序。如果你的测试创建调查，可以：
    * 为每个测试创建唯一标题，或
    * 测试后清理，或
    * 在 UI 运行之间重置数据库（最适合 CI）

## 调试失败：有头模式、截图和追踪

当 UI 测试失败时，你需要证据。与单元测试不同，浏览器测试涉及时机、渲染和用户交互，因此仅从测试输出往往无法明显看出失败原因。

需要理解的最重要概念之一是无头模式和有头浏览器模式的区别：

* **无头模式（默认）**：浏览器在无可见窗口下运行。这更快，通常用于 CI 流水线和自动化运行。
* **有头模式**：浏览器窗口显示在屏幕上。你可以实时观察测试与页面的交互，这在调试时极其有用。

**良好实践**：为速度和自动化使用无头模式。当某事失败且你需要看清发生了什么时，使用有头模式。

在开发或调试测试时，以有头模式（可见浏览器）运行测试：

```
pytest -s e2e --headed
```

你也可以减慢交互速度，使其更易于跟踪：

```
pytest -s e2e --headed --slowmo 200
```

前面的代码在动作之间添加延迟（毫秒），以便你可视化地跟踪测试在做什么。

即使有了有头模式，你通常也想要一份出错记录，特别是在无屏幕可见的 CI 中。一个简单的模式是在测试失败时捕获截图：

```
def test_something(page: Page) -> None:
    try:
        # 测试步骤...
        pass
    except Exception:
        page.screenshot(path="e2e_failure.png", full_page=True)
        raise
```

对于更复杂的失败，Playwright 还支持追踪，它记录：

* 每一步的截图
* 文档对象模型（DOM）快照

