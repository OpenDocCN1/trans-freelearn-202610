
网页服务文档并不完善，但非常简单，且文档以示例为主。在 API 主页上，每个 feed 的格式链接都是使用该 feed 的示例。涉及成员信息的 feed 使用用户 “RJ”，他是 Last.fm 的创始人之一。涉及艺术家信息的 feed，如热门听众，则使用多种艺术家。要使用任何 feed，只需取下你想要的播放列表和格式的 URL，并将其替换为用户或艺术家。

让我们试试一些示例。feed 易于使用和实验，因为 Audioscrobbler 的 feed 只是 URL，使用它们不需要开发者 ID。API 主页显示用户播放的热门曲目可通过纯文本、Last.fm 自有的 XML 格式以及 XSPF 播放列表获得。该页面上 RJ 的 XSPF 格式热门曲目的 URL 为 http://ws.audioscrobbler.com/1.0/user/RJ/toptracks.xspf。

如果我想查看我的热门曲目，只需将 RJ 替换为我自己的 Last.fm 用户名即可：http://ws.audioscrobbler.com/1.0/user/ShuTheMoody/toptracks.xspf。

艺术家 feed 使用 Metallica 作为默认示例。要查看 Metallica 的 Last.fm 顶级粉丝列表，请使用 API 主页上的 Top Fans feed：http://ws.audioscrobbler.com/1.0/artist/Metallica/fans.xml。要查看另一位艺术家的顶级粉丝，只需更改 URL 中的 Metallica 部分：http://ws.audioscrobbler.com/1.0/artist/Donnas/fans.xml。

## 使用 PEAR 进行解析

如果我们现在就开始 mashup，在 XSPF、YouTube 的 XML 响应和 RSS 之间，我们必须创建三个不同的解析器来处理全部三种响应格式。我们必须查阅文档并为全部三种格式创建灵活的解析器。如果其中任何一种格式的 XML 响应发生变化，我们也必须负责更改我们的解析器代码。这不是一个困难的任务，但我们应该意识到已经有人为我们完成了这项工作。有人已经拆解了 XML 代码。为了节省时间，我们可以利用这项工作用于我们的 mashup。

我们在第 1 章中早些时候使用过 PEAR 来帮助 XML-RPC 解析。对于本项目，我们将再次使用 PEAR，以免我们为将遇到的三种 XML 格式编写解析器而费心。

对于本项目，我们将查看用于 mashup 的三个包。File_XSPF 是一个用于提取和设置 XSPF 播放列表的包。Services_YouTube 是一个专门为我们处理 YouTube API 而创建的 Web Services 包。最后，XML_RSS 是一个用于处理 RSS feed 的包。

对于本项目，很好的是有三个特定包适合我们的 XML 和 RSS 格式。如果你需要处理没有特定 PEAR 包的 XML 格式，可以使用 XML_Unserializer 包。该包会接收一个 XML 并将其作为字符串返回。

## PEAR 是否适合你？

![](img/c637937d44400f1da21fb7cca101de80_159_0.png)

在开始安装 PEAR 包之前，我们应该看看是否可行将它们用于项目。PEAR 包使用命令行包管理器安装，该管理器包含在每个 PHP 核心安装中。要安装 PEAR 包，你需要对服务器具有管理访问权限。如果你处于共享托管环境中且你的托管公司很吝啬，或者如果你处于严格的企业环境中，获取服务器更改的麻烦大于其价值，PEAR 安装可能不被允许。你可以通过下载 PEAR 文件并将它们安装在你的 Web 文档目录中来解决此问题。然而，你随后必须自行管理包依赖关系和包更新。这种麻烦可能弊大于利，你可能最好编写自己的代码来处理该功能。

另一方面，PEAR 包通常能节省大量时间。包的目的要么是简化繁琐的任务，要么是与复杂系统交互。PEAR 开发者已经为你完成了困难的工作。此外，由于它们是用 PHP 而不是像 PHP 扩展那样的 C 编写的，如果文档不足，有能力的 PHP 开发者应该能够阅读代码作为文档。最后，许多包，包括我们将要查看的包的一个关键好处是，它们是对它们所交互内容的面向对象表示。值可以通过简单地调用对象的属性来提取，复杂连接可以通过简单的函数调用来点燃。这有助于保持我们的代码更干净和模块化。PEAR 的好处是否超过潜在障碍取决于你的具体情况。

## 包安装和使用

就像我们安装 XML-RPC 包时一样，我们将使用 install 二进制文件来安装我们的三个包。如果你还记得，安装一个包，只需在命令行中输入 install 后跟包名即可。在这种情况下，我们需要设置一些额外的标志以强制安装程序获取依赖项和处于 beta 状态的代码。

要安装 File_XSPF，请切换到机器的 root 用户并使用此命令：

```
[Blossom:~] shuchow# /usr/local/php5/bin/pear install -f --alldeps File_XSPF
```

此命令将下载包。-alldeps 标志告诉 PEAR 还要检查所需的依赖项并在必要时安装它们。下载的进度和结果将被报告。

对 Services_YouTube 执行类似的命令：

```
[Blossom:~] shuchow# /usr/local/php5/bin/pear install -f --alldeps Services_YouTube
```

通常，你不需要 -f 标志。默认情况下，PEAR 下载包的最新稳定版本。-f 标志，force，强制 PEAR 下载最新版本，无论其发布状态如何。截至本文撰写时，File_XSPF 和 Services_YouTube 没有稳定版本，只有 beta 和 alpha 分别。因此，我们必须使用 -f 来获取并安装此包。否则，PEAR 会抱怨最新版本不可用。如果你要下载的包处于发布状态，你将不需要 -f 标志。

XML_RSS 就是这种情况，它有可用的稳定版本。

```
[Blossom:~] shuchow# /usr/local/php5/bin/pear install --alldeps XML_RSS
```

之后，向 PEAR 发送 list-all 命令将显示三个新包以及你之前拥有的包。

PEAR 包基本上是自包含的 PHP 文件，PEAR 将其安装到你的 PHP includes 目录中。includes 目录是 php.ini 文件中的一个指令。导航到此目录以查看 PEAR 包的源文件。要使用 PEAR 包，你需要在代码顶部包含包的源文件。请参阅包的文档，了解如何包含主包文件。例如，File_XSPF 通过包含一个名为 XSPF.php 的文件来激活。PEAR 将 XSPF.php 放在名为 File 的目录中，而该目录位于你的 includes 目录内。

```
<?php
    require_once 'File/XSPF.php';
    //File_XSPF is now available.
```

## File_XSPF

XSPF 最新版本的文档位于 http://pear.php.net/package/File_XSPF/docs/latest/File_XSPF/File_XSPF.html。

该包简单易用。该包的核心是一个名为 XSPF 的对象。你实例化并使用此对象与播放列表交互。它具有检索和修改播放列表中的值的方法，以及将播放列表加载到内存中、将播放列表从内存写入文件以及将 XSPF 文件转换为其他格式的实用方法。

从播放列表获取信息包括两个简单的步骤。首先，XSPF 文件的位置传递给 XSPF 对象的 parse 方法。这会将文件加载到内存中。文件加载后，你可以使用对象的各种 getter 方法从列表中提取值。大多数 XSPF getter 方法与获取播放列表本身的元数据有关。要获取播放列表中曲目的信息，请使用 getTracks 方法。此方法将返回一个 XSPF_Track 对象数组。播放列表中的每个曲目都作为此数组中的一个 XSPF_Track 对象表示。然后你可以使用 XSPF_Track 对象的 方法来获取有关各个曲目的信息。

我们可以从 Last.fm 获取一个播放列表来说明其工作原理。Web 服务有一个成员最常播放歌曲的播放列表。名为 Top Tracks 的播放列表位于 http://ws.audioscrobbler.com/1.0/user/USERNAME/toptracks.xspf，其中 USERNAME 是你想要查询的 Last.fm 用户名。

示例中此页名为 XSPFPEARTest.php。它使用 File_XSPF 来显示我来自 Last.fm 的热门曲目播放列表。

```
<?php
    require_once 'File/XSPF.php';
    $xspfObj =& new File_XSPF();
    //Load the playlist into the XSPF object.
    $xspfObj->parseFile('http://ws.audioscrobbler.com/1.0/user/ShuTheMoody/toptracks.xspf');
    //Get all tracks in the playlist.
    $tracks = $xspfObj->getTracks();
?>
```

这一节首先创建 XSPF 对象并加载播放列表。首先，我们将 File_XSPF 包引入脚本。然后，我们实例化对象。parseFile 方法用于跨网络加载 XSPF 文件列表。这将播放列表绑定到 XSPF 对象。然后我们使用 getTracks 方法将播放列表上的歌曲转换为 XSPF_Track 对象。

```
<html>
<head>
<title>Shu Chow's Last.fm Top Tracks</title>
</head>
<body>
    Title: <?= $xspfObj->getTitle() ?><br />
    Created By: <?= $xspfObj->getCreator() ?>
```
