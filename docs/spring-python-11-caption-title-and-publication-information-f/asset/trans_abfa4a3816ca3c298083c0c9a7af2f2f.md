
-   所有 LDAP 组件都指向一个目录服务器，格式为 `ldap://<server>:<port>/<baseDN>`
-   `BindAuthenticator` 配置为从 `<baseDN>` 递归搜索，模式为 `uid={0},ou=people`，其中 `{0}` 将被提供的用户名替换。当找到 dn 时，会使用提供的密码执行绑定
-   `DefaultLdapAuthoritiesPopulator` 将在 baseDN 下一层级 `ou=groups` 中搜索，查找任何具有指向用户 dn 的 `member` 属性的条目。可以根据目录架构的不同，使用类似 `uniqueMember={0}` 的内容覆盖 `group_search_filter`。默认情况下，获取的组名将为组的 `cn`。覆盖 `group_role_attr` 允许选择另一个属性。默认情况下，组名将被转换为大写，并加上 `ROLE_` 前缀。这些可以通过 `role_prefix` 和 `convert_to_upper` 进行覆盖

若要改为进行密码检查，请将 `BindAuthenticator` 替换为 `PasswordComparisonAuthenticator`
-   默认情况下它同时支持明文密码以及 base64 编码的 SHA 哈希密码。可以通过向 `encoder` 属性提供替代密码编码器来覆盖此设置
-   它的构造函数参数与 `BindAuthenticator` 相同，这意味着你可以指定 `user_dn_patterns`
-   默认情况下，它查找存储在 `userPassword` 属性中的密码。你可以通过设置 `password_attr_name` 属性来覆盖此设置

如果你对密码认证或组管理有非传统的使用方式，可以插入你自己的 authenticator 或 authorities populater。我自己也遇到过这个问题，当时一个遗留系统将角色存储在用户记录内的自定义属性中，而不是单独的组。

凭借所有可覆盖默认设置的选择，启动 LDAP 安全管理应该相当容易。

## 使用多个安全提供者很容易

支持多个安全提供者是一个很有价值的功能。有几个常见用例在此情况下非常有益。

-   从旧的安全解决方案迁移到新的解决方案 — 这是指你已经开始使用一个安全提供者，但需要迁移到另一个提供者。例如，我们最初使用数据库存储用户账户，但希望迁移到双因素认证系统。
-   支持多个用户社区 – 这是指在两个不同服务器上运行的两个不同站点合并到一台服务器上运行。无需维护两个不同的应用程序，我们希望整合数据但保留现有的角色和账户。
-   提供冗余安全访问 – 这是在我们需要多个安全提供者来处理故障和维护窗口时。

让我们更详细地探讨这些场景。

### 从旧的安全解决方案迁移到新的解决方案

一个非常实用的用例是当你想将用户从旧的登录系统迁移到新的系统时。例如，你的项目可能有多个工具都使用各自的自定义数据库解决方案。然而，你最终希望将每个人的详细信息集中到一个公共的 LDAP 服务器中。在两个系统都启用后，随着新用户获得新凭证，他们可以无服务中断地登录。一旦所有人都迁移完成，你就可以移除旧的安全提供者。

它也使得拥有冗余的安全提供者变得容易，例如指向多个 LDAP URL。

之前，我们看到过如下所示的 AuthenticationManager 配置：

```
@Object
def auth_manager(self):
    auth_manager = AuthenticationManager()
    auth_manager.auth_providers = [self.auth_provider()]
    return auth_manager
```

AuthenticationManager 支持一个提供者列表。它会依次遍历每个提供者，直到匹配成功为止。为了支持迁移用例，可以将其修改为如下所示：

```
@Object
def auth_manager(self):
    auth_manager = AuthenticationManager()
    auth_manager.auth_providers = [
        self.old_auth_provider(),
        self.new_auth_provider()]
    return auth_manager
```

采用这种设置后，我们的系统将首先尝试使用旧的身份验证提供者。但是，如果失败，Spring Python Security 将尝试新的身份验证提供者。

### 支持多个用户社区

我们的应用程序可能需要支持多个用户。想象一下，我们已经开发了应用程序，并且也完成了 Spring Python Security 配置，基于 DatabaseUserDetailsService 处理用户。在此之后不久，我们的公司收购了另一家公司，带来了许多需要访问系统的员工。将他们迁移到新系统可能代价太高。或者至少成本太高。相反，配置另一个身份验证提供者指向他们的当前系统会容易得多。唯一需要的其他功能就是将新用户的额外角色添加到访问决策管理器中。

该解决方案看起来与之前的用例非常相似。公司现在可以轻松决定是否继续这种方式，或开始将新用户迁移到已有的系统中，如前一用例所示。

### 提供冗余安全访问

我们刚刚完成了配置 LDAP 服务器安全访问的过程。在当前版本的 Spring Python 中，它仅支持一个 URL。要在冗余解决方案中处理两个 LDAP 服务器，只需配置每个组件的两个实例，然后将其插入 AuthenticationManager 即可。

```python
from springpython.config import PythonConfig
from springpython.config import Object

from springpython.security.providers.Ldap import *

class LdapAppContext(PythonConfig):
    def __init__(self):
        super(LdapAppContext, self).__init__()

    @Object
    def auth_manager(self):
        auth_manager = AuthenticationManager()
        auth_manager.auth_providers = [
            self.auth_provider1(),
            self.auth_provider2()]
        return auth_manager

    @Object
    def auth_provider1(self):
        provider = LdapAuthenticationProvider()
        provider.ldap_authenticator = self.authenticator1()
        provider.ldap_authorities_populator =
            self.authorities_populator1()
        return provider

    @Object
    def auth_provider2(self):
        provider = LdapAuthenticationProvider()
        provider.ldap_authenticator = self.authenticator2()
        provider.ldap_authorities_populator =
            self.authorities_populator2()
        return provider

    @Object
    def context1(self):
        return DefaultSpringSecurityContextSource(
            url="ldap://server1:53389/dc=springpythonbook,dc=com")
    @Object
    def context2(self):
        return DefaultSpringSecurityContextSource(
            url="ldap://server2:53389/dc=springpythonbook,dc=com")

    @Object
    def authenticator1(self):
        return BindAuthenticator(
            context_source=self.context1(),
            user_dn_patterns="uid={0},ou=people")

    @Object
    def authenticator2(self):
        return BindAuthenticator(
            context_source=self.context2(),
            user_dn_patterns="uid={0},ou=people")

    @Object
    def authorities_populator1(self):
        return DefaultLdapAuthoritiesPopulator(
            context_source=self.context1(),
            group_search_base="ou=groups")

    @Object
    def authorities_populator2(self):
        return DefaultLdapAuthoritiesPopulator(
            context_source=self.context2(),
            group_search_base="ou=groups")
```

虽然 Spring Python 最终可能会支持多个 URL，但组合现有组件的能力为你提供了现成的可用解决方案。

## 编写自己的安全扩展

虽然本章迄今为止展示的所有功能都体现了一个强大的安全框架，但可以说最重要的功能是能够编写一个自定义安全扩展并将其插入此架构。

在 LDAP 部分中曾提到，可以调整搜索位置以查找与用户相关的组。在 SQL 部分中，展示了可以注入自定义查询。这两个功能都允许灵活调整，然而它们的弯曲程度有限。如果我们的安全系统高度专业化，那么直接编写自己的扩展并将其插入可能更容易。

如果 Spring Python 尚未支持其他安全系统，例如双因素令牌、OpenID 或 X.509 证书，也需要这样做。虽然未来可能会支持这些功能，但我们的需求可能无法等待。

### 编写自定义身份验证提供者

对于我们的示例，让我们编写一个基于登录数据库而不是进行密码比较的自定义身份验证提供者。

> 这个示例与我曾经处理过的真实情况相符。我继承了一个基于将用户登录到数据库然后查找角色的应用程序。这将密码限制为数据库所支持的格式，从而不允许我们制定统一的可接受密码策略。迁移该应用程序脱离此有限解决方案的第一步需要一个与此示例类似的自定义身份验证提供者。它使我能够利用此框架，该框架提供了插入指向新用户安全数据的另一个安全提供者的能力。

为了编写自定义安全提供者，我们需要编写具有以下签名的 authenticate 方法：

```
def authenticate(self, authentication)
```
