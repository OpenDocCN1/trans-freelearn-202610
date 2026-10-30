

基于会话的身份验证本身就是一个庞大的主题，超出了本书的范围。如果你不熟悉基于会话的身份验证概念，请阅读官方文档 [https://docs.djangoproject.com/en/stable/topics/http/sessions/](https://docs.djangoproject.com/en/stable/topics/http/sessions/)。

Django 默认提供了一个标准的 `User` 模型，其中包含以下字段：

*   `username`：必填字段，最大长度为 150 个字符。用于登录目的。请记住——这不是电子邮件地址，因此可以使用此字段作为登录的唯一标识符。我见过组织将此字段用作电子邮件、电话号码或自定义用户名，因此请随意将此字段用作任何字段。
*   `first_name`：可选字段，最大长度为 150 个字符。
*   `last_name`：可选字段，最大长度为 150 个字符。
*   `email`：可选字段，可用于捕获用户的电子邮件地址。
*   `password`：必填字段，保存所设置密码的哈希值。Django 不会在数据库中存储原始密码。
*   `is_staff`：布尔字段，用于检查用户是否可以访问管理站点。
*   `is_active`：布尔字段，用于识别用户账户是否已启用。如果将 `is_active` 字段设置为 `False`，则 Django 提供的默认 `ModelBackend` 身份验证将不允许用户登录。*请注意，如果你正在创建任何自定义登录系统，则除非你已实现自定义逻辑来检查该标志，否则它将不会被尊重。*
*   `is_superuser`：布尔字段，应谨慎使用。如果为任何用户设置此标志，则他们可以完全访问所有 Django 管理部分。
*   `last_login`：日期时间字段，捕获用户上次登录的时间。*请注意，此字段仅在使用 `ModelBackend` 的默认登录机制时更新。*
*   `date_joined`：日期时间字段，捕获用户账户创建时的日期时间。
*   `groups`：与 `Group` 表的多对多关系。我们将在 *Using Django permissions and groups* 部分讨论 *groups* 概念。当我们使用 Django 授权为用户提供访问控制时，这特别有帮助。Django 使用此字段在后端提供 RBAC 支持。
*   `user_permissions`：与 `Permission` 表的多对多关系。Django 将权限与组结合使用，以在管理门户或 API 级别权限上提供 RBAC，使用 `has_perm` 方法。我们将在 *Using Django permissions and groups* 部分讨论 *permissions* 概念。

上述所有字段都是标准的，对于任何基础项目都足够了。许多组织或项目可能有自定义需求，需要与给定用户关联的额外字段，这时我们可以自定义 `User` 模型。在以下部分，我们将学习在 Django 中自定义 `User` 模型的不同方法。

# 自定义 User 模型

主要可以通过两种方法来自定义 `User` 模型。两者都有各自的优势，让我们一个一个地学习：

*   **通过一对一关系扩展 User 模型**：每当需要向用户添加额外字段时，这是一种最简单和最好的解决方案。在这种方法中，我们将创建一个新模型，例如 `UserProfile`，然后与 `User` 模型创建一对一关系。任何新的自定义字段都将添加到 `UserProfile` 模型中。我建议大多数情况下使用这种方法，因为这是最简单、最干净的解决方案之一，不需要开发人员付出太多努力，并且满足我们的需求。
*   **使用 AbstractUser 和 AbstractBaseUser 模型扩展**：这是扩展 `User` 模型的另一种方法。在这种方法中，我们必须创建我们自己的自定义 `User` 模型并继承自 `AbstractUser` 类或 `AbstractBaseUser` 类。这些模型之间的关键区别是：
    *   `AbstractUser`：仅当你想更新 `User` 表中的用户名字段时才使用 `AbstractUser` 模型。
    *   `AbstractBaseUser`：如果你想从头开始在 `User` 表中创建所有字段，请使用 `AbstractBaseUser` 模型。

建议仅在你对 Django 有充分理解并熟悉登录和身份验证工作流程时才使用自定义模型方法。

## 使用 AbstractUser 或 AbstractBaseUser 创建自定义 User 模型

现在，让我们学习如何在 Django 项目中使用 `AbstractUser` 或 `AbstractBaseUser` 模型创建自定义 `User` 模型：

1. 创建一个新的 Django 应用，`custom_user`。
2. 然后，定义一个 `CustomUser` 模型，具有适当的字段，例如 `phone_no` 和 `city`，继承 `AbstractUser` 类。
3. 接下来，将 `username` 字段设置为 `None`。我们还需要从 `BaseUserManager` 定义一个自定义模型管理器：

```
from django.contrib.auth.base_user import BaseUserManager
from django.contrib.auth.models import AbstractUser
from django.db import models
class CustomUserManager(BaseUserManager):
    """
    Used for updating default create user behaviour
    """
    def create_user(self, phone_no, password, **kwargs):
        # implement create user logic
        user = self.model(phone_no=phone_no, **kwargs)
        user.set_password(password)
        user.save()
        return user
    def create_superuser(self, phone_no, password, **kwargs):
        # creates superuser.
        self.create_user(phone_no, password)
class CustomUser(AbstractUser):
    username = None
    phone_no = models.CharField(unique=True, max_length=20)
    city = models.CharField(max_length=40)
    USERNAME_FIELD = "phone_no"
    objects = CustomUserManager()
```

前面的代码片段定义在 `custom_user/models.py` 文件中。在代码片段中，我们创建了一个 `CustomUser` 模型和一个 `CustomUserManager` 模型管理器，并将 `username` 逻辑更新为 `phone_no`。这意味着，从现在开始，我们可以利用 `phone_no` 字段以及密码登录到我们的 Django 应用程序。

4. 一旦我们创建了模型，我们需要告诉 Django 为任何身份验证目的使用新模型。我们需要进入 `settings.py` 文件，然后定义一个新变量：

```
AUTH_USER_MODEL = "custom_user.CustomUser"
```

请注意，`"custom_user"` 是我们自定义应用的名称，其中定义了 `CustomUser` 模型。

5. 现在，运行 Django 迁移命令来创建我们的新 `User` 模型。
6. 我们还需要更新 Django Admin 表单，以便我们的 Django Admin 工具正常工作。我们不会深入表单的细节，因为这超出了本书的范围。

### 阅读更多

Django 的官方文档提供了明确的示例；请在此处查看 [https://docs.djangoproject.com/en/stable/topics/auth/customizing/#a-full-example](https://docs.djangoproject.com/en/stable/topics/auth/customizing/#a-full-example)。

我们已经学习了 Django 如何开箱即用地提供 `User` 模型，以及如何自定义 `User` 模型以适应我们的用例。

尽管我们已经学习了如何在 Django 中使用自定义 `User` 模型，但建议创建一个单独的配置文件模型来捕获所有额外的用户信息，并*使用一对一关系*与默认的 `User` 模型关联，以便 Django 可以处理所有身份验证和其他安全功能。使用这种方法，开发人员可以专注于产品功能开发，而无需过多参与身份验证的框架级更改。

