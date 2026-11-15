
**良好实践**：保持您的 `.dockerignore` 文件与 `.gitignore` 文件保持一致，但不要假设它们应该完全相同。Docker 关注构建上下文和镜像内容，而 Git 关注版本控制。某些文件（如 `Dockerfile` 或 `docker-compose.yml`）会被 Git 跟踪，但通常不属于最终镜像内部。

## 创建 Docker Compose 文件

接下来，我们将定义一个 `docker-compose.yml` 文件。`Dockerfile` 定义如何构建单个镜像，`.dockerignore` 文件从镜像中排除文件和文件夹，而 `docker-compose.yml` 文件定义如何将多个容器一起运行。

您可以使用 `docker run` 手动运行 Django 容器，但这很快就会变得混乱。您需要单独启动 PostgreSQL，手动链接容器，手动传递环境变量，并自行管理端口和卷。Docker Compose 使用一条命令和一个配置文件取代了所有这些工作。

`docker-compose.yml` 文件是**编排**。它解决了以下问题：

* 我们需要哪些容器？
* 它们如何连接？
* 它们使用哪些环境变量？
* 哪些端口被暴露？
* 哪些卷持久化数据？

对于我们的 TallyApp 项目，Docker Compose 将定义两个服务：

* `web` 是 Django 应用容器（由 `Dockerfile` 构建）
* `db` 是 PostgreSQL 容器

它还将：

* 通过网络连接这两个容器
* 映射端口（`8000`、`5432`）
* 为 Django 项目设置环境变量
* 创建一个用于数据库持久化的卷

运行单条命令 `docker compose up` 将启动整个系统，而不仅仅是一个容器。

在 `Chapter11\tallyapp` 文件夹中，为 Django 和 PostgreSQL 创建一个 `docker-compose.yml` 文件：

```yaml
services:
  db:
    image: postgres:16
    environment:
      POSTGRES_DB: tallyapp
      POSTGRES_USER: tallyapp
      POSTGRES_PASSWORD: tallyapp_password_change_me
    volumes:
      - tallyapp_postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U tallyapp -d tallyapp"]
      interval: 5s
      timeout: 5s
      retries: 5
  web:
    build:
      context: .
    depends_on:
      db:
        condition: service_healthy
    environment:
      # Django will read these to build DATABASES config (you wire this up in settings.py)
      DJANGO_DB_NAME: tallyapp
      DJANGO_DB_USER: tallyapp
      DJANGO_DB_PASSWORD: tallyapp_password_change_me
      DJANGO_DB_HOST: db
      DJANGO_DB_PORT: "5432"
      DJANGO_SECRET_KEY: dev-only-change-me
      DJANGO_DEBUG: "1"
    ports:
      - "8000:8000"
    volumes:
      - .:/app
volumes:
  tallyapp_postgres_data:
```

请注意上述代码的以下几点：

* `db` 使用官方 PostgreSQL 镜像，并使用该镜像文档中记录的环境变量初始化数据库。
* `db` 有一个 `healthcheck`，而 `web` 依赖于 `db` 处于 `service_healthy` 状态，而不仅仅是已启动。
* `tallyapp_postgres_data` 卷使数据库数据在容器重启时保持持久化。
* `web` 服务从您的 `Dockerfile` 构建。
* `volumes: - .:/app` 绑定挂载意味着主机上的代码更改会立即出现在容器内部，这非常适合基于 Docker 的开发环境。
* `depends_on` 在 Web 容器之前启动数据库容器，但它本身并不能保证 PostgreSQL 已准备好接受连接。为了使启动更加可靠，我们定义了一个 PostgreSQL `healthcheck`，并将 `depends_on` 设置为等待条件：`service_healthy`。然后 Docker Compose 会等待数据库报告就绪后，再启动 Django。

## 从 SQLite 切换到 PostgreSQL

现在我们可以配置项目文件，如 Python 依赖和 Django 项目设置，以便项目使用 PostgreSQL 而不是 SQLite：

1. 在 `Chapter11\tallyapp` 文件夹中，创建一个 `requirements.txt` 文件，配置 Django 6 在 Docker 中使用 PostgreSQL：

```
Django>=6.0,<7.0
psycopg[binary]>=3.1
```

2. 在 `settings.py` 中，您可以根据环境变量切换数据库配置。一种简单的模式是：

* 如果设置了 `DJANGO_DB_HOST`，则使用 PostgreSQL
* 否则，回退到 SQLite 用于本地非 Docker 运行

```python
import os
DJANGO_DB_HOST = os.environ.get("DJANGO_DB_HOST")
if DJANGO_DB_HOST:
    # PostgreSQL configuration
    DATABASES = {
        "default": {
            "ENGINE": "django.db.backends.postgresql",
            "NAME": os.environ.get("DJANGO_DB_NAME", "tallyapp"),
            "USER": os.environ.get("DJANGO_DB_USER", "tallyapp"),
            "PASSWORD": os.environ.get("DJANGO_DB_PASSWORD", ""),
            "HOST": DJANGO_DB_HOST,
            "PORT": os.environ.get("DJANGO_DB_PORT", "5432"),
            "CONN_MAX_AGE": 60,  # persistent connections (good for production)
        }
    }
else:
    # SQLite fallback (default for local development)
    DATABASES = {
        "default": {
            "ENGINE": "django.db.backends.sqlite3",
            "NAME": BASE_DIR / "db.sqlite3",
            "OPTIONS": {
                "timeout": 20,  # helps reduce "database is locked" errors
            },
        }
    }
```

## 构建并运行 Docker 系统

现在我们可以使用 Docker Compose 构建 Docker 镜像并启动容器：

1. 构建镜像：

```
docker compose build
```

2. 启动容器：

```
docker compose up
```

3. 此时 PostgreSQL 在一个容器中运行，Django 在另一个容器中运行，暴露在 [http://localhost:8000](http://localhost:8000) 上，如 *图 11.2* 所示：

![](Images/B34553_11_02.png)

**图 11.2**：Docker Compose 中运行的两个容器

**提示**：当我运行 `docker compose up` 时，底层实际上发生了什么？

1. 在 **Docker Desktop** 中，展开 **tallyapp** 条目，查看其 PostgreSQL 数据库和 **tallyapp-web** Django 主机的两个容器，如 *图 11.3* 所示：

![](Images/B34553_11_03.png)

**图 11.3**：Docker Desktop 中运行的两个容器

2. 在新终端窗口中，在 `web` 容器内运行迁移并创建管理员用户：

```
docker compose exec web python manage.py migrate
docker compose exec web python manage.py createsuperuser
```

Docker 改变了您运行命令和查找错误的位置。您通常不会 SSH 进入服务器。您在已经包含您的应用和依赖项的容器内运行命令。您还需要阅读容器日志，因为应用就在那里运行。

3. 启动您喜欢的浏览器并导航到管理站点：[http://localhost:8000/admin/](http://localhost:8000/admin/)

4. 使用您之前创建的管理员账户登录并查看用户列表，如 *图 11.4* 所示：

![](Images/B34553_11_04.png)

**图 11.4**：管理员账户

5. 关闭浏览器。

6. 在命令提示符或终端中，停止容器：

```
docker compose down
```

要停止容器并删除卷（这会清除数据库），请使用 `-v` 开关：`docker compose down -v`。这加强了卷为何重要。没有卷，数据库容器是一次性的，其数据也是如此。

## 使用多阶段构建减小镜像大小
