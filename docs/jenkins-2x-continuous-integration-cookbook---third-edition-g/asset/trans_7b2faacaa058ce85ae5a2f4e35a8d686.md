

## 搜索结果

服务器：我的 LDAP 服务器
查询：默认

ou=groups,dc=jenkinscookbook,dc=net

找到条目数：2
（0.01 秒）

基础 DN：ou=groups,dc=jenkinscookbook,dc=net
执行的过滤器：objectClass=*

cn=jenkinsadmin
dn: cn=jenkinsadmin,ou=groups,dc=jenkinscookbook,dc=net
cn: jenkinsadmin
gidNumber: 500
objectClass: posixGroup
objectClass: top

cn=jenkinsusers
dn: cn=jenkinsusers,ou=groups,dc=jenkinscookbook,dc=net
cn: jenkinsusers
gidNumber: 501
objectClass: posixGroup
objectClass: top

## ou=users

服务器：我的 LDAP 服务器
专有名称：ou=users,dc=jenkinscookbook,dc=net
模板：默认

- 刷新
- 切换模板
- 复制或移动此条目
- 重命名
- 创建子条目

- 显示内部属性
- 导出
- 删除此条目
- 与其他条目比较
- 添加新属性

提示：要创建子条目，请清空文本字段并点击保存。
提示：要查看属性的模式，请点击属性名称。

objectClass
必填
organizationalUnit（结构类型）
top
（添加值）

ou
必填，添加
users
（添加值）

现在该创建用户了。点击“在此创建新条目”。

选择“通用：用户账户”：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_231_0.png)

如果在 `phpldapadmin` 中创建通用用户账户时在设置密码字段时遇到错误，请以编辑模式打开 `/usr/share/phpldapadmin/lib/TemplateRender.php`，并在第 2469 行将 `password_hash` 改为 `password_hash_custom`：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_232_0.png)

提供密码和用户 ID 详细信息，然后点击“创建对象”：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_233_0.png)

点击“提交”：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_233_1.png)

同样，创建另一个用户 ID 为 `ssoni` 的用户：

## 创建 LDAP 条目

是否要创建此条目？

| 属性 | 新值 | 跳过 |
|---|---|---|
| cn=Shreyu Soni,ou=users,dc=jenkinscookbook,dc=net |  |  |
| 通用名称 | Shreyu Soni |  |
| 名 | Shreyu |  |
| GID 编号 | 501 |  |
| 主目录 | /home/users/soni |  |
| 姓 | Soni |  |
| 登录 Shell | /bin/sh |  |
| objectClass | inetOrgPerson<br>posixAccount |  |
| 密码 | *************** |  |
| UID 编号 | 1001 |  |
| 用户 ID | ssoni |  |

现在，组和用户都已就绪。下一步是将用户分配到组中。

点击“添加新属性”。选择 memberUid：

点击“保存更改”：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_237_0.png)

点击“更新对象”：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_237_1.png)

现在我们已经有了可用用户，可以开始将 OpenLDAP 与 Jenkins 集成。

- 5. 在“授权”部分，转到“基于项目的矩阵授权策略”，并添加我们在 OpenLDAP 中创建的用户：

- 6. 保存更改后，Jenkins 将退出登录。使用你的 LDAP 用户凭据登录，即可成功登录：

- 7. 仅授予 ssoni 用户读取权限以验证访问：

授权

- 任何人都可以做任何事
- 传统模式
- 已登录用户可以做任何事
- 基于矩阵的安全
- 基于项目的矩阵授权策略

- 8. 使用权限受限的 ssoni 用户登录，可以看到“管理 Jenkins”部分不可用：

- 9. 点击 Jenkins 仪表板上可用的项目，确认只有读取权限可用：

## Jenkins

打开 Blue Ocean

搜索

ssoni | 退出登录

Jenkins > FirstAndroidProject

返回仪表板

状态

变更

收藏

## 项目 FirstAndroidProject

### 最近的变更

### 永久链接

### 构建历史

趋势

查找

所有 RSS   失败 RSS

## Jenkins 与 Active Directory 集成

在本节中，我们将配置 Active Directory，它已可用于与 Jenkins 集成，从而使 Active Directory 特定域中的所有可用用户都可以根据所授予的访问权限，使用他们自己的 Active Directory 凭据来使用 Jenkins。

### 操作步骤...

- 1. 前往 Jenkins 仪表板 | 管理 Jenkins | 配置全局安全 | 启用安全 | 选择 Active Directory：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_246_0.png)

- 2. 选择“指定自定义 Active Directory 域名”，点击“添加域”，提供域名，然后保存：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_246_1.png)

- 3. 如果配置和访问均正确，那么我们可以直接在 Jenkins 中使用该域中的所有用户。

## Jenkins 与 OWASP Zed Attack Proxy 集成

**OWASP Zed Attack Proxy（ZAP）**是一款开源 Web 应用安全扫描器。你可以将 ZAP 安全工具与 Jenkins CI 环境集成。

### 准备工作

前往 https://github.com/zaproxy/zaproxy/wiki/Downloads 下载 Windows（64 位）安装程序。

在 Windows 上安装它。

在 Windows 中打开 OWASP ZAP，点击“文件 | 持久化会话”，并将其保存到 Jenkins 工作空间中该构建作业的目录下：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_248_0.png)

现在你可以开始配置 Jenkins 中的 ZAP 插件了。

### 操作步骤...

- 1. 前往 Jenkins 仪表板 | 配置系统 | 全局属性 | 环境变量，创建 `ZAPPROXY_HOME` 变量：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_249_0.png)

- 2. 前往 Jenkins 仪表板 | Zap 项目 | 构建 | 添加构建步骤 | 执行 ZAP。
- 3. 将主机和端口设置保留为默认值，或根据你的 OWASP ZAP 安装情况进行更改：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_249_1.png)

- 4. 提供 `ZAPPROXY_HOME`：

- 5. 提供 ZAP 主目录的路径，注意这不是安装目录。

- 6. 提供你保存在工作空间中的会话名称：

- 7. 配置攻击模式：

- 8. 配置“生成报告”部分：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_251_0.png)

- 9. 发布 HTML 报告。点击“保存”：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_251_1.png)

### 另请参阅

- 运行 Jenkins 构建，它将创建 HTML 报告。导航到 HTML 报告并验证扫描报告：

## ZAP 扫描报告

### 警报摘要

| 风险级别 | 警报数量 |
|---|---:|
| 高 | 1 |
| 中 | 1 |
| 低 | 2 |
| 信息 | 0 |

### 警报详情

| 字段 | 值 |
|---|---|
| 高（中） | SQL 注入 |
| 描述 | 可能存在 SQL 注入。 |
| URL | `http://localhost:9999/petclinic/oups.htm;jsessionid=E47011BD0E9AC2E848D2F180797D2E7E?query=query%22+AND+%221%22%3D%221` |
| 方法 | GET |
| 参数 | query |
| 攻击 | `query" AND "1"="1` |
| URL | `http://localhost:9999/petclinic/oups.htm;jsessionid=E47011BD0E9AC2E848D2F180797D2E7E?query=query+AND+1%3D1+--` |
| 方法 | GET |
| 参数 | query |
| 攻击 | `query AND 1=1 --` |
| URL | `http://localhost:9999/petclinic/oups.htm;jsessionid=E47011BD0E9AC2E848D2F180797D2E7E?query=query+AND+1%3D1+--+` |
| 方法 | GET |

- 你已将 Jenkins 与 OWASP ZAP 集成用于安全测试，也可以将其集成到流水线中。

## 测试 OWASP 的十大安全问题

本教程详细介绍了使用 w3af 对 Jenkins 进行自动测试以发现已知安全问题的方法。w3af 是来自 **开放 Web 应用程序安全项目（OWASP）** 的渗透测试工具。更多信息，请访问 [http://w3af.sourceforge.net](http://w3af.sourceforge.net)。OWASP 的目标是让应用安全可见。OWASP 2010 年的十大不安全清单包括：

- **A2-跨站脚本（XSS）：** 当应用程序将未转义的输入返回给客户端浏览器时，就可能发生 XSS 攻击。默认情况下，Jenkins 管理员可以通过作业描述执行此操作。

- **A6-安全配置错误：** Jenkins 插件让你能够编写自定义身份验证脚本。通过错误配置很容易把脚本弄错。

- **A7-不安全的加密存储：** Jenkins 有 600 多个插件，每个插件都将配置存储在单独的 XML 文件中。极有可能出现一种罕见错误：以明文形式存储密码。你需要仔细检查。

- **A9-传输层保护不足：** 默认情况下，Jenkins 通过 HTTP 运行。获取受信任的证书可能很麻烦，而且涉及额外成本。你可能会想不实施 TLS，从而使数据包暴露在外。

你会发现，与 2010 版本相比，OWASP 2013 年的十大不安全清单有些变化。最重要的变化是加入了 **A9-使用已知易受攻击组件**。如果你的软件依赖于较旧的库，那么就存在利用已知弱点进行操纵的机会窗口。

Jenkins 拥有大量由积极、分散且勤奋的社区编写的插件。由于代码变动频繁，可能会无意中加入安全缺陷。例如，在配置文件中以明文形式保留密码，或使用不会移除可疑 JavaScript 的不安全渲染。你可以通过手动检查配置文件来发现第一类缺陷。第二类缺陷面向更广泛的受众，因此更容易被破解。你可以手动攻击新插件。互联网上有有用的速查表（[http://hackers.org/xss.html](http://hackers.org/xss.html)）。这项工作枯燥乏味；自动化测试可以覆盖更多内容，并可以作为 Jenkins 作业的一部分进行调度。

在名为 *探索 OWASP dependency-check 插件* 的教程中，你将配置 Jenkins，使其基于对代码依赖项的自动审查，向你发出已知攻击媒介的警告。

### OWASP 店面：

OWASP 每年都会发布一份针对 Web 应用的十大最常见安全攻击媒介清单。他们通过 [http://lulu.com](http://lulu.com) 发布这份文档以及大量书籍。在 Lulu，你可以免费访问 OWASP 文档的 PDF 版本，也可以购买廉价的按需印刷版本。官方店面位于：[http://stores.lulu.com/owasp](http://stores.lulu.com/owasp)。

