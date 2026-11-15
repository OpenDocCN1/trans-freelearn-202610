
# SQLite 并发限制与 PostgreSQL 隔离级别对比

**Prompt**：请解释 Django 应用中 SQLite 的并发限制。PostgreSQL 使用哪些隔离级别？它们与 SQLite 相比如何？何时应该将 Django 项目从 SQLite 迁移到 PostgreSQL？

## 使用 Docker 的前提条件

要使用 Docker，你需要以下组件：

*   `Docker Desktop`（Windows 或 Mac）或 `Docker Engine`（Linux）
*   一个包含 `manage.py` 的 Django 项目

从以下链接安装适用于你操作系统的 Docker：[https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)。

## 设置基础 TallyApp 项目

现在我们可以在容器化之前构建最简单的 Django 项目：

1.  创建一个名为 `Chapter11` 的文件夹。在该文件夹内，运行虚拟环境命令以创建包含该项目专用 Python 安装的 `.venv` 文件夹：
    *   Windows 上运行：`python -m venv .venv`
    *   macOS 和 Linux 上运行：`python3 -m venv .venv`

2.  激活虚拟环境：
    *   Windows 上运行：`.venv\Scripts\activate`
    *   macOS 和 Linux 上运行：`source .venv/bin/activate`

3.  激活成功后，终端提示符通常会显示虚拟环境名称：
    ```
    (.venv) C:\web-dev-ai\Chapter11>
    ```

4.  在虚拟环境激活的情况下，使用 pip 安装 Django：
    ```
    pip install django
    ```

5.  验证安装：
    ```
    django-admin --version
    ```

6.  你应该会看到类似 `6.0.2` 的 Django 版本号。如果出现，说明 Django 已准备就绪。

7.  在命令提示符或终端中，创建一个名为 `tallyapp` 的 Django 项目：
    ```
    django-admin startproject tallyapp
    ```

8.  进入 `tallyapp` 项目目录：
    ```
    cd tallyapp
    ```

9.  对本地 SQLite 数据库应用迁移：
    ```
    python manage.py migrate
    ```

10. 创建管理员账户，并根据提示设置用户名、邮箱和密码：
    ```
    python manage.py createsuperuser
    ```

11. 运行 Django 开发服务器：
    ```
    python manage.py runserver
    ```

12. 启动浏览器并访问 [http://127.0.0.1:8000/](http://127.0.0.1:8000/)。你可以 `Ctrl` + `Click` 输出中的链接，而无需手动输入 URL。如果看到 Django 欢迎页面，说明设置正确。

要查看管理站点，请访问 [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/)。

## 设置 Docker 托管项目

现在我们可以创建文件，将项目配置为在 Docker 容器中运行，并连接另一个容器中的 PostgreSQL 作为数据库。

你将创建三个文件：

*   `Dockerfile`：定义如何为 Django 项目构建 Web 应用容器，关注创建过程
*   `.dockerignore`：定义构建镜像时排除的文件和文件夹
*   `docker-compose.yml`：定义如何协同运行多个容器，关注容器间协调

### 创建 Dockerfile

我们首先定义一个 `Dockerfile`，告诉 Docker 如何构建容器镜像。你可以把 `Dockerfile` 看作一个配方，它回答以下问题：

*   从什么基础系统开始？
*   安装什么依赖？
*   复制什么代码？
*   容器启动时运行什么命令？

对于 TallyApp 项目，`Dockerfile` 将：

*   从 `python:3.12-slim` 开始，因为它是一个稳定、轻量的官方 Python 基础镜像，适合 Django 6 项目，但不是最新的 Python 镜像
*   从 `requirements.txt` 安装依赖
*   将 TallyApp Django 项目复制到 `/app`
*   运行 `python manage.py runserver`

`Dockerfile` 生成一个可复用的镜像，如 `tallyapp-web-image`。该镜像是一个自包含单元，可在任何地方运行。

在 `Chapter11\tallyapp` 文件夹中，创建一个名为 `Dockerfile` 的文件，内容如下：

```dockerfile
# Use an official Python image as the base
FROM python:3.12-slim
# Prevents Python from writing .pyc files and buffers to stdout/stderr
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
# Set a working directory inside the container
WORKDIR /app
# System deps (optional but common): build tools for some Python packages
RUN apt-get update \
    && apt-get install -y --no-install-recommends build-essential \
    && rm -rf /var/lib/apt/lists/*
# Install Python dependencies first (for better layer caching)
COPY requirements.txt /app/
RUN pip install --no-cache-dir -r requirements.txt
# Copy the rest of the project
COPY . /app/
# Default command for the container (development-friendly)
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

关于上述代码的说明：

*   我们使用 `python:3.12-slim` 作为基础镜像，以获得稳定、广泛支持的 Python 运行时和最小化的基础系统，避免 `latest` 标签的不可预测性，同时保持镜像精简且可复现。

    **Prompt**：`python:3.12-slim` 是 Docker 容器中最新且最好的 Python 镜像吗？

*   它从 `requirements.txt` 安装依赖。

*   它将你的代码复制到镜像中。

    **Prompt**：为什么 `COPY requirements.txt` 在 `COPY .` 之前执行？

*   它将 Django 开发服务器绑定到 `0.0.0.0`，使其可从容器外部访问。

    **Warning**：生产环境容器必须将 `runserver` 替换为 `Gunicorn`，但目前这是一个清晰的第一步。你将在 [*第 13 章*](#13.xhtml#_idTextAnchor872) *《指定并构建初始项目》* 中学习 `Gunicorn`。

### 创建 Docker Ignore 文件

构建 Docker 镜像时，Docker 会将整个项目文件夹（称为构建上下文）发送给 Docker 守护进程。默认情况下，项目目录中的所有内容都会被包含，甚至包括你不想放入镜像的文件。这就是 `.dockerignore` 文件的作用。

就像 `.gitignore` 告诉 Git 忽略哪些文件（你将在下一节看到），`.dockerignore` 告诉 Docker 在镜像构建过程中排除哪些文件。这能让镜像更小、构建更快，并避免意外包含敏感或不必要的文件。

在 `Dockerfile` 同级目录下创建 `.dockerignore` 文件，内容如下：

```text
# .dockerignore
.venv/
__pycache__/
*.pyc
.git/
.gitignore
db.sqlite3
.env
*.sqlite3
docker-compose.yml
Dockerfile
.DS_Store
```

其好处包括：

*   **更小的镜像**：虚拟环境、缓存文件和本地数据库可能很大。排除它们可显著减小镜像体积。
*   **更快的构建**：Docker 在构建前上传构建上下文。文件越少，构建越快，尤其是项目增长时。
*   **更强的安全性**：`.env` 等文件可能包含 API 密钥或数据库密码等机密。如果包含在镜像层中，后续很难移除。
*   **更整洁的环境**：容器应仅包含运行所需内容。`.venv` 或 `.git` 等开发工件在容器内无关紧要。

即使 `Dockerfile` 使用 `COPY . /app/`，`.dockerignore` 文件也能确保排除的文件从一开始就不会被复制。这让你无需复杂的 `COPY` 规则即可保持镜像整洁。
