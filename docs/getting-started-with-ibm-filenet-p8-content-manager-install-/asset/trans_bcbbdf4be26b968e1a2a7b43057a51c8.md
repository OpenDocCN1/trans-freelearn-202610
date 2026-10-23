

点击 OK 应用更改。退出控制台管理工具，然后返回到登录页面。你会发现它变成了更符合预期的样子。

![](img/2295191c450a0c86b2d82df6cec04526_78_0.png)

你现在可以使用我们之前配置的凭据 cn=root 登录到目录管理工具（不是控制台配置工具）。如果你需要返回控制台配置工具，只需点击 Login to Console admin 链接即可。

如果点击 Login 按钮，你将被带到 Tivoli Directory Server Web Administration Tool，我们将通过该工具完成所有的目录配置和数据录入。

![](img/2295191c450a0c86b2d82df6cec04526_78_1.png)

## Server-side sorting

作为 CE 的要求，需要配置 TDS 以允许搜索结果的服务器端排序。这通常默认启用，但你可以通过 TDS Web 管理应用程序进行检查。导航到 **Server administration | Manage server properties | Search settings** 并确保 **Allow only administrators to perform sort searches** 复选框未被选中。你不想将其限制为管理员；你希望允许任何人进行排序搜索。

## The suffix

TDS 可以保存任意数量的数据逻辑分区，每个分区都位于唯一的 **suffix** 下。后缀只是在其下所有其他条目都落入的属性值集合。我们的组织将企业目录称为 "WhoZit?"，因此我们将后缀设为 dc=whozit,dc=example,dc=net。

> 目录概念和记法看起来可能很奇怪。大多数人不会每天遇到它们。网上有大量材料可以解释概念和记法。这里有一个例子写得清晰，面向目录新手：http://www.skills-1st.co.uk/papers/ldap-schema-design-feb-2005/index.html。

你可以通过基于 Web 的管理工具向 TDS 添加后缀。导航到 **Server administration | Manage server properties | Suffixes**。在 **Suffix DN** 框中输入完整的后缀，然后点击 **Add** 按钮。务必同时点击 **OK** 或 **Apply** 按钮以提交更改（很容易遗漏）。

> 你可能还需要在新后缀在目录中显示之前重启目录服务器。导航到 **Server administration | Stop/start/restart server。**

## Users

目录管理中的一个良好做法是为个人添加所有条目采用扁平结构，然后从随时间可能发生变化的组、组织实体和其他结构中引用这些条目。其思想是无论目录中需要什么其他更改，用户 Distinguished Name 都保持不变。我们将定义一个名为 cn=people 的节点来保存所有人员条目，无论其组织或其他隶属关系如何。导航到 Directory management | Add an entry。从结构对象类列表中选择 groupOfNames 并点击 Next。我们使用 groupOfNames 类是因为它在 TDS 管理工具用户界面中为我们触发了一些便利；请注意不要意外选择 groupOfUniqueNames。对于这个简单的容器节点，我们不需要任何额外属性，因此点击 Next 跳过辅助对象类面板。这将带你进入 Required attributes 面板。

![](img/2295191c450a0c86b2d82df6cec04526_80_0.png)

输入我们新容器的名称 cn=people 作为 Relative DN，并将 people 也作为 cn 属性值输入。因为 groupOfNames 必须至少有一个成员，请在该字段中添加节点本身。你可以浏览以选择 Parent DN 的 LDAP 后缀。

> TDS 管理工具某些版本的怪癖是，新创建的 LDAP 后缀在某些地方要到其下至少有一个条目后才会显示。如果这种情况发生在你身上，只需将值 dc=whozit,dc=example,dc=net 输入到该字段即可；它将通过 Browse 按钮在所有未来操作中可用。

你可以将下一屏幕上出现的可选属性字段留空。点击 Finish 提交更改后，TDS 会询问你是否要添加类似的条目。你可以通过点击 Yes 为自己节省一点工作。创建一个用于保存组定义的容器，并将其 Relative DN 设为 cn=groups。我们现在有了两个特殊用途的目录节点 – cn=people,dc=whozit,dc=example,dc=net 用于用户，以及 cn=groups,dc=whozit,dc=example,dc=net 用于组。

要添加单个用户，请返回 Directory management | Add an entry。这次，选择结构对象类 person。另一个流行选择是 inetOrgPerson，因为它有许多额外的可选属性，但就我们的目的而言，person 类更简单且足够。重要的是用户条目具有 userPassword 属性，因为它将是 CM 的登录密码以及其他各种用途的密码。

对于 Relative DN，提供对给定用户唯一的值。这通常是员工 ID 号或电子邮件地址。它也将是登录用户 ID，因此应是用户有意义且不保密的值。在下一屏幕中，我们使用了姓名首字母（这对示例来说是可以的，但如果你的企业有超过几个人则可能很糟糕）。重要的是该值永久唯一，因为 Relative DN 和 Parent DN 的组合构成了用户的完整 Distinguished name。目录不允许你创建具有重复值的条目，这是使用用户扁平列表的优点之一。

![](img/2295191c450a0c86b2d82df6cec04526_81_0.png)

尽管定义为可选，但请务必为每个条目提供 userPassword 属性的值。在真正的企业目录中，你可能会自定义使其 userPassword 属性成为必需。

继续创建一两个个人用户条目。随意尝试该工具；你以后随时可以删除条目。我们将在本节稍后返回填充 CM 特定信息。

## Groups

让我们至少创建一个目录组来看看如何操作。我们之前添加了节点 cn=groups,dc=whozit,dc=example,dc=net。导航到该节点，选中 cn=groups 旁边的复选框，并从下拉菜单中选择 Add 以在该节点下直接放置新条目。使新条目成为 groupOfNames 并将其 Relative DN 设为 cn=vampires。（有人告诉我们，如果我们提到吸血鬼就会卖出更多书，但我们认为只有在他们年轻、漂亮且情绪化时才有效。我们拭目以待。）同样，将新条目添加为其自身的成员。当你完成并提交更改后，你应该能够导航到新节点 cn=vampires,cn=groups,dc=whozit,dc=example,dc=net。从组列表中选择该节点，如下一张屏幕截图所示。从 Select Action 下拉菜单中选择 Manage Members 并点击 Go。

![](img/2295191c450a0c86b2d82df6cec04526_82_0.png)

你将被带到一个屏幕，可以通过输入其 Distinguished names 或在目录中浏览来添加组成员。正如你所料，你也可以导航到个人条目并选择 Manage Memberships 来从该角度管理事物。在进入下一小节之前随意尝试。

## CE-specific entries

对于 CE 的安装和初始配置，需要少量用户和组。通常会指定现有的目录条目用于这些用途，但对于我们的全包环境，我们将创建新的专用条目。这将帮助你了解哪些账户用于哪些目的。

下表总结了你现在应使用刚刚描述的流程创建的用户和组。所示的具体用户或组 ID 只是建议，并与 PPG 和安装工作表中使用的比喻名称相匹配。如果你愿意，可以使用不同的名称。务必记录你为这些账户分配的密码。虽然对 CE 目的不是必需的，但你可能喜欢将这些特殊账户分配给一个描述性命名的组以便于简单记账。例如，我们有时在设置中使用名为 pseudousers 的组。

| User | Group | Comment |
|---|---|---|
| `ce_bootstrap_admin` |  | 使其成为不用于其他任何用途的专用账户，并且密码永不过期。CE 用于初始访问 GCD。 |
| `gcd_admin` |  | 域管理员。 |
| `object_store_admin` | `object_store_admin_group` | ObjectStore 管理员。 |
| `ce_service_user` |  | CE 用于执行 LDAP 目录查找。 |
| `was_admin` |  | 在我们将其更改为使用 TDS 进行身份验证后，使用此用户 ID 登录 WAS 管理控制台。 |

如果你遵循本节中描述的模式，你将拥有像 cn=ce_bootstrap_admin,cn=people,dc=whozit,dc=example,dc=net 这样的用户 Distinguished name。不要忘记将用户 object_store_admin 放入组 object_store_admin_group 中。

