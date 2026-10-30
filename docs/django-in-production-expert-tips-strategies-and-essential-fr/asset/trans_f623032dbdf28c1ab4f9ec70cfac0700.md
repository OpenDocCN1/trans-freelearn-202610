

```
name: Django CI Test Cases
on: push # Run this Action on every code push made.
env:
  DB_NAME: blog_testdb
  DB_USERNAME: root
  DB_PASSWORD: root
  DB_HOSTNAME: 127.0.0.1
  DB_PORT: 5432
  DB_ENGINE: django.db.backends.postgresql
  REDIS_HOST: 127.0.0.1
  REDIS_PORT: 6379
  REDIS_PASSWORD: redisPassWord
```

工作流的名称使用 `name` 属性定义；在此我们将工作流命名为 `CI Django Test Cases`：

```
name: CI Django Test Cases
```

`on` 属性定义工作流应何时执行。多个事件可以触发 GitHub Actions；请查阅官方文档了解更多详情：https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows：

```
on: push
```

`env` 部分与环境变量相同。它用于为代码仓库设置任何环境变量值。我们在此使用 `env` 属性设置 Postgres 凭据：

```
env:
  DB_NAME: blog_testdb
  ... # Other environment variables as needed
```

现在，让我们在 `.github/workflows/django-ci.yaml` 配置文件中添加 GitHub jobs。请注意，我们是将代码追加到前一节：

```
jobs:
  # Label of the container job
  container-job:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:latest
        env:
          POSTGRES_USER: root
          POSTGRES_PASSWORD: root
          POSTGRES_DB: blog_testdb
        ports:
          - 5432:5432
        # Set health checks to wait until Postgres has started
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
      redis:
        image: redis:latest
        ports:
          - 6379:6379
```

在 `job` 属性部分内，我们可以定义工作流的多个 jobs。例如，如果我们想在同一个工作流中运行 linting、单元测试和集成测试，那么我们可以创建不同的 jobs。在我们的例子中，我们创建了 `container-job`（注意 – 它可以取任何名称），它运行 Django 测试。

`runs-on` 属性定义 job 执行的基础层。除非有特定的需求或定制需求，否则建议从 `ubuntu-latest` 开始：

```
runs-on: ubuntu-latest
```

`services` 属性定义 job 所需的所有外部服务。例如，我们需要 Postgres 来运行单元测试，因为我们的 Django 应用连接了数据库。同样，我们也可以将 Redis 作为单独的服务连接到此 job。更多详情请查阅官方文档：https://docs.github.com/en/actions/using-containerized-services/about-service-containers。

现在让我们为我们的 actions 添加 `steps`：

```
jobs:
  container-job:
    services:
      . . . # Service config
    steps:
      - uses: actions/checkout@v3
      - name: Set up Python 3.11
        uses: actions/setup-python@v3
        with:
          python-version: 3.11
      - name: Install Dependencies
        run: |
          cd backend
          python -m pip install --upgrade pip
          pip install -r requirements/requirements-local.txt
      - name: Run Django migrations
        run: |
          cd backend
          python manage.py migrate
      - name: Running Django Test cases
        run: |
          cd backend
          python manage.py test --no-input
```

在设置好 services 后，我们现在将定义 `steps`。在 `steps` 中，我们将把 job 中需要执行的全部命令分组：

```
steps:
     - uses: actions/checkout@v3
     - name: Set up Python 3.11
       uses: actions/setup-python@v3
       with:
         python-version: 3.11
```

首先，我们使用 `uses: actions/checkout@v3` GitHub Actions job 从仓库检出代码并使其可用，然后为代码执行设置 Python 版本。

下一步是通过使用 `pip install -r requirements/ci-requirements.txt` 命令安装依赖：

```
     - name: Install Dependencies
       run: |
         cd backend
         python -m pip install --upgrade pip
         pip install -r requirements/requirements-local.txt
```

`name` 属性用于为 job 中的某个步骤命名；在 GitHub Actions UI 中，使用名称可以轻松识别步骤。

```
     - name: Run Django migrations
       run: |
        cd backend
        python manage.py migrate
     - name: Running Django Test cases
       run: |
        cd backend
        python manage.py test --no-input
```

在我们的自定义 actions 中，我们连接了 Postgres 数据库，然后每当有人进行代码提交并将代码推送到 GitHub 时运行 Django 测试用例。在我们的示例中，我们执行了一个简单的操作，即每次开发者进行代码推送时都运行 Django 测试用例。

### 测试 GitHub Actions

现在，我们准备运行与 Django 相关的命令。首先，我们在 Django 项目上运行迁移，然后执行 Django 测试用例。如果我们的测试用例通过，则我们将在 GitHub Actions 流水线中看到成功；否则，我们会看到失败的测试用例。

每当我们将代码更改推送到 GitHub 服务器时，我们的流水线将被自动触发。我们可以使用以下命令进行代码提交并推送代码：

```
git commit -am "Test pipeline"
git push
```

这将把本地更改推送到远程 GitHub 仓库并触发流水线。我们可以看到 *Figure 12.2* 显示工作流成功运行，其标题与我们的 `git commit` 消息相同：

![Figure 12.2: Showing GitHub Actions pipeline triggered on code push](image/Figure_12.2_B18867.jpg) 

图 12.2：显示代码推送时触发的 GitHub Actions 流水线

当我们进入工作流时，我们只能看到一个条目，因为我们只定义了一个 job。对于复杂项目，可能存在多个 jobs。*Figure 12.3* 显示我们在推送代码时 job 的运行情况：

![Figure 12.3: Showing a GitHub job](image/Figure_12.3_B18867.jpg) 

图 12.3：显示一个 GitHub job

当我们再次进入 `container-job` 时，我们可以看到我们在 `django-ci.yaml` 文件中定义的所有步骤。*Figure 12.4* 显示正在运行的所有步骤：

![Figure 12.4: Showing all the steps we have configured in our django-ci.yaml file](image/Figure_12.4_B18867.jpg) 

图 12.4：显示我们在 django-ci.yaml 文件中配置的所有步骤

GitHub Actions 可以被视为一个远程服务器，可以执行开发者能够在服务器上执行的任何任务。同样，我们可以向我们的 CI 流水线添加更多 actions。以下是一些我推荐使用的 actions：

*   **代码覆盖率阈值**：为每次代码推送添加代码覆盖率阈值测试将确保开发者积极追求编写单元测试，以确保系统的稳定性

*   **代码 linter**：应启用 Linting，这样如果开发者未设置 Git hooks 和本地 linter，则可以在 GitHub 流水线中捕获

*   **运行 Git hooks**：始终建议使用 GitHub Actions 重新运行所有客户端 Git hooks，这样如果某些开发者未设置或绕过检查，则可以轻松捕获

*   **Slack/Teams/Email 通知器**：如果你的团队使用 Slack 或 Teams，你应该将 Slack 集成到所有重要操作中，以便通知并将所有重要通信直接发送为个人消息或群组消息

现在，让我们学习在使用 GitHub Actions 时的一些良好实践：

