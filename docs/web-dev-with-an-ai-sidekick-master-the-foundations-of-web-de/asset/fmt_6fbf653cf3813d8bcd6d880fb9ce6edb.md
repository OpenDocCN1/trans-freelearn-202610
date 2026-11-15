
# TallyApp 项目结构

## Django 基础模板

```html
{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>{% block title %}TallyApp{% endblock %}</title>
    <link rel="stylesheet" href="{% static 'css/site.css' %}">
</head>
<body>
    <header class="site-header">
        <div class="wrap nav-shell">
            <a href="{% url 'home' %}" class="brand">
                <img src="{% static 'img/logo.svg' %}" alt="TallyApp logo" class="brand-logo">
                <span>TallyApp</span>
            </a>
            <nav class="site-nav">
                <a href="{% url 'home' %}">Home</a>
                {% if request.user.is_authenticated %}
                    <a href="{% url 'survey_list' %}">My Surveys</a>
                    {% if request.user.is_staff %}
                        <a href="{% url 'approval_list' %}">Approvals</a>
                    {% endif %}
                    <form method="post" action="{% url 'logout' %}">
                        {% csrf_token %}
                        <button type="submit" class="link-button">Log out</button>
                    </form>
                {% else %}
                    <a href="{% url 'login' %}">Log in</a>
                    <a href="{% url 'register' %}">Register</a>
                {% endif %}
            </nav>
        </div>
    </header>
    <main class="wrap page-shell">
        {% if messages %}
            <ul class="messages">
                {% for message in messages %}
                    <li class="message {{ message.tags }}">{{ message }}</li>
                {% endfor %}
            </ul>
        {% endif %}
        {% block content %}{% endblock %}
    </main>
</body>
</html>
```

## 文件夹结构

- `registration` 文件夹包含登录和注册两个网页。
- `surveys` 文件夹包含审批创作者、管理调查等网页。

> **Prompt**：如果 TallyApp 增长到每份调查 100 万份回复，需要哪些架构变更？如何将 TallyApp 演变为多租户 SaaS 产品？

# TallyApp 中最有趣的设计决策

如果 TallyApp 有一个数据库设计真正重要的地方，那就是调查答案的建模方式。系统中的其他部分都相对简单直接。

困难源于一个简单事实：并非所有问题都相同。

TallyApp 目前支持三种问题类型：

- 单选多项选择题
- 多选多项选择题
- 自由文本题

乍一看，可能会想用一个灵活的 `Answer` 模型配合几个可空字段来存储所有内容。例如：

- 针对单选题的 `selected_choice` 字段
- 针对多选题的 `selected_choices` 多对多字段
- 针对文本题的 `text_value` 字段

这可行，但迫使你必须仔细考虑数据完整性。你必须确保：

- 单选题永不存储多个选项
- 多选题可存储多个选项
- 文本题永不引用选项
- 基于选项的题目永不以自由文本代替选项

模型必须编码反映应用业务逻辑的规则。

这正是你在《第 7 章 SQL – Managing Data》中学到的关系型思维变得有趣的地方。一种常见且整洁的方法是：

- `Response` 模型：代表一次用户提交
- `Answer` 模型：代表对单个问题的回答
- 从 `Answer` 到 `Question` 的外键
- 从 `Answer` 到 `Response` 的外键
- 可空外键指向 `Choice`，用于单选题
- 多对多关系指向 `Choice`，用于多选题
- 文本字段用于文本回答

然后基于 `question.question_type` 在表单或模型验证层面强制执行规则。

这个设计传授了三个有用的理念：

1. 首先，它展示了关系型数据库如何建模变异性。你不需要为三种问题类型创建三张独立的表，而是创建一个灵活的结构并通过逻辑约束它。
2. 其次，它教授了规范化。调查回复不是一团扁平的 JSON，而是一组结构化的答案，每个答案都关联到一个问题。这使得聚合、过滤和报表变得容易得多。
3. 第三，它凸显了未来的可扩展性。如果你后来决定添加评分量表题或数值输入题，你的模型已经有了清晰的扩展点。

这里还有一个重要的架构教训：数据模型定义了什么是可能的。如果你或你的 AI 模型草率地回答，稍后在实现聚合计数、按问题过滤、验证规则、导出功能和性能优化等特性时就会举步维艰。

事实上，许多超出自身承载能力的真实系统，都是因为早期的数据模型过于简单。

> **Prompt**：请深入解释为什么建模调查答案是 TallyApp 中最重要的架构决策。展示三种处理单选、多选和文本答案的不同数据库设计，并比较权衡。高级工程师如何审查 AI 生成的 Django 模型？

最后在本章中，我们将了解 2026 年 2 月发布的 Mac 版和 2026 年 3 月发布的 Windows 版 Codex GUI 应用。

# 使用 Codex App 进行交互式开发

到目前为止在本章中，你使用一次性提示词生成了完整的项目脚手架。这种技术高效但仍主要基于文本。你描述想要什么，AI 生成代码，然后你审查它。

**Codex App** 引入了一种不同的工作方式。你不再仅通过文本描述系统，而是可以与运行中的应用交互，并直接针对所见内容给出反馈。

这在 TallyApp 本地运行后、从*构建结构*转向*完善行为和界面*时特别有用。

你可以在官方文档了解更多关于 Codex App 的信息：[https://developers.openai.com/codex/app](https://developers.openai.com/codex/app)

## Codex App 增加的能力

Codex App 是一个用于 AI 辅助开发的图形界面。它与你的编辑器和本地开发环境并行，允许你：

- 与模型聊天讨论代码库
- 运行命令并检查输出
- 处理文件和生成的代码
- 与应用的实时预览交互

与本章早期工作流的关键区别在于：AI 不再仅从描述工作。它可以通过实际运行的 UI 获得指导。

这减少了 AI 辅助开发中的一个常见问题：模糊指令（如“让它看起来更好”或“修复布局”）往往产生不一致的结果。

## 用于 UI 工作的基于浏览器的交互

Codex App 最有用的功能之一是其通过内置浏览器面板直接与运行中的 Web 应用交互的能力。这将 Web UI 工作变得更接近“指着屏幕说：改这里”。

一旦你的 Django 开发服务器运行，你可以将 Codex App 连接到本地站点：

1. 与你偏好的模型开始聊天
2. 启动 Django 开发服务器（例如 `python manage.py runserver`）
3. 在 Codex App 中打开浏览器面板
