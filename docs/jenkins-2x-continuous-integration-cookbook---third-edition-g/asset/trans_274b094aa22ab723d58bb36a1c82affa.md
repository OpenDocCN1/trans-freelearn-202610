

## 创建 HTML 报告

作业仪表盘左侧菜单是宝贵的空间。开发者的目光自然会扫向该区域。本食谱描述了如何从自定义 HTML 报告添加一个链接到菜单，使报告能更快被注意到。

### 准备工作

安装 HTML Publisher 插件 (https://wiki.jenkins-ci.org/display/JENKINS/HTML+Publisher+Plugin)。我们假设你有一个 Subversion 仓库，其中已提交 Packt 的代码。

### 如何操作...

- 1. 创建一个自由风格软件项目，并将其命名为 `CH9.html_report`。
- 2. 在源代码管理部分，点击 Subversion。
- 3. 在模块部分，将 `Repo/CH9.communicating/html_report` 添加到仓库 URL，其中 `Repo` 是你的 Subversion 仓库的 URL。
- 4. 在构建后操作部分，勾选发布 HTML 报告。添加以下详细信息：
  - 要归档的 HTML 目录：`target/custom_report`
  - 索引页[s]：`index.html`
  - 报告标题：`My HTML Report`
  - 勾选保留过去的 HTML 报告复选框
- 5. 点击保存。
- 6. 运行作业并查看左侧菜单。你现在会看到指向报告的链接，如以下屏幕截图所示：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_562_0.png)

### 工作原理...

你的 Subversion 仓库包含一个 `index.html` 文件，该文件会被拉取到作业的工作区中。插件按广告所述工作，并添加一个指向 HTML 报告的链接。这使你的受众能够高效地找到你自定义生成的信息。

### 更多内容...

示例报告如下所示：

```
```html
<html><head><title>Example Report</title>
<link rel="stylesheet" type="text/css" href="/css/style.css" /></head>
<body>
<h2>Generated Report</h2>
Example icon: <img title="A Jenkins Icon" alt="Schedule a build" src="/images/24x24/clock.png"
/>
</body></html>
```
```

它引入了主 Jenkins 样式表 `/css/style.css`。

有可能当你在应用程序中更新样式表时，直到你清理浏览器缓存后才会在浏览器中看到更改。Jenkins 以一种巧妙的方式解决了这种延迟问题。它使用一个随每次 Jenkins 版本更改而变化的唯一数字的 URL。例如，对于 `css` 目录，你有两个 URL：

- `/css`
- `/static/uniquenumber/css`

大多数 Jenkins URL 使用后一种形式。建议对你的样式表也这样做。

唯一数字会随版本更改，因此每次升级时你都需要更新 URL

在 Maven 构建中运行 site 目标时，会生成一个本地网站 (http://maven.apache.org/plugins/maven-site-plugin)。该网站在 Jenkins 作业内部有一个固定的 URL，你可以用 My HTML Report 链接指向它。这使诸如测试结果等文档触手可及。

### 另请参阅

- *Efficient use of views* 食谱
- *Saving screen space with the Dashboard View plugin* 食谱

## 高效使用视图

Jenkins 上瘾般的易配置性使其容易创建大量作业。这增加了向开发者展示的信息量。Jenkins 需要通过高效利用浏览器空间来避免混乱。一个方法是定义最小化视图。在本食谱中，你将使用 `DropDownViewsTabBar` 插件。它将视图作为标签移除，并用一个选择框替换标签。这有助于更快导航。你还将学习如何使用脚本生成的简单 HTML 表单快速配置大量作业。

在本食谱中，你将创建大量可能稍后想要删除的视图。如果你使用的是虚拟机镜像，请考虑克隆镜像，并在完成后删除它。

### 准备工作

安装 `DropDownViewsTabBar` 插件 (https://wiki.jenkins-ci.org/display/JENKINS/DropDown+Views+TabBar+Plugin)。

### 如何操作...

- 1. 复制并粘贴以下 Perl 脚本到名为 create.pl 的可执行文件中：
- 2. 从 Perl 脚本的输出创建一个 HTML 文件，例如：
- 3. 在 Web 浏览器中，以管理员身份登录 Jenkins。
- 4. 创建 Template1 作业，添加你想要的任何详细信息。这是你的模板作业，将被复制到许多其他作业中。
- 5. 将 form.html 加载到同一浏览器中。
- 6. 点击其中一个 Create CH9.fake 按钮。Jenkins 返回错误消息：
- 7. 访问 http://localhost:8080/configureSecurity 上的配置全局安全，并取消勾选防止跨站请求伪造攻击复选框。
- 8. 点击保存。
- 9. 点击所有 Create CH9.fake 按钮。
- 10. 访问 Jenkins 的首页并验证作业是否已创建，并且基于 Template1 作业。
- 11. 创建大量包含随机作业选择的视图。查看首页，注意混乱情况。
- 12. 访问配置屏幕 /configure；选择 DropDownViewsTabBar 提供一个用于在视图标签栏选择框中选择视图的下拉菜单。在 DropDown ViewsTabBar 子部分中，勾选显示作业计数框，如以下屏幕截图所示：

```
#!/usr/bin/perl
$counter=0;
$end=20;
$host='http://localhost:8080';
while($end > $counter){
    $counter++;
    print "<form action=$host/createItem?mode=copy method=POST>\n";print "<input type=text name=name value=CH9.fake.$counter>\n";
    print "<input type=text name=from value=Template1 >\n";
    print "<input type=submit value='Create CH9.fake.$counter'>\n";
    print "</form><br>\n";
    print "<form action=$host/job/CH9.fake.$counter/doDelete method=POST>\n";print "<input type=submit value='Delete CH9.fake.$counter'>\n";
    print "</form><br>\n";
}
```

```
perl create.pl > form.html
```

```
HTTP ERROR 403
Problem accessing /createItem. Reason:
No valid crumb was included in the request
```

![](img/2a4d2e09d9af2c678ea8f43ea344059d_568_0.png)

- 13. 点击保存按钮：
- 14. 在 Jenkins 中，访问 `http://localhost:8080/configureSecurity` 配置全局安全并勾选防止跨站请求伪造攻击复选框。
- 15. 点击保存。

### 工作原理...

只要 Jenkins 中的面包屑安全功能关闭，表单就能正常工作。该功能在开启时，会生成一个随机数，表单在提交时必须返回该数。这使 Jenkins 能知道表单是与服务器有效对话的一部分。生成的 HTTP 状态错误在 4xx 范围内，这意味着客户端输入无效。如果 Jenkins 返回 5xx 错误，则意味着服务器错误。因此我们在提交自己的数据时必须关闭该功能。我们不建议在生产环境中这样做。

一旦你以管理员身份登录 Jenkins，你就可以创建作业。你可以通过 GUI 或发送 POST 信息来完成。在本食谱中，我们将名为 `Template1` 的作业复制到以 `CH9.fake` 开头的新作业中，如以下代码所示：

```
<form action=http://localhost:8080/createItem?mode=copy
method=POST>
<input type=text name=name value=CH9.fake.1>
<input type=text name=from value=Template1 >
<input type=submit value='Create CH9.fake.1'>
</form>
```

你使用的 POST 变量为 `name` 表示新作业的名称，`from` 表示模板作业的名称。POST 操作的 URL 为 `/createItem?mode=copy`。

要更改主机名和端口号，你需要更新 Perl 脚本中找到的 `$host` 变量。

要删除作业，Perl 脚本生成了指向 `/job/Jobname/doDelete` 的操作表单（例如，`/job/CH9.fake.1/doDelete`）。不需要额外变量。

要增加表单条目数量，你可以将变量 `$end` 从 20 改为其他值。

### 更多内容...

Jenkins 使用标准库 Stapler (http://stapler.kohsuke.org/what-is.html) 将服务绑定到 URL。插件也使用 Stapler。当你安装插件时，可用操作的数量也会增加。这意味着你可以通过与本食谱类似的 HTML 表单激活大量操作。你将在第 7 章 *Exploring Plugins* 中了解到，为 Stapler 编写绑定代码所需的努力极小。

