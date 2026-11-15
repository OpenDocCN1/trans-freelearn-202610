
# 多阶段 Docker 构建

前面展示的 `Dockerfile` 简单易懂，但它包含了仅在安装期间需要的工具，例如编译器。这些工具会增加最终镜像的体积，且在应用运行时并不需要。

当前的 Docker 镜像包含编译器（`gcc`、`make`）、包管理器缓存和构建依赖。这些在运行时并不需要，但会同时增加镜像体积和潜在的攻击面。

一种称为 `multi-stage build`（多阶段构建）的更高级技术将构建环境与运行时环境分离。第一阶段安装依赖，第二阶段仅将结果复制到一个干净的镜像中。

多阶段构建分离了：

1. **构建阶段**：安装依赖，编译所需内容
2. **运行时阶段**：仅复制运行应用所需的内容

这避免了将编译器等不必要的工具打包进最终镜像。

以下是使用多阶段构建重写的 `Dockerfile` 版本：

```
# ---- 阶段 1：构建依赖 ----
FROM python:3.12-slim AS builder
WORKDIR /app
RUN apt-get update \
 && apt-get install -y --no-install-recommends build-essential \
 && rm -rf /var/lib/apt/lists/*
COPY requirements.txt .
RUN pip install --upgrade pip \
 && pip install --prefix=/install --no-cache-dir -r requirements.txt
# ---- 阶段 2：运行时镜像 ----
FROM python:3.12-slim
WORKDIR /app
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
# 从构建阶段复制已安装的包
COPY --from=builder /install /usr/local
# 复制项目代码
COPY . /app/
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

这将生成更小的镜像、更快的下载速度和更小的攻击面。最终镜像仅包含 Python 运行时、您的依赖和您的应用。多阶段构建在生产环境设置中很常见，但对于学习项目则是可选的。

## 查看日志以调试容器问题

当基于 Docker 的设置出现问题时，首先要查看的地方是容器日志。

如果您使用以下命令运行容器：

```
docker compose up
```

您将在该终端窗口中直接看到日志。

但是，如果您在后台运行容器或打开一个新终端，可以使用以下命令查看日志：

```
docker compose logs
```

这将显示所有服务的组合输出，包括您的 Django 应用和 PostgreSQL 数据库。例如，您可能会看到 Django 启动消息、数据库连接错误、迁移警告和 Python 异常。

您可以专注于某个特定容器：

```
docker compose logs web
```

或者：

```
docker compose logs db
```

当您想将 Django 错误与数据库输出分开时，这非常有用。

要实时查看日志：

```
docker compose logs -f
```

这类似于运行 `docker compose up`，但即使容器已在后台运行也能工作。

查看日志很重要，因为 Docker 改变了您调试问题的方式。您不再直接在机器上运行所有内容。您的应用在容器内运行，因此错误会显示在那里，而不是在您的编辑器或终端历史记录中。

如果应用无法启动，或无法连接到数据库，日志通常会告诉您原因。

例如：

- "could not connect to server" 告诉您 PostgreSQL 尚未就绪
- "no such table" 告诉您数据库迁移未应用
- Python 回溯告诉您代码中有 bug

## 常见错误与快速修复

如果 Django 在 PostgreSQL 准备好接受连接之前启动，您可能会在首次启动时看到连接错误。对于学习项目，最简单的修复是重新运行 `docker compose up` 或重试迁移。在更高级的设置中，您可以添加 `wait-for-database`（等待数据库）脚本。

**提示**：请为 Django 容器展示一个 wait-for-database 脚本。

Docker 通过为每个人提供相同的 `Dockerfile` 和 Compose 服务，减少了 `works-on-my-machine`（在我的机器上能跑）问题。这就是团队使用它的原因。

在学习过程中，当在 Compose 文件中使用密钥时，请保持凭据明确标记为仅限开发使用。在真实部署中，您应该使用环境变量注入、密钥管理器或 Docker secrets，而不是提交密码。

**提示**：请解释环境变量、`.env` 文件和 Docker secrets 之间的区别。在 Docker Compose 中管理密钥的最佳实践是什么？

## 这在 TallyApp 整体部署故事中的定位

此 Docker 设置主要是一个开发环境，但它教授的模式可延伸至生产环境：不可变构建、多服务编排、通过卷持久化数据。

它还建立了简单的数据库升级路径：您可以在本地使用容器中的 PostgreSQL 进行开发，稍后部署时使用 VPS 上的 PostgreSQL 或托管 PostgreSQL，而无需更改应用逻辑，只需更改其配置。

虽然 Docker 专注于将应用打包成一致、可移植的环境，但它并不解决代码本身如何被跟踪、共享和随时间演变的问题。这正是 Git 的用武之地。如果 Docker 确保软件在任何地方以相同方式运行，Git 则确保团队（包括人类和 AI 程序员）可以协作开发该软件，而不会丢失变更记录或覆盖彼此的工作。Docker 和 Git 共同构成了 Web 开发工作流中天然的组合，但在结合它们之前，了解 Git 如何管理版本控制非常重要，以便每次变更都能被保存、审查，并在必要时撤销。

# 使用 Git 进行源代码版本控制

在为 TallyApp 添加功能之前，您需要防范自己的失误。Git 为您提供完整的项目历史，让您安全地实验，并使协作和部署变得简单直接。

本节将通过 VS Code 内置的 Git 集成，教授一个聚焦、实用的 Git 子集。您将学习如何初始化仓库、进行有意义的提交、创建分支、连接远程仓库，以及从常见错误中恢复。

## Git 的作用

Git 是一个分布式版本控制系统。它记录项目随时间变化的快照。

有几个术语很重要，如 *表 11.2* 所示：

| **术语** | **定义** |
| --- | --- |
| 仓库 | Git 跟踪的文件夹。它包含一个隐藏的 `.git` 目录，用于存储项目历史。 |
| 提交 | 项目在特定时刻的已保存快照，附带一条解释变更的消息。 |
| 工作分支 | 您当前正在编辑的文件。 |
| 暂存区 | 您打算包含在下一次提交中的变更集合。 |
| 分支 | 一条并行的开发线。您可以在分支上实验而不影响 `main`。 |
| 远程仓库 | 您仓库的托管副本，通常在 GitHub、GitLab 或 Bitbucket 上。 |

表 11.2：常见 Git 术语

Git 在您的机器上本地运行。GitHub 等服务托管远程副本，以便您备份和共享工作。

**提示**：请使用分步解释详细说明 Git 的暂存区。

## 在 VS Code 中为 TallyApp 初始化 Git

创建 TallyApp Django 项目后，立即初始化 Git：

1. 在 VS Code 中打开 `TallyApp` 文件夹。
2. 点击左侧边栏的 **源代码管理** 图标。
3. 点击 **初始化仓库**，如 *图 11.5* 所示：

![](Images/B34553_11_05.png)

图 11.5：VS Code 源代码管理视图

4. 您现在拥有了一个本地 Git 仓库，如 *图 11.6* 所示：

![](Images/B34553_11_06.png)

图 11.6：VS Code 源代码管理视图

## 为 Django 创建 .gitignore

Django 项目会生成不应提交的文件。您想要跟踪的是源代码，而非特定机器的构件或密钥。

