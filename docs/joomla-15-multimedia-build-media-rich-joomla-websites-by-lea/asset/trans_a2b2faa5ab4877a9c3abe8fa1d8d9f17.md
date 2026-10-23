

# 文档中各元素

JSON 计数：4

```json
{
  "direction": "horizontal",
  "contents": [
    {
      "type": "paragraph",
      "markdown": "...",
      "bbox": [0.1, 0.1, 0.5, 0.2]
    }
  ]
}
```

在 Joomla! 文章中嵌入音频所需的最小有效代码如下所示：

```
<object width="350" height="50">
<param name="src" value="audio/sound.mp3">
<param name="autoplay" value="true">
<param name="controller" value="true">
<param name="bgcolor" value="#F4F4F4">
<embed src="audio/sound.mp3" autostart="true" loop="false" width="350" height="50" controller="true" bgcolor="#F4F4F4">
</embed>
</object>
```

让我们分解这段代码中的各个元素：

| object | 用于在网页中嵌入多媒体。 |
| height and width | 用于定义对象的高度和宽度。 |
| param | 控制对象的参数。 |
| src | 文件来源。 |
| autostart | 设置为 true 或 false，以控制音频是否自动播放。 |
| controller | 设置为 true 或 false，以显示或隐藏控制器。 |
| bgcolor | 用于设置播放器的背景颜色。 |
| embed | 使用 embed 标签将播放器放置到页面中。 |
| pluginspage | 将用户带到插件下载页面，在那里他们可以下载相应的播放器。 |

你会注意到 `<embed>` 标签中的内容与 `<object>` 标签中的部分内容相似，这是为了兼容所有浏览器。

前面的代码会将 MP3 文件嵌入到我们的 Joomla! 文章中，前提是音频文件名为 `sound.mp3`，并以 MP3 格式保存在名为 `audio` 的目录中，该目录位于 Joomla! 文件结构的根目录下（与 `images` 目录处于同一顶层）。

![](img/0151410972a5c5ea0aab26416e44b1da_143_0.png)

前面的代码会使用用户计算机上的默认媒体播放器来播放音频文件。这虽然可行，但无法灵活控制播放器的行为。因此，我们可以在代码中指定播放器，从而控制音频文件的播放器。

现在有许多基于 Flash 的 MP3 播放器；我特别喜欢的是 http://www.flash-mp3-player.net 上的这款，它是一个功能强大的免费 Flash MP3 播放器，并且具备一些很棒的功能。该网站甚至提供了一个简易代码生成器功能，可以让你轻松生成将播放器嵌入网页的正确代码。如果你使用 flash-mp3-player 作为音频播放器，HTML 代码大致如下：

```
<object type="application/x-shockwave-flash" data="http://flash-mp3-player.net/medias/player_mp3_maxi.swf" width="200" height="20">
<param name="movie" value="http://flash-mp3-player.net/medias/player_mp3_maxi.swf" />
<param name="bgcolor" value="#ffffff" />
<param name="FlashVars" value="mp3=audio/sound.mp3" />
</object>
```

> 诸如 Flash MP3 Player 之类的音频播放器提供了一种播放外部音频媒体的工具。要播放音频文件，需要正确设置音频文件的路径、文件名及其权限。

![](img/0151410972a5c5ea0aab26416e44b1da_144_0.png)

这段代码将以极少的代码知识和工作量，在 Joomla! 文章中加入一个精美的 Flash 音频播放器。如前所述，市面上有许多播放器和在线服务，各有各的功能。其中大多数都是基于 Flash 的播放器，因为近 98% 的网民电脑和浏览器都装有 Flash 播放器。

如果你希望进一步操作 HTML 音频代码，`<object>` 标签中还可以包含许多参数。遗憾的是，参数实在太多，本书无法一一介绍。如需更详细的参数列表，请访问 W3 Schools 网站：http://www.w3schools.com/TAGS/tag_object.asp。

## 使用 Joomla! 插件添加音频

我们已经完成了使用原始 HTML 代码向 Joomla! 文章添加音频文件的繁重工作，现在来看看 Joomla! 音频扩展的强大功能。

Joomla! 音频扩展为核心 CMS 提供了多种多样的扩展功能。安装并配置插件后，添加音频只需简单的“几次点击”即可完成。本章后面将重点介绍一些具体的 Joomla! 音频扩展，但下面的示例将向你展示它们对管理员来说有多么强大。

本示例所选的扩展是 JosDewplayer：

![](img/0151410972a5c5ea0aab26416e44b1da_145_0.png)

JosDewplayer 是一个适用于 Joomla! 的 GPL 许可 MP3 音频播放器。它采用基于 Flash 的播放器界面，可获取 MP3 文件并在 HTML 页面中流式播放。这些文件可以位于你的服务器上，也可以位于远程位置；最新版本支持多文件播放。

要获取适用于 Joomla! 的 JosDewplayer，请访问 http://extensions.joomla.org/ 并搜索“JosDewplayer”。

> 在向 Joomla! 网站安装任何第三方代码之前，请确保在安装扩展前备份你的网站数据库和文件集。

## 在 Joomla! 网站中使用音频

登录你网站的后台管理，然后点击顶部菜单中的**扩展 | 扩展管理器**：

![](img/0151410972a5c5ea0aab26416e44b1da_146_0.png)

使用**选择文件**上传按钮浏览本地计算机，找到 `josdewplayer_plugin.zip` 文件。选中后，使用**上传并安装**按钮将该扩展安装到你的 Joomla! 网站。

安装成功后，前往**扩展 | 插件管理器**配置新插件。此插件属于**内容**类型插件，因此你应在此类型下查找“JosDewplayer”。点击该文本即可打开插件编辑页面：

![](img/0151410972a5c5ea0aab26416e44b1da_146_1.png)

左侧的一组字段可让你根据需要重命名插件、启用或禁用插件，并为站点用户设置访问级别。在这些字段下方是**描述**字段，其中包含使用该插件的基本说明。右侧的一组参数可让你自定义**自动播放、背景颜色和自动重播**功能。**插件参数**通常只需设置一次；在本示例中，我们将发布插件，其他参数设置保持不变。

![](img/0151410972a5c5ea0aab26416e44b1da_146_2.png)

> 确保插件已启用，才能正常工作。

![](img/0151410972a5c5ea0aab26416e44b1da_147_0.png)

让我们回到之前创建的 Joomla! 文章，即包含原始 HTML 音频代码和 MP3 播放器代码的那篇。现在我们的扩展已经完成所有工作，我们只需在文章中输入一个插件能够识别的特殊标记——`{play}`——，然后输入音频文件的 URL 链接，最后再次关闭该特殊标记——`{/play}`。在 Joomla! 文章中输入以下代码：

```text
{play}/audio/sound.mp3{/play}
```

要创建指向该文件的绝对链接，请使用以下代码（将链接替换为音频文件的完整 URL）：

```text
{play}http://localhost/Joomla1.6/audio/sound.mp3{/play}
```

你可以根据音频文件所在的位置使用相对链接或绝对链接。如果链接到外部音频内容，绝对链接非常有用；这也是 JosDewplayer 的一个不错的功能。

在插件参数中全局设置的自动播放、背景颜色和自动重播功能，可以在每次使用标记时被覆盖。例如，要更改默认颜色，可以使用以下代码：

```text
{play}audio/sound.mp3|#000000 [BGCOLOR]{/play}
```

音频播放器现在将显示为：

![](img/0151410972a5c5ea0aab26416e44b1da_147_1.png)

可用参数列表位于插件参数的**描述**字段中。只要你的音频文件路径正确，几秒钟内就能将音频播放器放入 Joomla! 文章。现在，查看你网站的前端页面，即可看到音频通过 JosDewplayer 插件播放。只需插入此插件特有的简单标记，你就可以在网站中任意多次使用该播放器。使用 Joomla! 音频插件为网站页面添加音频，就是这么简单。

