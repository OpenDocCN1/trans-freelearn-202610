
如果短代码不存在，将返回 `404 Not Found` 响应。

这些条件足够具体，可以转化为自动化测试。

### 数据模型

添加一个名为 `ShortLink` 的新 Django 模型。其字段如表 14.1 所示：

| **字段** | **类型** | **描述** |
| --- | --- | --- |
| `id` | 主键 | 内部标识符 |
| `owner` | `ForeignKey(User)` | 短链接的创建者 |
| `target_url` | `URLField` | 原始 URL |
| `code` | `CharField(10)` | 唯一短标识符 |
| `created_at` | `DateTimeField` | 时间戳 |

表 14.1：ShortLink 模型的字段

模型约束：

*   `code` 值必须唯一，且最多十个字符
*   `code` 值应从 URL 安全字符中随机生成

### 验证步骤

可以通过以下方式验证该功能：

1.  运行单元测试
2.  在浏览器中访问生成的短链接
3.  确认重定向行为

## 请求 AI 生成实现计划

规范存在后，你可以将其提供给 AI 编码助手并请求计划。

示例提示词：

```
You are helping extend an existing Django project named TallyApp.
Implement the following feature according to the specification below.
[Paste spec]
First generate an implementation plan. Do not write code yet.
```

一个好的计划应建议如下步骤：

1.  创建一个名为 `shortlinks` 的新 Django 应用
2.  定义 `ShortLink` 模型
3.  添加迁移
4.  创建视图用于：
    *   创建短链接
    *   重定向短链接
5.  添加 URL 路由
6.  创建基础 HTML 表单
7.  添加单元测试

在请求 AI 实现之前，请先审查计划。

## 使用 Specify 和 Codex 规划并实现 URL 短链接功能

首先，关于组件的说明。GitHub 的 Spec Kit 及其 Specify CLI 可以初始化现有项目、安装模板并生成特定代理的命令文件。Spec Kit 支持 Codex CLI 作为首选代理选项，并专门记录了 `specify init . --ai codex` 和 `specify init --here --ai codex` 用于现有项目。

OpenAI 当前的 Codex 设置流程是：登录，选择要让 Codex 处理的文件夹或仓库，然后开始任务。

让我们分步使用 SpecKit 与 Codex：

1.  在现有 Django 项目的根文件夹中开始，即包含 `manage.py` 和应用文件夹的同一文件夹。
2.  在当前文件夹中初始化 Spec Kit，而不是创建新文件夹：

    ```
    specify init . --ai codex
    ```

    Spec Kit 也支持 `--script ps`，如果你想要面向 PowerShell 的脚本，这在 Windows 上是明智的选择：`specify init . --ai codex --script ps`

3.  你将收到关于现有文件的警告，如图 14.2 所示：

    ![](Images/B34553_14_02.png)

    图 14.2：在项目文件夹中初始化 Specify

4.  按 `Y` 继续。
5.  选择是否需要 Bash/shell 或 PowerShell 脚本。只要安装了 **Windows Subsystem for Linux 2** (**WSL2**)，两者在 Windows 上都可用。在 Mac 或 Linux 上，选择 Bash。

    **提示**：请解释 GitHub Spec Kit 的内部工作原理及其在项目仓库中生成的文件。展示 Spec Kit 创建的目录结构并解释每个文件的用途。

6.  为该项目设置 Codex 配置的环境变量：

    ```
    setx CODEX_HOME "C:\web-dev-ai\TallyProject\.codex"
    ```

7.  使用最新的 GPT 模型启动 Codex，例如 2026 年 4 月的 5.5 版本：

    ```
    codex --model gpt-5.5
    ```

    截至 2026 年 4 月撰写时，OpenAI 也表示 GPT-5.5 现已在 Codex 中可用，且是大多数 Codex 任务的推荐选择；在 CLI 中可使用 `codex --model gpt-5.5` 启动会话。你可以在以下链接查看最新的 OpenAI Codex 模型：[https://developers.openai.com/codex/models](https://developers.openai.com/codex/models)

8.  在 Codex 中创建宪章文件，如图 14.3 和以下输出所示：

    ```
    /speckit.constitution
    This is an existing Django project named TallyApp.
    Project principles:
    1. Keep the architecture simple and beginner-friendly.
    2. Prefer Django built-ins over third-party packages unless there is a strong reason.
    3. Write code that is readable for learners new to Django.
    4. Use pytest or Django tests to verify acceptance criteria.
    5. Add small, incremental features instead of broad architectural rewrites.
    6. Preserve existing behavior unless the spec explicitly changes it.
    7. Use server-rendered Django templates unless a richer client-side experience is clearly justified.
    8. Avoid over-engineering.
    ```

    ![](Images/B34553_14_03.png)

    图 14.3：创建宪章文件

## 创建 URL 短链接规范

接下来，运行 Spec Kit 规范步骤。记录的命令是 `/speckit.specify`。

在 Codex 中运行：

```
/speckit.specify
```

然后粘贴如下聚焦的提示词：

```
Create a feature specification for adding a URL shortener to the existing TallyApp Django project.
Goal:
Allow authenticated users to create short URLs that redirect to longer links inside TallyApp. The main use case is generating short links for surveys so they are easier to share.
Non-goals:
- No click analytics in this version
- No expiration dates
- No custom aliases
- Only authenticated users can create short links
User stories:
- As a logged-in user, I can create a short link for a survey URL
- As a logged-in user, I can view a list of short links I created
- As any visitor, I can visit a short URL and be redirected to the target page
Acceptance criteria:
- When a valid URL is submitted, a short code is generated
- The short code is unique
- Visiting /s/<code> redirects to the stored target URL
- A user can only see short links they created
- If a short code does not exist, return 404 Not Found
Data model:
ShortLink
- owner: ForeignKey(User)
- target_url: URLField
- code: CharField(max_length=10, unique=True)
- created_at: DateTimeField(auto_now_add=True)
Constraints:
- code must be unique
- codes should be randomly generated
- deleting the owning user deletes the link
Please generate a spec that matches the existing Django codebase and keeps the implementation small and teachable.
```

当 Codex 生成规范时，请仔细阅读。这是你应该纠正任何过于聪明、过于宽泛或与现有 TallyApp 设计不一致的地方。

你可以随意修改 Specify 为你创建的任何文件。

## Spec Kit 文件应提交到 Git 吗？

在大多数教学和团队项目中，是的：提交解释功能的规范工件，特别是 `specs/` 文件夹和 `spec.md`、`plan.md`、`tasks.md`、`quickstart.md` 等文件。这些文件是项目设计历史的一部分。它们解释了你要求 AI 构建什么、接受了什么以及功能应如何验证。

请更谨慎地对待 `.specify/` 文件夹。它可能包含项目级的 Spec Kit 配置、模板、脚本和代理命令文件。对于团队项目，如果你希望每个人使用相同的 Spec Kit 工作流，则提交它。对于单人实验，如果它只包含可重新创建的生成辅助文件，可以将其保留在本地。
