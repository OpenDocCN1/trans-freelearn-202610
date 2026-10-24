

现在我们已经有了计算机生成的语音，你可能想要对其进行自定义。eSpeak 提供了完整的自定义功能，包括大量语言、语音和其他选项。要访问这些功能，你可以在命令行提示符下输入选项。例如，输入 `espeak -v +f3 hello`，你应该能够听到女声。你甚至可以通过输入 `espeak -v en-sc +f3 hello` 来添加苏格兰口音。

使用 eSpeak 时可供选择的语音有很多。这取决于你自己的偏好，但你可能喜欢带有英式口音的女声。随意尝试并选择你最喜欢的语音。然后，编辑默认文件将其设置为该语音。此默认文件位于 eSpeak 的主目录中。但是，不要指望能得到电影中电脑那种声音；那些是演员而不是电脑。然而，有一天，我们希望能够达到电脑听起来更像真人的阶段。

## 使用 pocketsphinx 来接受你的语音命令

现在你的机器人可以说话了，你也希望它能服从语音命令。本节将向你展示如何为你的机器人项目添加语音识别。这并不像说话部分那么简单，但幸运的是，你得到了开源开发社区的很大帮助。你将下载一套名为 **pocketsphinx** 的功能，它将允许我们的项目收听我们的命令。

第一步是下载 pocketsphinx 软件。不幸的是，这并没有 eSpeak 过程那么用户友好，所以请仔细按照以下步骤操作。有两种方法可以做到这一点。如果你连接了键盘、鼠标和显示器，或想通过 vncserver 连接，你可以通过图形方式执行以下步骤：

- 1. 前往由 **卡内基梅隆大学 (CMU)** 主办的 Sphinx 网站 `http://cmusphinx.sourceforge.net`。这是一个开源项目，可为你提供语音识别软件。在我们较小的嵌入式系统上，我们将使用此代码的 pocketsphinx 版本。

- 2. 你需要下载两个软件——**sphinxbase** 和 pocketsphinx。在页面顶部选择 **DOWNLOAD** 选项，然后找到这两个软件包的最新版本。下载软件包的 `.tar.gz` 版本并将它们移动到你的树莓派的 `/home/pi` 目录。

另一种方法是直接从树莓派的命令行提示符使用 **wget**。如果你想这样做，请执行以下步骤：

- 1. 要在你的主机上使用 wget，请找到你想要下载的文件的链接。在本例中，前往由 CMU 主办的 Sphinx 网站 `http://cmusphinx.sourceforge.net`。这是一个开源项目，可为你提供语音识别软件。在你的较小的嵌入式系统上，你将使用 pocketsphinx 版本的此代码。

- 2. 你需要下载两个软件，即 sphinxbase 和 pocketsphinx。在页面顶部选择 **DOWNLOAD** 选项，然后找到这两个软件包的最新版本。右键单击 `sphinxbase-0.8.tar.gz` 文件（只要 0.8 是最新版本）并选择 **Copy link location**。现在在你的树莓派中打开一个 PuTTY 窗口，登录后，输入 `wget` 并粘贴你刚刚复制的链接。这将下载 sphinxbase 的 `.tar.gz` 版本。现在，对 pocketsphinx 的最新版本重复相同的操作。

在构建这些软件之前，你需要另一个库。该库称为 **Bison**。这是一个通用开源解析器，被 pocketsphinx 使用。要获取此软件包，输入 `sudo apt-get install bison`。

一旦所有内容都下载完毕并且库已安装，你就可以解压并构建 pocketsphinx。要解包并构建 sphinxbase 模块，输入 sudo tar -xzvf sphinxbase-0.y.tar.gz，其中 y 是版本号，在本例中为 8。这应该会将归档中的所有文件解压到名为 sphinxbase-0.8 的目录中。现在，输入 cd sphinxbase-0.8。文件列表应如下所示：

![](img/324d126a7e2da696837a3e066326bd61_156_0.png)

要构建应用程序，首先发出 sudo ./configure --enable-fixed 命令。此命令检查系统是否一切正常，然后配置构建。

现在你准备好构建 sphinxbase 代码库了。这是一个两步过程，具体如下：

- 1. 输入 sudo make，系统将构建所有可执行文件。
- 2. 输入 sudo make install 将所有可执行文件安装到系统上。

现在，你需要制作系统的第二部分——pocketsphinx 代码。进入主目录并通过输入 `tar -xzvf pocketsphinx-0.8.tar.gz` 解压缩和解归档代码。现在，文件将被解归档，你可以构建代码。安装这些文件是一个三步过程，具体如下：

- 1. 输入 `cd pocketsphinx-0.8` 进入 pocketsphinx 目录，然后输入 `sudo ./configure` 检查你是否准备好构建文件。
- 2. 输入 `sudo make` 并等待所有内容构建完成。
- 3. 输入 `sudo make install`。

> 如果你打算使用 Python 作为编程语言使用 pocketsphinx 功能，我们库安装的其他添加项稍后会很有用。你可以使用 `sudo apt-get install python-dev` 安装 **Python-Dev**。同样，你可以使用 `sudo apt-get install cython` 获取 **Cython**。你也可以选择安装 `pkg-config`，这是一个在处理复杂编译时有时有帮助的实用程序。使用 `sudo apt-get install pkg-config` 安装它。

安装完成后，你需要让系统知道你的文件在哪里。为此，使用你喜欢的编辑器更改 `/etc/ld.so.conf` 文件，通过向文件中添加一行使其如下所示：

![](img/324d126a7e2da696837a3e066326bd61_157_0.png)

## 添加语音识别和语音 – 一个语音激活的机器人

输入 `sudo /sbin/ldconfig`，系统将意识到你的 pocketsphinx 库。现在一切都已安装，你可以尝试语音识别。重新启动系统并输入 `cd /home/pi/pocketsphinx-0.8/src/programs` 进入尝试演示程序的目录。然后，输入 `./pocketsphinx_continuous`。由于某种原因，我必须输入 `sudo ./pocketsphinx_continuous`，让它失败，然后再输入 `./pocketsphinx_continuous` 才能使其工作。此程序从麦克风获取输入并将其转换为语音。运行命令后，你会得到大量无关信息，然后你会看到以下截图：

![](img/324d126a7e2da696837a3e066326bd61_158_0.png)

INFO 和 Warning 语句来自 C 或 C++ 代码，用于调试目的。最初，它们会警告你找不到你的 Mic 和 Capture 元素，但当你的树莓派找到它们时，它会打印出 `READY.....`。如果你按照前面所述设置好一切，你就可以给你的树莓派一个命令了。对着麦克风说“hello”。当它检测到你停止说话时，它会处理你的语音，并在给出大量无关信息后，最终显示命令，如下图所示：

![](img/324d126a7e2da696837a3e066326bd61_159_0.png)

注意 `00000000: hello` 命令。它识别了你的语音！你也可以尝试其他单词和短语。系统非常敏感，因此可能会拾取背景噪音。你也会发现它并不十分准确。我们稍后会处理这个问题。要停止程序，请按 **Ctrl + C**。

使语音识别更准确有两种方法。一种是训练系统更准确地理解你的声音。这相当复杂，但如果你想了解更多，请访问本章前面提到的 CMU pocketsphinx 网站。

提高准确性的第二种方法是限制系统用于确定你在说什么的单词数量。默认情况下有数千个单词可能性，因此如果两个单词发音相似，pocketsphinx 可能会选择错误的单词。为了避免这种情况，你可以制作自己的词典来限制 pocketsphinx 可供选择的单词。要创建自己的词典，请按照 `http://cmusphinx.sourceforge.net/wiki/tutorialdict` 上的说明进行操作。

你的系统现在可以理解你的语音命令了！在本章下一节中，你将学习如何使用此输入来创建响应。

