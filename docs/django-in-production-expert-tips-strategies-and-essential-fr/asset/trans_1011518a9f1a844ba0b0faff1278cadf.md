

```
DEBUG=True
DJANGO_ALLOWED_HOSTS=*
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
DB_USERNAME=django_db_user
DB_PASSWORD=abc^A12a*12
DB_HOSTNAME=django-demo.cz2r2xzvhh2n.ap-south 1.rds.amazonaws.com
DB_PORT=5432
REDIS_HOST=django-demo-
cache.dysbo0.ng.0001.aps1.cache.amazonaws.com
REDIS_PORT=6379
```

我们的代码现在已准备好连接到 ElastiCache Redis 服务器并部署到 Elastic Beanstalk。我们需要将 Django 设置文件移至 Git 暂存区——这是必不可少的，因为 `eb` CLI 仅会部署 Git 跟踪的代码，或在我们传递 `--stage` 标志时处于暂存状态的代码：

```
> git add .env
> eb deploy --staged
Creating application version archive "app-8e23-231024_202626040031-stage-231024_202626040080".
Uploading dda/app-8e23-231024_202626040031-stage-231024_202626040080.zip to S3. This may take a while.
Upload Complete.
2023-10-24 14:56:27    INFO    Environment update is starting.
2023-10-24 14:56:30    INFO    Deploying new version to instance(s).
2023-10-24 14:56:47    INFO    Instance deployment completed successfully.
2023-10-24 14:56:52    INFO    New application version was deployed to running EC2 instances.
2023-10-24 14:56:52    INFO    Environment update completed successfully.
```

### 3. 在浏览器中打开 `/ht/` 端点以验证数据库和缓存连接。在 *图 13.21* 中，我们可以看到当我们的 RDS Postgres 和 ElastiCache Redis 实例正确配置并连接到 Django 服务器时的预期输出：

![Figure 13.21: The health check endpoint, /ht/, showing success](image/Figure_13.21_B18867.jpg) 

图 13.21：健康检查端点 `/ht/` 显示成功

至此，我们已学习如何将 RDS Postgres 和 ElastiCache Redis 集群连接到 Django 服务。我们使用了一个非常简单的设置来创建数据库并连接到 Django 服务。我们将在 *Following the best practices for the AWS **infrastructure* 部分讨论在生产中使用 RDS 和 Elasticache 的所有最佳实践。

我们在 [*第 8 章*](#B18867_08.xhtml#_idTextAnchor169) 中学习了如何将 Celery 与 Django 集成，而在 [*第 11 章*](#B18867_11.xhtml#_idTextAnchor219) 中，我们看到了如何使用 Docker 在 Celery beat 旁边运行 Celery worker。我们使用 Docker 在 Elastic Beanstalk 环境中运行 Django 应用程序。对于小型应用程序，我们可以使用相同的设置并运行 Celery。

对于大型应用程序，我们应该将运行 Django 的 Web 应用程序与运行 Celery 任务的 worker 解耦。我们需要设置不同的 Beanstalk 环境来运行 Celery 和 Celery beat worker。为 worker 设置 Beanstalk 环境超出了本书的范围。

现在，让我们学习如何使用 GitHub Actions 部署我们的 Django 应用程序。

## 使用 GitHub Actions 在 Elastic Beanstalk 中部署 Django 应用程序

在 [*第 12 章*](#B18867_12.xhtml#_idTextAnchor233) 中，我们学习了如何使用 GitHub Actions 运行测试用例。现在，我们将学习如何使用 GitHub Actions 在 Elastic Beanstalk 中部署我们的 Django 应用程序。

首先，让我们学习如何将 GitHub Actions 连接到我们的 AWS 账户。

AWS 提供了官方的 GitHub Actions 属性（`https://github.com/aws-actions/configure-aws-credentials`），它们使用 AWS 访问密钥和 AWS 秘密访问密钥来处理身份验证，这两者我们都可以从 AWS IAM 控制台获取。我们需要提取密钥并将其添加到我们希望在相应 GitHub 仓库中运行操作的位置。

重要提示

在创建 AWS 访问密钥时，建议创建一个具有有限 AWS 资源访问权限的单独 IAM 用户，以便 GitHub Actions 使用。

一旦我们拥有 AWS 访问密钥，我们就可以遵循 GitHub Actions 文档。GitHub 提供了关于如何在 GitHub Actions 中使用密钥的非常好的指南。请查看 `https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions` 处的指南。

一旦我们在 GitHub 中配置了密钥，我们就可以使用以下示例测试我们的 GitHub-AWS 设置。在这里，我们使用 AWS CLI 来验证 GitHub Actions 是否可以访问我们的 AWS 账户：

```
name: Test Github-AWS actions integration
on: [workflow_dispatch]
jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: [3.8]
    steps:
      - uses: actions/checkout@v1
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_KEY }}
          aws-region: ap-south-1
      - name: get caller identity
        run: aws sts get-caller-identity
```

至此，我们已验证 GitHub Actions 可以连接到 AWS 账户。在 *Connecting RDS to the Django server* 部分，在将 RDS 与 Django 集成时，我们在代码中硬编码了所有 RDS 密钥值。这是一种不好的做法，也是安全隐患，因为我们的密钥会在代码中公开。因此，我们应该使用环境变量传递此类敏感值，并使用 AWS Secrets Manager 填充环境变量。我们可以使用 AWS 提供的官方 GitHub Actions 来提取我们的密钥并将其保存到 `.env` 文件中。在以下示例中，我们正在下载所有以 `beta` 开头的密钥：

```
- name: Get Secret Names by Prefix
  uses: aws-actions/aws-secretsmanager-get-secrets@v1
  with:
    secret-ids: |
      django*    # Retrieves all secrets that start with 'django'
```

当我们运行前述代码块中的 GitHub Actions 时——假设我们有三个以 `django` 开头的密钥（`djangoSecretName`、`djangoTest` 和 `django/NewSecret`）——那么将创建三个环境变量，如下所示：

```
DJANGOSECRETNAME: secretValue1
DJANGOTEST: secretValue2
DJANGO_NEWSECRET: secretValue3
```

有关如何使用 AWS Secrets Manager GitHub Actions 的更多详细信息，请查看 `https://github.com/aws-actions/aws-secretsmanager-get-secrets` 处的官方文档。

要将环境变量保存到特定文件，请执行以下操作：

```
- name: Save secret manager variables to .env
  run: printenv | grep "^DJANGO" > .env
```

前述代码将把所有以 `DJANGO` 开头的环境变量保存到 `.env` 文件中。我们可以使用 Docker 环境访问此 `.env` 文件。

至此，我们已学习如何使用本地系统的 `eb` CLI 部署新应用程序代码。接下来，我们将学习如何将相同的操作移至 GitHub Actions。

以下是使用 GitHub Actions 将应用程序代码部署到 Elastic Beanstalk 所需的所有步骤的一个小示例：

```
- name: Install aws cli and beanstalk cli
  run: pip install awsebcli awscli
- name: Get Secret Names by Prefix
  uses: aws-actions/aws-secretsmanager-get-secrets@v1
  with:
    secret-ids: |
      django*    # Retrieves all secrets that start with 'beta'
- name: Save secret manager variables to .env
  run: printenv | grep "^DJANGO" > .env
- name: git add for .env
  run: git add .
- name: Run eb init
  run: eb init django-demo-app –region ap-south-1 –platform docker
- name: Run eb use
  run: eb use prod-env –region ap-south-1
- name: Run eb deploy
  run: eb deploy –staged
```

现在，我们可以从 GitHub Actions 触发 Django 代码的部署。我们不会深入介绍代码中提到的每个步骤，因为我们在 *Integrating AWS Elastic Beanstalk to deploy **Django* 部分中已经讨论过它们。

