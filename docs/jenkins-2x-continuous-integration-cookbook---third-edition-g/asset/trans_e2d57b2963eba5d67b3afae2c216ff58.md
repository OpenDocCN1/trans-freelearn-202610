

# 另请参阅

-   *使用 Dashboard View 插件节省屏幕空间*方法

# 使用 Dashboard View 插件节省屏幕空间

在 *高效使用视图* 方法中，你发现可以使用 `views` 插件节省水平标签页空间。在本方法中，你将使用 `Dashboard View` 插件来压缩水平空间的利用。压缩水平空间有助于高效地整合信息。

`Dashboard View` 插件允许你配置视图的各个区域以显示特定功能，例如，作业的网格视图，或显示失败作业子集的视图区域。用户可以拖放这些区域在屏幕上的位置。

开发人员已经让仪表板易于扩展，所以未来会有更多选择。

## 如何操作...

1. Dashboard View 插件将屏幕划分为多个区域。在仪表板配置期间，你选择作业网格和不稳定作业小工具。其他仪表板小工具包括作业列表、最新构建、从站统计、测试统计图表或网格，以及测试趋势图。随着插件成熟，将有更多选择。
2. 作为 Jenkins 管理员，登录你的 Jenkins 实例主页。
3. 通过单击屏幕顶部第二个选项卡中的加号创建新视图。
4. 选择 Dashboard 视图。
5. 单击 OK：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_575_0.png)

6. 在“作业筛选器”中选择“作业”：
7. 选择“仪表板小工具”：

### 仪表板小工具

- ☑ 在页面顶部显示标准 Jenkins 列表
- ☐ 全屏视图 - 隐藏标准 Jenkins 面板
- ☐ 设置 CSS 自定义参数

8. 添加或删除要在 Dashboard 视图中保留的列：

### 列

状态

删除

名称

删除

构建按钮

删除

上次成功

删除

添加列

### 仪表板小工具

- 在页面顶部显示标准 Jenkins 列表
- 全屏视图 - 隐藏标准 Jenkins 面板
- 设置 CSS 自定义参数

9. 单击“保存”并验证新的 Dashboard 视图：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_579_0.png)

10. 你可以将仪表板小工具添加到页面的不同部分：

### 仪表板小工具

- ☑ 在页面顶部显示标准 Jenkins 列表
- ☐ 全屏视图 - 隐藏标准 Jenkins 面板
- ☐ 设置 CSS 自定义参数

### 页面顶部的小工具

将仪表板小工具添加到视图顶部

- 每个项目的 Android Lint 问题
- Android Lint 问题趋势图（新增 vs. 已修复）
- Android Lint 问题趋势图（按优先级分布）
- 构建流水线仪表板视图
- 构建统计
- Iframe 小工具
- 图像
- Jenkins 作业列表
- 作业统计
- 作业网格
- 最新构建
- 从站统计
- 测试统计图表

11. 选择“构建统计”小工具并保存：

### 仪表板小工具

- ☑ 在页面顶部显示标准 Jenkins 列表
- ☐ 全屏视图 - 隐藏标准 Jenkins 面板
- ☐ 设置 CSS 自定义参数

### 页面顶部的小工具

![](img/2a4d2e09d9af2c678ea8f43ea344059d_580_0.png)

12. 在 Jenkins 仪表板中保存配置后，验证新的小工具：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_581_0.png)

与其他视图相比，作业网格小工具节省了空间，因为显示的作业密度很高。

如果你还使用了“多视图”选项卡（参见上一个方法），会有个小问题。当你单击仪表板标签时，显示的是原始视图集合，而不是选择框。

# 更多内容...

Dashboard 插件为其他插件开发者提供了一个创建仪表板视图的框架。这种用法的一个示例是 `Project Statistics` 插件（https://wiki.jenkins-ci.org/display/JENKINS/Project+Statistics+Plugin）。

# 另请参阅

- *创建 HTML 报告*方法
- *高效使用视图*方法

# 使用 HTML5 浏览器发出声音

本方法介绍如何在事件发生时（例如构建成功）向 Jenkins 用户的浏览器发送自定义声音。你也可以在任意时间发送声音消息。这不仅适合那些喜欢被喊叫、被著名演员唱歌等开发者，也适合在大型服务器场中寻找计算机的系统管理员。

## 准备工作

安装 Jenkins 声音插件（https://wiki.jenkins-ci.org/display/JENKINS/Jenkins+Sounds+plugin）。确保你安装了符合 HTML5 规范的浏览器，例如当前版本的 Firefox 或 Chrome。

有关浏览器中 HTML5 合规性的更多详细信息，请参考：http://en.wikipedia.org/wiki/Comparison_of_layout_engines_%28HTML5%29。

## 如何操作...

1. 以 Jenkins 管理员身份登录，并访问“配置系统”屏幕 `/configure`。
2. 在“Jenkins 声音”部分，勾选“通过支持 HTML5 音频的浏览器播放”。

如果 Jenkins 在查找声音归档时出现问题，并出现诸如 `File not found 'file:/C:/Users/Alan/.jenkins/jar:file:/C:/Users/Alan/.jenkins/plugins/sounds/WEB-INF/lib/classes.jar/sound-archive.zip'` 的错误消息，则解压 `classes.jar` 文件，并将 `sounds-archive.zip` 文件移动到错误消息中提到的相同目录。最后，将配置指向该归档，例如 `file:/C:/Users/Alan/.jenkins/plugins/sounds/WEB-INF/lib/sound-archive.zip`。

3. 单击“保存”按钮。
4. 选择 Jenkins 主页上的“创建作业”链接。
5. 创建一个新作业，作业名称为 `ch4.sound`。
6. 选择“构建一个自由风格的软件项目”。
7. 单击 OK。
8. 在“构建后操作”部分，勾选“Jenkins 声音”选项。
9. 添加两个声音：EXPLODE 和 doh。
10. 单击“保存”。
11. 单击“立即构建”链接。
12. 成功后，你的浏览器将播放 `EXPLODE` wav 文件。
13. 编辑你的作业使其失败，例如，通过添加一个不存在的源代码仓库。
14. 再次构建该作业。失败时，你的 Web 浏览器将播放 `doh` wav 文件。

## 工作原理...

你已成功配置作业，使其根据构建的成功或失败播放不同的声音。

你可以通过配置哪些事件转换将触发声音来进一步调整插件的响应方式，例如，如果上一次构建结果为失败而当前构建结果为成功。这可以在“对于上一次构建结果”复选框组中定义。

该插件作为一个页面装饰器工作。它添加了以下 JavaScript，用于异步轮询新声音。你的浏览器承担了大部分工作，从而释放了服务器资源：

```html
<script src="/sounds/script"
     type="text/javascript"></script><script type="text/javascript"
     defer="defer">function _sounds_ajaxJsonFetcherFactory(onSuccess,
     onFailure) {
return function() {
new Ajax.Request("/sounds/getSounds", {
parameters: { version: VERSION },
onSuccess: function(rsp) {
onSuccess(eval('x='+rsp.responseText))
},
onFailure: onFailure
});
}
}
if (AUDIO_CAPABLE) {
    _sounds_pollForSounds(_sounds_ajaxJsonFetcherFactory);
}</script>
```

# 更多内容...

声音插件还允许你将任意声音流式传输到已连接的 Web 浏览器。这不仅对针对分布式团队的恶作剧和激励演讲有用，你还可以执行有用的操作，例如在重启服务器前发出 10 分钟警告警报。

你可以在 http://www.archive.org/details/opensource_audio 找到一些不错的声音集。

例如，你可以在 http://www.archive.org/details/OpenPathMusic44V2 找到“每个孩子一台笔记本电脑”音乐库的副本。在该集合中，你会发现 `shenai.wav`。首先，将声音添加到互联网上可以找到的地方。一个合适的位置是 Jenkins 的 `userContent` 目录。要在任何已连接的 Web 浏览器上播放该声音，你需要访问触发地址（将 `localhost:8080` 替换为你自己的地址）：

```
http://localhost:8080/sounds/playSound?src=http://localhost:8080/userContent/shenai.wav
```

