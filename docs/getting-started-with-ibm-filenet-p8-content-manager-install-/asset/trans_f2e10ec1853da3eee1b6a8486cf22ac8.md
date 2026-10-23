

## PE 客户端软件

稍后，我们将在 XT 环境中安装 PE 客户端软件。由于 CE 服务器与 XT 在同一台物理机器上，PE 客户端安装程序会同时检测到两者并一次性完成更新。如果由于某种原因，你既不安装 AE 也不安装 XT，那么你必须为 CE 服务器的使用安装 PE 客户端软件。请按照下面描述的步骤为 XT 安装 PE 客户端软件。

## 应用引擎 (AE)

AE 的术语可能有点令人困惑。一方面，你安装 AE 是为了部署 Workplace 应用程序。Workplace 是主要开箱即用的面向终端用户的应用程序。另一方面，AE 也是可以构建自定义应用程序的平台。Workplace 的许多功能可以通过自定义 Model-View-Controller (MVC) 框架中的集成点直接利用。AE 包含用于构建此类应用程序的工具包：Web Application Toolkit (WAT)。

如果你计划在环境中使用全部自定义应用程序，你需要安装和部署 AE（或 XT）吗？不需要；也就是说，除非你的应用程序是基于 WAT 构建的。如果你的应用程序是直接基于 CE 和 PE API 构建的，那么你可以跳过 AE 的安装。然而，即使是这种情况，我们也建议你在开发环境中至少安装 AE 或 XT。拥有一个开箱即用的工具来验证这样或那样的细节，或与你自己的自定义编码工作进行比较，是非常方便的。

由于 AE 和 XT 在 CM 生态系统中的位置相似，我们将跳过 AE 的安装，直接进入下一节安装 XT。如果你想在环境中安装 AE，你会发现安装过程与 XT 安装非常相似。特别是，LDAP 目录准备步骤完全相同，如果你将 AE 和 XT 安装到同一个 WAS 配置文件中，则只需执行一次。其他章节中的功能讨论将侧重于 XT 而不是 AE，尽管在许多情况下功能是等效的。

## Workplace XT (XT)

Workplace XT 是一个开箱即用的 Web 应用程序，用于访问许多 CM 功能。你可以浏览、搜索并直接操作 CE 中的许多类型对象，并且还可以参与 PE 中运行的工作流。XT 在 J2EE Web 容器中运行。由于它通过 EJB 传输与 CE 通信，XT 必须在与 CE 服务器相同品牌和版本的应用程序服务器上运行。

AE 中的 Workplace 应用程序（现在非正式地称为“Workplace classic”）最初有在 HTML 3.2 环境中工作的限制。XT 另一方面，使用更现代的 UI 范式，具有所谓的 Web 2.0 的外观和感觉。XT 开发人员以 Workplace classic 代码库为起点，按子系统逐步现代化 UI。尽管 XT 中仍保留了 Workplace classic 代码的几个部分，但大部分已被重写。XT 不包含用于自定义应用程序集成的完整 WAT，但仍有几个钩子可以添加自定义菜单项和操作。XT 使用行业标准的 MVC 框架，因此对其进行增强比对 Workplace 执行相同操作更少专业化。

我们将在与 CE 相同的物理服务器上，在其自己的 WAS 配置文件中安装 XT。不支持将它们安装到同一配置文件中（由于性能和功能原因）。在生产环境中，除轻负载环境外，你可能不会在同一台物理服务器上安装 XT 和 CE。拆分它们可以让你更好地控制负载均衡及其各自的扩展行为。

XT 的安装过程包括：

- 准备步骤（LDAP 目录和信任关系）
- 安装软件
- 配置和部署软件

## 配置 LDAP

请回忆在第 3 章中我们将 CE 服务器安装到配置文件 AppSrv02。现在我们将 XT 安装到配置文件 AppSrv01。XT 必须使用与 CE 服务器相同的 LDAP 目录设置，因此我们的第一步将确保 AppSrv01 与我们在 AppSrv02 中已完成的 LDAP 设置相匹配。有几种方法可以做到这一点。如果你是 WAS 专家，你可以只将 WAS 配置文件 AppSrv02 的相关部分复制到 AppSrv01。你也可以使用配置管理器（如我们在第 3 章 CE 安装中所做的那样），将其重新定位到 AppSrv01，并应用 LDAP 配置。我们将改为通过 WAS 管理控制台手动配置。

> 由于这是两个单独的配置文件，我们理论上可以在单独的浏览器窗口中同时打开控制台，这将便于并排比较。实际上，这可能会混淆保存会话信息的浏览器 cookie，让你有点抓狂。如果你安装了两个不同的浏览器，例如 Firefox 和 Internet Explorer，你可以在每个浏览器中打开一个控制台。

1. 登录 AppSrv01 的 WAS 控制台。导航到 Security | Global Security。
2. 在 User account repository 部分，从下拉列表中选择 Standalone LDAP registry，然后单击 Configure。
3. 单击 Advanced Lightweight Directory Access Protocol (LDAP) user registry settings。
4. 在配置面板中输入以下值：
   - User filter: `&&(cn=\*)(objectclass=person))`
   - Group filter: `&&(cn=\*)(|(objectclass=groupOfNames) (objectclass=groupOfUniqueNames) (objectclass=groupOfURLS)))`
   - User ID map: `*:uid`
   - Group ID map: `*:cn`
5. 单击 OK 返回到上一个面板。
6. 在配置面板中输入以下值：
   - Primary administrative user name: `was_admin`
   - Type of LDAP server: IBM Tivoli Directory Server
   - Host: `wjc-rhe1.example.net`
   - Port: `389`
   - Base distinguished name: `dc=whozi t,dc=example,dc=net`
   - Bind distinguished name: `cn=ce_service_user, cn=people, dc=whozi t,dc=example,dc=net`
7. 单击 Test connection 按钮。如果由于任何原因目录连接不成功，你必须在此点纠正它。如果你不纠正任何问题就继续，你可能会将自己锁定在该配置文件的 WAS 控制台之外。（不要问我们是怎么知道的。）
8. 单击 OK 返回到 Global security。
9. 单击 Set as current 以使此配置成为当前领域定义。
10. 选择 Enable administrative security 和 Enable application security 的复选框。取消 Java 2 security 部分中的所有复选框。
11. 单击 Apply。
12. 在屏幕顶部，你将看到保存或查看更改的链接。单击 Save。

如果你退出 WAS 控制台然后重新登录，你会注意到你仍在使用旧凭据。你必须重启 WAS 配置文件 AppSrv01 才能使 LDAP 更改生效；请执行此操作。从这一点开始，你必须使用 userid `was_admin` 登录 AppSrv01 的 WAS 控制台，就像你一直为 AppSrv02 所做的那样。

## 信任关系和 LTPA

在用户身份验证期间，将在 XT 应用程序服务器上创建一个名为 JAAS Subject 的对象。该 Subject 必须被 CE 应用程序服务器信任。不同的供应商使用不同的技术来实现这种所谓的信任关系。

WebSphere 使用的技术涉及一种称为 Lightweight Third Party Authentication (LTPA) 的机制。通常的做法是从 CE 服务器导出 WAS LTPA 密钥并将其导入 XT 服务器。

要导出 LTPA 密钥，请使用 AppSrv02 的 WAS 控制台。导航到 Security | Global Security。在 Authentication 框中，确保选择了 LTPA，然后单击 LTPA 链接。

![](img/2295191c450a0c86b2d82df6cec04526_174_0.png)

在 Cross-cell single sign-on 框中提供请求的密码和文件名，然后单击 Export keys。

![](img/2295191c450a0c86b2d82df6cec04526_174_1.png)

