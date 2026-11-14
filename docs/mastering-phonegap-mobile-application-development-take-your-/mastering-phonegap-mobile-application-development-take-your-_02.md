## 任务自动化

在开发应用时，通常有许多任务需要定期执行。虽然这些任务很少困难或极其耗时，但随着时间的推移，精力累积起来，很快就会变得枯燥且容易出错。

任务自动化简化了这些繁琐的例行工作。自动化让您定义频繁执行的任务步骤（甚至那些 *不常执行* 的任务，这可能甚至更有用）。在某种程度上，您可以将任务自动化视为类似于您可能使用的其他生产力应用程序（如 Microsoft Word）中的宏。

单个任务也可以依赖于其他任务，因此您可以将手动流程简化为一两个易于记忆和输入的命令。此外，大多数任务自动化工具都提供了一种机制来监视项目的变更，在检测到任何变更时自动执行各种任务。

从技术上讲，开发 PhoneGap/Cordova 应用并不需要任务自动化；但随着应用变得更大、更复杂，它变得越来越有益。当然，这有一些初始开销，这通常是小项目从未实施任务自动化的原因。但当一个应用拥有多个视图、大量模块和相当数量的依赖项时，初始开销很快就会得到回报。

### 注意

尽管本书标题为 *Mastering PhoneGap Mobile Application Development*（精通 PhoneGap 移动应用开发），但我们将使用 *Cordova* 来指代 PhoneGap 和 Cordova。PhoneGap 源自 Cordova，我们使用 Cordova 所做的一切也适用于 PhoneGap。如果不适用，我们会明确提及。

此外，当我们提到 Cordova 和 PhoneGap 时，指的是命令行工具。有一个可用的 PhoneGap Build 服务可在云端执行编译和打包；但如果您想使用它，需要适当调整本书的内容。如果想了解更多，请参阅本书代码包中的 `README.md` 文件。




# 使用 Gulp 进行任务自动化

## 本章内容

*   Logology，演示应用
*   为何使用 Gulp 进行任务自动化
*   设置应用的目录结构
*   安装 Gulp
*   创建首个 Gulp 配置文件
*   创建模块化 Gulp 配置
*   复制资源文件
*   执行替换操作
*   执行各种 Cordova 任务
*   管理版本号
*   支持 ES2015
*   代码检查
*   压缩/混淆代码

## 先决条件

本书附带一个代码包，可在 https://github.com/kerrishotts/Mastering-PhoneGap-Code-Package 获取。如果您尚未下载，我强烈建议您这样做。它包含每一章的所有代码，以及大量演示大多数章节中示例的代码片段。此外，本书各章主要侧重于代码片段——若要在实际应用中查看这些主题的应用，您一定想看看演示应用的代码。

在继续本章之前，请确保您满足本书前言中列出的先决条件。软件和硬件要求也列在本书代码包的 `README.md` 文件中。

## 构建演示应用

如果您想从代码包构建并部署演示应用程序，则需要安装前面提到的工具。由于 Cordova 项目和特定平台文件被视为构建产物，您需要在每一章的目录中执行以下命令，以构建应用的每个版本：

```
## 在 Linux / Mac OS X 上（使用 Bash shell）
$ npm install && gulp init

% 在 Windows 上
> npm install
> gulp init
```

## 演示应用：Logology

在进一步深入之前，让我们描述一下将在本书过程中构建的演示应用。

我称之为 **Logology**。如果您熟悉任何希腊单词，可能已经猜到该应用是什么了：一本词典。现在，我理解这不一定是最惊人的应用，但它足以满足我们的目的。它将帮助您了解高级移动开发是如何进行的。

## 功能

到本书结束时，该应用将具备以下功能：

*   **搜索**：用户将能够搜索术语
*   **响应式设计**：应用将适当调整大小以适应任何显示尺寸
*   **无障碍访问**：即使用户有视觉障碍，也能使用该应用
*   **持久化存储**：应用将持久化设置和其他用户生成的信息

## 为何要任务自动化？

尽管该应用听起来相对简单，但它足够复杂，可以从任务自动化中受益。既然从一开始就建立任务自动化很有用，在本章中我们将安装 Gulp，并首先通过一些简单的文件验证其是否正常工作。因此，第一章代码包中的应用非常简单；它仅用于验证我们的任务是否正常工作。

您可能会认为配置任务自动化非常耗时，但从长远来看这是值得的。一旦您拥有了喜欢的工作流，就可以将其应用到未来可能构建的任何其他应用中。这意味着未来的应用几乎可以立即启动（只需从之前的应用复制配置）。即使您不编写其他应用，拥有任务运行器节省的时间也将超过初始设置时间。

## 介绍 Gulp

Gulp (http://gulpjs.com) 是一个基于 `Node.js` 平台的任务自动化工具。与其他一些任务运行器不同，Gulp 通过编写 JavaScript 代码进行配置。Gulp 的配置就像任何其他 JavaScript 文件一样，这意味着如果您懂 JavaScript，就可以快速开始定义自动化任务。

Gulp 还使用了 `Node.js` 中的 `streams`（流）概念。虽然您可以将流视为文件，但流实际上更强大。插件可以插入到流处理中，以执行许多不同的转换，包括美化或丑化、转译（例如，ECMAScript 6 转 ECMAScript 2015）、合并、打包等。

### 提示

如果您在命令行上执行过任何类型的管道操作，Gulp 应该会让您感到熟悉，因为它基于类似的概念运行。一个进程的输出被传递给下一个进程，后者执行任意数量的转换，依此类推，直到最终输出被写入另一个位置。

## Gulp 的并行执行

Gulp 还尝试并行运行尽可能多的依赖任务。理想情况下，这使得 Gulp 任务运行得更快，尽管这实际上取决于您的任务结构。其他任务运行器（如 Grunt）按顺序执行任务步骤，这可能导致输出较慢，尽管按顺序执行时，从输入到输出的步骤可能更容易跟踪。

这并不是说 Gulp 是最好的任务运行器——有很多都很不错，您可能会发现更喜欢其中的一个。您在本书中学到的技能可以轻松迁移到其他任务自动化工具中。

## 其他任务运行器

以下是一些其他有用的任务运行器：

*   **Grunt** (http://www.gruntjs.com)：配置通过设置指定，而非代码。任务按顺序执行。
*   **Cake** (http://coffeescript.org/documentation/docs/cake.html)：它使用 CoffeeScript，配置通过代码指定，正如在 Gulp 中所见。如果您喜欢使用 CoffeeScript，可能会比 Gulp 更喜欢它。
*   **Broccoli** (https://github.com/broccolijs/broccoli)：它也通过代码进行配置。

## 设置目录结构

在安装 Gulp 之前，我们应该为应用创建目录结构。请记住，没有单一的正确方式来构建应用程序结构，随着经验的积累，您对应用结构的看法可能会改变。话虽如此，本节将向您展示我喜欢如何构建我的项目。

我典型的结构从项目的根目录开始。如果您查看本书的代码包，会注意到项目的根目录称为 `logology-v01/`。

### 注意

我通常不会在项目上附加版本号——那是版本控制系统的工作。然而，既然重要的是您能看到版本之间的变化，代码包按章节分割这些变化——因此有了版本号。

## 项目目录

在项目的根目录中有一些额外的目录：

*   `config/`：任务期间所需的配置文件存储在此目录中。
*   `src/`：所有应用的源代码和图像资源存储在此目录中。这是我们提供给 Gulp 的*源代码*。Gulp 随后转换源代码并将其存储在我们选择的目录中（通常是 `build` 目录）。
*   `build/`：此目录包含转换后的 HTML、CSS 和 JavaScript 代码，以及 Cordova 项目的原生部分。

### 注意

`build/` 目录不会出现在本书的代码包中。它被视为构建产物，因此您始终可以重新生成它。

## 源代码目录结构

在 `src/` 目录中存放着我们应用的源代码。我喜欢按如下方式构建代码和资源：




```
project-root/
  src/
    config.xml            # 我们 Cordova 应用配置的
                          # 模板文件
    res/                  # 图标和启动画面（第 11 章介绍）
    www/                  # HTML、JavaScript 和 CSS 以及其他
                          # Web 资源
      index.html          # 初始 HTML 文件（在 config.xml 中指定）
      html/               # 其他 HTML 文件（如有）
      img/                # 图片文件（如需要）
      scss/               # Sassy CSS 文件（见第 3 章）
        lib/              # 实用函数
        themes/           # 主题（应用的外观）
        views/            # 我们应用中特定视图的样式
      js/                 # JavaScript
        lib/              # 第三方库代码和支持
                          # 代码
        app/              # 我们的应用代码
          index.js        # 我们应用的入口点
          controllers/    # 视图控制器放在这里
          lib/            # 特定于应用的实用文件
          localization/   # 语言翻译
          models/         # 数据模型放在这里
          views/          # 视图和模板放在这里
```

如果你查看代码包中本章的目录结构，你会注意到其中缺少了很多内容。这是因为目前还不需要这些内容；我们将在后续章节中完善它们。

如果你在想 Cordova 文件在哪里，说明你很细心。这里并没有。*暂时没有*。这是因为它们被视为 **构建产物**。构建产物是指编译应用时创建的文件。如果这让你感觉既有点陌生又有点熟悉，那是有充分理由的：Cordova 项目本身已经有部分被视为构建产物。陌生的部分在于，你可能习惯于编辑 Cordova 项目中的 `www/` 文件夹，并执行 `cordova build` 来创建其余的构建产物（即原生包装器，通常在 `platforms/` 中）。

然而在本书中，我们将采用更高层级的方法，将 *整个 Cordova 项目* 视为构建产物。为什么？因为 Cordova 已被其他几个项目扩展（例如 Steroids：[`www.appgyver.com/steroids`](http://www.appgyver.com/steroids)），它们通常有自己的项目格式和构建步骤。如果你想以这些平台为目标，你可以轻松做到，因为你的代码不存在于 Cordova 项目中。此外，你可能会发现想要以完全不同的技术为目标，例如 Electron ([`electron.atom.io`](http://electron.atom.io))，它使用适合桌面执行的 Chromium webview 封装你的代码。Electron 的构建步骤和项目结构与你对 Cordova 项目的预期不同。简而言之，这是一种避免束缚自己的方式。

总而言之，当我们完成本章时，你将拥有一个填充了你源代码的 Cordova 应用。该项目将位于 `build/` 目录中。

### Tip

如果你需要在 Gulp 之外执行 Cordova 命令，你首先需要切换到 `build/` 目录，否则命令将会失败。这是因为 Cordova CLI 期望在 Cordova 项目内运行，而我们应用的根目录不是一个 Cordova 项目。只有 `build/` 包含一个有效的 Cordova 项目。

我们工作流程中至关重要的一部分是项目的 `package.json` 文件。该文件将包含应用的版本信息、Cordova 配置等。如果你是从头开始，你需要通过切换到项目根目录并执行 `npm init` 来自行创建此文件：

### Note

如果你使用的是本章的代码包，`package.json` 文件已经为你构建好了。

```
### （在你的项目根目录中）
$ npm init [ENTER]
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sane
defaults.
…

name: (logology-v01) Logology [ENTER]
version: (1.0.0) [ENTER]
description: Logology and PhoneGap demonstration app [ENTER]
entry point: (index.js) [ENTER]
test command: [ENTER]
git repository: [ENTER]
keywords: dictionary word study phonegapcordova html5 javascript
css [ENTER]
author: Kerri Shotts<kerrishotts@gmail.com> [ENTER]
license: (ISC) MIT [ENTER]
About to write to .../logology-v01/package.json:

Is this ok? (yes) [ENTER]
```

此时，`package.json` 文件已创建，但还需要一些额外的编辑。在你喜欢的编辑器中打开 `package.json` 文件并移除 `scripts` 部分。然后，添加以下内容（有关该文件的完整内容，可参考代码包）：

```json
{ ...,
"cordova": {
  "name": "Logology",
  "id": "com.packtpub.logologyv1",
  "description": "Dictionary application",
  "author": {
    "name": "Kerri Shotts",
    "email": "kerrishotts@gmail.com",
    "site": "http://www.photokandy.com"
  },
  "template": "../blank",
  "platforms": [ "ios", "android" ],
  "preferences": {
    "permissions": "none",
    "fullscreen": "false",
    "orientation": "default",
    "stay-in-webview": "false",
    "ShowSplashScreenSpinner": "false",
    "AutoHideSplashScreen": "false",
    "disable-cursor": "false",
    "KeyboardDisplayRequiresUserAction": "false",
    "target-device": "universal",
    "prerendered-icon": "true",
    "webviewbounce": "false",
    "DisallowOverscroll": "true",
    "exit-on-suspend": "false",
    "deployment-target": "7.0",
    "detect-data-types": "false",
    "SupressesIncrementalRendering": "true",
    "android-minSdkVersion": "14",
    "android-installLocation": "auto",
    "android-windowSoftInputMode": "adjustResize",
  },
  "plugins": [
    "cordova-plugin-device@1.1.0",
    "cordova-plugin-network-information@1.1.0",
    "cordova-plugin-globalization@1.0.2",
    "cordova-plugin-whitelist@1.1.0",
    "ionic-plugin-keyboard@1.0.8",
    "cordova-plugin-inappbrowser@1.0.1"
    ]
  }
}
```

前面的代码应该很容易理解。通过它，我们本质上是在复制 Cordova 的 `config.xml` 文件的内容。因为 Cordova 项目本身被视为构建产物，所以在别处管理插件、平台和首选项是合理的，而且因为 `package.json` 处理我们项目的其他配置方面，所以将这些配置设置包含在此处也是合理的。

### Note

这并不意味着不需要 `config.xml` 文件。我们稍后在本章中将介绍这一点。

此时，我们准备好安装 Gulp 以及我们项目可能需要的任何其他依赖。

安装 Gulp 很简单，但实际上是一个两步过程。第一步是全局安装 Gulp。这会安装命令行工具；但如果不在我们的项目中 *本地* 安装 Gulp，它将无法工作。如果你不熟悉 Node.js，包可以本地和/或全局安装。本地安装的包位于项目根目录，而全局安装的包特定于开发者的机器。项目依赖在 `package.json` 中跟踪，这使得在另一台机器上复制开发环境变得容易。

假设你已安装 Node.js 并在项目目录中创建了 `package.json`，Gulp 的安装将非常容易。确保你位于项目根目录，然后执行以下命令：

```
$ npm install -g gulp@3.9.0
$ npm install --save-dev gulp@3.9.0
```

### Note

如果你在 OS X 上运行这些命令时收到错误，可能需要使用 `sudo` 运行它们。例如：`sudo install -g gulp`。

通常可以忽略任何 `WARN` 消息。



请注意，我们在这里指定了版本号——这些是我在编写本书代码时使用的版本。如果你愿意，可以尝试更新的版本，只要它们是次要修订版即可。主要修订版可能也能工作，但你可能也需要对本书中的代码进行修改以支持它们。

### 提示

每当你执行 `npm` 或 `gulp` 命令时，最好都定位在项目的根目录下。在 Linux 和 OS X 上，这些命令通常会自动定位项目的根目录；但这在所有平台上都无法保证，所以小心驶得万年船。

就是这样！Gulp 本身非常容易安装，但大多数工作流需要与 Gulp 配合工作的额外插件。此外，我们还将为该项目安装 Cordova 依赖。

### 注意

如果你正在使用本章的代码包，可以通过执行 `npm install` 来安装以下所有依赖。

首先，让我们安装 Cordova 依赖：

```
$ npm install --save-dev cordova-lib@5.4.1 cordova-ios@3.9.2 cordova-android@4.1.1
```

`cordova-lib` 允许我们以编程方式与 Cordova 交互。我们可以创建项目、构建它们并模拟它们——我们在 Cordova 命令行中所做的一切都可以用 `cordova-lib` 完成。`cordova-ios` 和 `cordova-android` 指的是 `cordova platform add ios android` 会添加的 iOS 和 Android 平台。我们已将它们作为项目的依赖，以便我们可以轻松控制将使用哪个版本进行构建。

### 提示

在开始一个新项目时，明智的做法是从最新版本的 Cordova 和所需的平台开始。一旦开始，通常最好坚持使用特定的平台版本，除非存在严重的错误或安全问题，需要更新到更新的平台版本。

接下来，让我们安装所需的 Gulp 插件：

```
$ npm install --save-dev babel-eslint@4.1.1 cordova-tasks@0.2.0 gulp-babel@5.2.1 gulp-bump@0.3.1 gulp-concat@2.6.0 gulp-eslint@1.0.0 gulp-jscs@3.0.2 gulp-notify@2.2.0 gulp-rename@1.2.0 gulp-replace-task@0.10.1 gulp-sourcemaps@1.3.0 gulp-uglify@1.4.0 gulp-util@3.0.6 merge-stream@1.0.0 require-all@1.1.0 rimraf@2.4.3
```

这些安装需要片刻时间；完成后，请查看 `package.json`。请注意，我们添加的所有依赖也被添加到了文件的 `devDependencies` 部分。这使得稍后（比如在一台新机器上）只需执行 `npm install` 即可轻松安装项目的所有依赖。

在继续之前，让我们快速浏览一下前面提到的每个实用程序的作用。随着本章剩余部分的推进，我们将更详细地介绍它们：

*   `gulp-babel`：将 ES2015 JavaScript 转换为 ES5。如果你不熟悉 ES2015，它有几个新特性和改进的语法，使编写移动应用更容易。不幸的是，因为大多数浏览器还不原生支持 ES2015 的特性和语法，所以必须将其转译为 ES5 语法。当然，如果你更喜欢其他可以编译为 ES5 JavaScript 的语言，你也可以使用它们（包括 CoffeeScript 等）。
*   `gulp-bump`：这个小工具管理 `package.json` 中的版本号。
*   `gulp-concat`：这将流连接在一起。我们可以用它将文件打包在一起。
*   `gulp-jscs`：这对你的代码执行 JavaScript 代码风格检查。它支持 ES2015。
*   `gulp-eslint`：这会检查你的 JavaScript 代码。它支持 ES2015。
*   `babel-eslint`：这为 `gulp-eslint` 提供 ES2015 支持。
*   `gulp-notify`：这是一个可选插件，但非常方便，特别是当你的某些任务需要运行几秒钟时。当发生重要事件时——可能是错误或完成消息——此插件会向计算机的通知面板发送通知。如果插件无法发送到通知面板，它将记录到控制台。
*   `gulp-rename`：这会重命名流。
*   `gulp-replace-task`：这在流中执行文本搜索和替换。
*   `gulp-sourcemaps`：在将 ES2015 转译为 ES5 时，拥有原始源代码和转译后源代码之间的映射会很有帮助。此插件会自动为你创建它们。
*   `gulp-uglify`：这会压缩/混淆你的代码。虽然对于代码混淆很有用，但它也能减小代码体积。
*   `gulp-util`：这为 Gulp 提供额外的实用程序，例如日志记录。
*   `merge-stream`：这合并多个任务。
*   `require-all`：这让我们可以一次性将整个代码目录导入到一个对象中。
*   `rimraf`：轻松删除文件。类似于命令行上的 `rm`。

Gulp 任务由项目 `gulpfile.js` 文件的内容定义。这是一个 JavaScript 程序，因此你拥有的 JavaScript 技能同样适用于此处。此外，它由 Node.js 执行，所以如果你有任何 Node.js 知识，你可以利用它。

### 提示

此文件应放置在项目的根目录中，且必须命名为 `gulpfile.js`。

Gulp 配置文件的前几行将 `require` 完成任务所需的 Gulp 插件。以下几行将指定如何执行各种任务。例如，一个非常简单的配置可能如下所示：

```
var gulp = require("gulp");
gulp.task("copy-files", function () {
  gulp.src(["./src/**/*"])
      .pipe(gulp.dest("./build"));
});
```

此配置仅执行一项任务：它将 `src/` 中包含的所有文件移动到 `build/`。在许多方面，这是构建工作流最简单的形式，但对于我们的目的来说，它有点过于简单了。

### 提示

请注意我们用于匹配所有文件的模式。如果你需要查看支持哪些模式的文档，请参阅 [`www.npmjs.com/package/glob`](https://www.npmjs.com/package/glob)。

要执行该任务，可以执行 `gulp copy-files`。Gulp 将执行该任务并将所有文件从 `src/` 复制到 `build/`。

Gulp 之所以强大，在于任务组合的概念。任务可以依赖任意数量的其他任务，而那些任务又可以依赖更多的任务。这使得从更简单的部分创建复杂的工作流变得容易。此外，每个任务都是异步的，因此许多没有共享依赖的任务可以并行运行。

正如你在前面的代码中所见，每个任务由一系列源文件的选择（`src()`）、可选地对每个文件执行一些额外处理（通过 `pipe()`），然后将这些文件写入目标路径（`dest()`）组成。如果未指定额外处理（如前面的示例），Gulp 将简单地复制匹配通配符模式的文件。然而，流的美妙之处在于，可以在最终数据保存之前执行任意数量的转换。因此，工作流可以变得非常复杂。

现在你已经看到了一个简单的任务，让我们在下一节深入了解一些更复杂的任务。

虽然你可以将所有想要运行的任务添加到单个配置文件中，但随着你向环境中添加更多任务，这很快就会变得难以驾驭。为了保持配置维护简单，最好将所有内容拆分到单独的文件中。

这意味着除了项目的目录结构外，我们的 Gulp 配置也有自己的结构。以下展示了我喜欢如何构建我的配置：




## 项目结构概览

```
project-root/
  gulpfile.js            # Stub (loads in everything else)
  gulp/
    config.js            # Configuration – files to copy,
                         # output paths, etc.
    settings.js          # values of command-line flags
    tasks.js             # Stub (loads in all the tasks)
    tasks/               # Contains each task, in its own
      some-task.js       # JavaScript file
      another-task.js
    utils/               # Utility functions all tasks share
      paths.js           # path manipulation methods
```

## `gulp/config.js` 详解

首先，让我们看一个简化的 `gulp/config.js` 文件，它存储了基础路径，以及项目的源路径和目标路径：

```
var config = {
    paths: {
        base: process.cwd(), // [1]
        dest: "build",       // [2]
        src: "src",          // [3]
        config: "config"     // [4]
    },
    assets: { // [5]
        copy: [ // [6]
                {src:  "www/*.*",         dest: "www"},
                {src:  "www/html/**/*",   dest: "www/html"},
                {src:  "www/img/**/*",    dest: "www/img"},
                {src:  "www/js/lib/**/*", dest: "www/js/lib"},
                {src:  "res/**/*",        dest: "res"}
              ]
    }
}
module.exports = config; // [7]
```

这是一个相当简单的配置文件——随着书籍的深入，我们会向其中添加更多内容。

第一部分定义了 Gulp 需要了解的各种路径，以便复制项目文件以及转换代码所需的路径。`base` 路径（`[1]`）用作其他所有路径的基础，因此你将看到的所有其他路径都是相对路径，而非绝对路径。

输出目录在 `[2]` 中指定，源目录在 `[3]` 中指定。代码转换和风格检查可能需要的配置文件在 `[4]` 中指定。每一个都相对于 `base` 路径。

每个项目都有一组资源，我们的项目也不例外——这些在 `[5]` 部分指定。在本例中，资源不多，但即便如此，也需要指定它们，以便任务知道需要处理哪些文件。我们可能有许多不同的资源，其中一些可能需要不同的处理，因此我们可以根据需要添加到此部分。目前，我们只需要复制一些文件，因此将它们添加到 `copy` 部分（`[6]`）。注意，我们通过源通配符字符串和目标路径来指定它们。这些将自动相对于 `src`（`[3]`）和 `dest`（`[2]`）路径。

最后一行（`[7]`）用于导出该文件的信息。稍后我们可以在另一个文件中 `require` 该文件（我们的大多数任务等都会这样做）。这意味着我们的资源和路径配置只需在一个地方维护。

## `gulp/settings.js` 详解

Gulp 可以接受自定义命令行参数，这些参数可用于控制各种任务的操作方式。典型的参数可能指定生成的日志量。所有这些都由 `gulp/settings.js` 文件处理。让我们来看一下：

```
var gutil = require("gulp-util");
var settings = {
    VERBOSE: gutil.env.verbose ? (gutil.env.verbose === "yes") 
                               : false
}
module.exports = settings;
```

目前，此文件中没有太多内容，因为我们确实没有需要使用命令行参数配置的任务。但随着书籍的深入，我们会向该文件添加内容。

单独来看，此文件作用不大。它只是使用 `gutil.env` 读取命令行传递的参数。在本例中，它检查是否在命令行传递了 `verbose`。如果传递了，且值为 `yes`，则 `settings.VERBOSE` 设为 `true`。如果未传递（或传递了但值为 `no`），则 `settings.VERBOSE` 设为 `false`。如果我们想在稍后的任务中利用此设置，可以直接使用。

## `gulp/tasks.js` 详解

`gulp/` 目录下还有一个文件，让我们看看 `gulp/tasks.js`：

```
var path = require("path");
var tasks = require("require-all")(path.join(__dirname, "tasks"));
module.exports = tasks;
```

正如你所见，这是一个非常短的文件。它所做的就是找到 `gulp/tasks/` 中的所有任务并将它们加载到 `tasks` 对象中。目前这将返回一个空对象，但到本章结束时，`tasks` 对象将包含 Gulp 可用的多个方法。我们使用 `require-all` 包来简化工作——这样我们就不必单独 `require` 每一个任务。稍后，当我们向 Gulp 配置添加更多任务时，这意味着我们不必再回来编辑此文件。

## `gulp/utils/paths.js` 详解

接下来，让我们看看 `gulp/utils/paths.js`：

```
var path = require("path"),
    config = require("../config"); // [1]
function makeFullPath(filepath, relativeTo) {
    var pathComponents = [config.paths.base];
    if (relativeTo) {
        pathComponents.push(config.paths[relativeTo]);
    }
    pathComponents = pathComponents.concat(filepath.split("/"));
    return path.join.apply(path, pathComponents);
}
module.exports = {
    SRC: "src",
    DEST: "dest",
    CONFIG: "config",
    makeFullPath: makeFullPath
};
```

此工具文件提供了一种机制，供任务用来构建相对于项目中源、目标、配置和基础路径的路径。它大量使用了 Node.js 的 `path` 库，以便我们的 Gulp 任务能在不同平台上工作。

## `gulpfile.js` 详解

最后，我们需要创建实际的 `gulpfile.js` 文件来启动一切。它本身不做太多事；相反，它加载其他所有内容并使用 Gulp 配置任何可用的任务：

```
require ("babel/register"); // [1]
var gulp = require("gulp"),
    tasks = require("./gulp/tasks"); // [2]
Object.keys(tasks).forEach(function(taskName) { // [3]
    var taskOpts = tasks[taskName];
    if (typeof taskOpts === "function") {
        gulp.task(taskName, taskOpts); // [4]
    } else {
        gulp.task(taskName, taskOpts.deps, taskOpts.task); // [5]
    }
});
```

第一行（`[1]`）导入 Babel，以便我们可以在任务中使用 ES2015 代码（如果选择的话）。第三行（`[2]`）导入所有可用的任务。目前这将是一个空对象，但随着我们添加任务，它将包含越来越多的函数，供 Gulp 用来复制和转换文件。

从 `[3]` 开始的代码只是获取所有可用任务并创建相应的 Gulp 任务。每个任务可以是一个函数，也可以是一个指定依赖项（及更多）的对象，因此在 `[4]` 和 `[5]` 处有两种不同的方法调用。

## 创建第一个任务：复制资源

现在我们已经创建了基本的 Gulp 配置结构，让我们创建第一个任务，将应用的资源从源路径复制到目标路径。

我们可以将此文件命名为 `gulp/tasks/copy-assets.js`，内容如下：

```
var merge = require("merge-stream"),
    gulp = require("gulp"),
    config = require("../config"),
    paths = require("../utils/paths");

function copyAssets() {
  return merge.apply(merge, config.assets.copy.map(function(asset) 
  {
    var fqSourcePath = paths.makeFullPath(asset.src, paths.SRC);
    var fqTargetPath = paths.makeFullPath(asset.dest, paths.DEST);
    return gulp.src([fqSourcePath])
      .pipe(gulp.dest(fqTargetPath));
  });
}

module.exports = {
  task: copyAssets
}
```

`copyAssets` 方法只是根据 `gulp/config.js` 中指定的项目文件结构复制大量文件。这里的代码本可以更简单，但你可能会发现稍后需要更改哪些文件需要替换、哪些不需要。因此，我们使其可配置。你只需更改 `gulp/config.js` 中 `config.assets.copy` 内的文件和目标，此任务就会做出相应反应。

让我们来看看这个任务到底在做什么：




我们使用工具方法 `paths.makeFullPath`（它使用 `path.join`）来确保配置在多个平台上都能工作。在类 Unix 系统中，路径分隔符是 `/`；但在 Windows 系统中，路径分隔符实际上是 `\`。为了简化配置，我们在 `config.assets.copy` 中使用 `/`。`makeFullPathsplits` 将每个字符串按 `/` 分割成数组，并使用 `path.join`（它知道正确的路径分隔符）来创建最终路径。

`map` 遍历数组并使用给定的转换返回一个新数组。例如，`[1, 2, 3].map(function(x) {return x*2;})` 将返回一个新数组 `[2, 4, 6]`。在我们的例子中，我们返回一个 `gulp.src(…).pipe(gulp.dest(…))` 链的数组。然后我们可以将该数组 `apply` 给 `merge` 以将所有任务组合在一起。

`apply` 是一种使用数组而不是多个参数来调用函数的方式。例如，`console.log.apply(console,[1,2,3])` 将记录 `1 2 3`。这不同于 `console.log([1,2,3])`，后者将记录 `[1, 2, 3]`。

此时，你可以在命令行输入以下内容，将项目资源从源位置复制到目标位置：

```
$ gulp copy-assets
```

很多时候，我们需要将 Gulp 流中的某些关键字转换为其他值。一个简单的例子是将 `{{{VERSION}}}` 转换为应用的版本号——例如 `1.23.4456`。这样做非常简单，但它开启了大量的可能性。

为此，我们将使用 `gulp-replace-task` 插件。该插件将特定正则表达式的所有实例替换为替换值。这些表达式可以变得非常复杂；但在我们的例子中，我们将保持简单。

我们只需要支持代码文件中的替换，所以让我们创建一个新任务，专门用于复制代码文件并在过程中应用任何必要的替换。我们将其命名为 `gulp/tasks/copy-code.js`。该文件应以如下内容开始：

```
var gulp = require("gulp"),
    replace = require("gulp-replace-task"),
    concat = require("gulp-concat"),
    pkg = require("../../package.json"),
    config = require("../config"),
    paths = require("../utils/paths");
```

接下来，我们需要定义一个方法，用于对输入流执行替换。记住，这些将是由提供给 `gulp.src()` 的模式匹配的文件：

```
function performSubstitutions() {
  return replace({
    patterns: [
      {
        match: /{{{VERSION}}}/g,
        replacement: pkg.version
      }
    ]
  });
}
```

接下来，让我们定义另一个配置设置，指定需要替换的代码文件及其存储位置。在 `gulp/config.js` 中，向 `config.assets` 对象添加一个 `code` 部分，如下所示：

```
assets: {
  copy: [ … ],
  code: {src: "www/js/app/**/*.js", dest: "www/js/app"}
}, …
```

接下来，我们需要定义将 `config.assets.code` 指定的文件复制到适当目标位置的代码。这将添加到 `gulp/tasks/copy-code.js` 中，如下所示：

```
function copyCode() {
  return gulp.src([paths.makeFullPath(config.assets.code.src, 
                     paths.SRC)])
             .pipe(performSubstitutions())
             .pipe(concat("app.js"))
             .pipe(gulp.dest(paths.makeFullPath(
                    config.assets.code.dest, paths.DEST)));
}
module.exports = {
    task: copyCode
}
```

`copyCode` 方法很容易理解。首先，使用我们指定的配置定位所有 JavaScript 文件。这些文件都通过 `performSubstitutions()` 进行处理。替换结果随后通过 `concat` 打包在一起。因此，即使我们有多个 JavaScript 文件，它们也会被打包成单个文件（`app.js`）。

### 注意

你不*必须*连接文件，如果你不想的话。然而，当你有多个 JavaScript 文件时，这意味着你必须在 `index.html` 文件中包含每一个。而如果你将它们打包成单个文件，就可以减少 `index.html` 文件中的 `script` 标签数量。

为了测试这些任务，我们可以创建两个简单的文件。第一个应放在 `src/www/` 中并命名为 `index.html`：

```
<!DOCTYPE html>
<html>
  <head>
    <script src="cordova.js" type="text/javascript"></script>
    <script src="js/app/app.js" type="text/javascript"></script>
  </head>
  <body>
    <p>Hello!</p>
    <div id="demo"></div>
  </body>
</html>
```

第二个文件应放在 `src/www/js/app/` 中并命名为 `index.js`：

```
document.getElementById("demo").textContent = "{{{VERSION}}}";
```

JavaScript 文件本身显然非常简单。其目的仅仅是证明我们的 Gulp 任务有效。如果你执行 `gulp copy-assets`，你会发现 `index.html` 已从 `src/www/` 复制到 `build/www/`。同样，如果你执行 `gulp copy-code`，你会发现 `index.js` 已从 `src/www/js/app/` 复制到 `build/www/js/app/` 并重命名为 `app.js`。如果你在编辑器中打开后者，你还会看到 `{{{VERSION}}}` 已被替换为 `1.0.0`（来自 `package.json`）。

正如你可能记得的，我们在本章前面提到过我们仍然需要一个 `config.xml` 文件。这是真的，但我们已在 `package.json` 中指定了所需的一切。如果能从模板生成有效的 `config.xml` 文件不是很好吗？这意味着我们需要更多的替换和一个合适的模板。

让我们先定义模板。这应该在 `src/config.xml` 中（完整文件见代码包）：

```
<?xml version='1.0' encoding='utf-8'?>
<widget id="{{{ID}}}" version="{{{VERSION}}}"
        xmlns="http://www.w3.org/ns/widgets"
        xmlns:cdv="http://cordova.apache.org/ns/1.0"
        xmlns:gap="http://phonegap.com/ns/1.0">
    <name>{{{NAME}}}</name>
    <description>
      {{{DESCRIPTION}}}
    </description>
    <author email="{{{AUTHOR.EMAIL}}}"href="{{{AUTHOR.SITE}}}">
      {{{AUTHOR.NAME}}}
    </author>
    <content src="index.html" />
      {{{PREFS}}}
    <access origin="*" />
…
</widget>
```

注意前面的代码中有很多替换变量。大多数都很简单：`{{{ID}}}`、`{{{NAME}}}` 等。其中一个稍微复杂一些：`{{{PREFS}}}`。这需要将 `package.json` 中的简单首选项列表渲染为 Cordova 所需的 XML 格式。

让我们创建一个名为 `gulp/utils/performSubstitutions.js` 的新工具文件，其中包含 `performSubstitutions` 方法的新版本。我们将在两个任务中需要这个新版本，因此需要将其拆分到单独的文件中。新文件应如下所示：




## 创建 `performSubstitutions` 工具函数

```javascript
var pkg = require("../../package.json"),
    replace = require("gulp-replace-task");

function performSubstitutions() {
  function transformCordovaPrefs() {
    var template = '<preference name="{{{NAME}}}" ' + 
                   'value="{{{VALUE}}}" />';
    if (pkg.cordova &&
      pkg.cordova.preferences instanceof Object) {
      return Object.keys(pkg.cordova.preferences).map(
        function(prefName) {
          var str = template.replace(/{{{NAME}}}/g,
            prefName)
            .replace(/{{{VALUE}}}/g,
              pkg.cordova.preferences[prefName]);
          return str;
        }).join("\n  ");
    }
  }

  return replace({
    patterns: [
      {
        match: /{{{VERSION}}}/g,
        replacement: pkg.version
      },
      {
        match: /{{{ID}}}/g,
        replacement: pkg.cordova.id
      },
      {
        match: /{{{NAME}}}/g,
        replacement: pkg.cordova.name
      },
      {
        match: /{{{DESCRIPTION}}}/g,
        replacement: pkg.cordova.description
      },
      {
        match: /{{{AUTHOR.NAME}}}/g,
        replacement: pkg.cordova.author.name
      },
      {
        match: /{{{AUTHOR.EMAIL}}}/g,
        replacement: pkg.cordova.author.email
      },
      {
        match: /{{{AUTHOR.SITE}}}/g,
        replacement: pkg.cordova.author.site
      },
      {
        match: /{{{PREFS}}}/g,
        replacement: transformCordovaPrefs
      }
    ]
  });
}

module.exports = performSubstitutions;
```

## 更新 `gulp/copy-code.js`

接下来，我们需要编辑 `gulp/copy-code.js` 以包含这个新版本。首先从该文件中移除 `performSubstitutions` 方法，然后在文件顶部添加以下 `require` 语句：

```javascript
var …,
  performSubstitutions = require("../utils/performSubstitutions");
```

## 创建 `copy-config.js` 任务

最后，让我们添加另一个任务来复制配置文件。我们将其命名为 `gulp/tasks/copy-config.js`，内容如下：

```javascript
var gulp = require("gulp"),
    performSubstitutions = 
      require("../utils/performSubstitutions"),
    config = require("../config"),
    paths = require("../utils/paths");

function copyConfig() {
  return gulp.src([paths.makeFullPath("config.xml", paths.SRC)])
             .pipe(performSubstitutions())
             .pipe(gulp.dest(paths.makeFullPath(".", 
               paths.DEST)));
}

module.exports = {
    task: copyConfig
}
```

## 创建聚合任务 `copy.js`

当然，我们不想运行大量单独的任务仅仅为了复制文件。所以让我们创建一个依赖这三个任务的简单任务。这样做，Gulp 将通过单个命令运行所有这些任务。

让我们创建名为 `gulp/tasks/copy.js` 的新任务。文件应包含以下内容：

```javascript
module.exports = {
    deps: ["copy-assets", "copy-config", "copy-code"],
}
```

这是迄今为止最短的任务。它所做的只是将其他三个任务列为依赖。这意味着它们将在 `copy` 之前执行。由于 `copy` 不包含任何额外代码，它只是同时执行多个任务的一种简单方式。如果你执行 `gulp copy`，你会发现在 `build` 目录下有一个新的 `config.xml` 文件。它应该看起来很像下面这样：

```xml
<?xml version='1.0' encoding='utf-8'?>
<widget id="com.packtpub.logologyv1" version="1.0.0"
        xmlns="http://www.w3.org/ns/widgets"
        xmlns:cdv="http://cordova.apache.org/ns/1.0"
        xmlns:gap="http://phonegap.com/ns/1.0">
  <name>Logology</name>
  <description>
    Dictionary application for Mastering PhoneGap book
  </description>
  <author email="kerrishotts@gmail.com"
   href="http://www.photokandy.com">
    Kerri Shotts
  </author>
  <content src="index.html" />

  <preference name="permissions" value="none" />
  <preference name="fullscreen" value="false" />
  <preference name="orientation" value="default" />
  …
  <access origin="*" />
</widget>
```

现在你已经掌握了执行替换的方法，在下一节中你将学习如何以编程方式与 Cordova 交互。

直接使用 Cordova 命令行界面很诱人，但这有一个问题：没有很好的方法来确保你编写的内容跨多个平台都能工作。如果你确定只会使用特定平台，可以继续执行 shell 命令；但我们要做的方法更灵活一些。

## 注意

本节代码受 [`github.com/kamrik/CordovaGulpTemplate`](https://github.com/kamrik/CordovaGulpTemplate) 启发。

Cordova CLI 实际上只是 `cordova-lib` 项目的一个薄包装器。Cordova CLI 能做的一切，`cordova-lib` 也能做。

因为 Cordova 项目将是一个构建产物，我们需要能够创建 Cordova 项目，除了构建项目之外。我们还需要模拟和运行应用。为了实现这一点，我们需要创建一个名为 `gulp/utils/cordova-tasks.js` 的新工具文件。在顶部我们需要引入 `cordova-lib` 和其他所需的包：

```javascript
var cordovaLib = require("cordova-lib"),
    pkg = require("../../package.json"),
    config = require("../config"),
    path = require("path"),
    settings = require("../settings"),
    paths = require("../utils/paths");

var cordova = cordovaLib.cordova.raw;
```

接下来，让我们创建在 `build` 目录中创建新 Cordova 项目的代码：

```javascript
var cordovaTasks = {
  // CLI: cordova create ./build com.example.app app_name
  //              --copy-from template_path
  create: function create() {
    return cordova.create(paths.makeFullPath(".", paths.DEST), 
      pkg.cordova.id, pkg.cordova.name,
      {
        lib: {
          www: {
            url: path.join(process.cwd(), pkg.cordova.template),
            link: false
          }
        }
      });
  }
}

module.exports = cordovaTasks;
```

虽然这比命令行上的 `cordova create` 稍微复杂一些，但你应该能看到它们的相似之处。`lib` 对象被传递仅仅是为了给项目提供一个模板（相当于命令行上的 `--copy-from`）。在我们的例子中，`package.json` 指定这应该来自本书代码包中的 `blank/` 目录。如果我们不这样做，所有的应用都会使用 Cordova 默认安装的示例 Hello World 应用来创建。

## 注意

我们的空白项目模板位于相对于项目根目录的 `../blank`。你的可能位于其他地方（因为你可能会重用同一个模板），所以 `package.json` 可以使用你需要的任何路径。或者，你可能希望模板在你的项目根目录内；在这种情况下，`package.json` 应该使用项目根目录内的路径。

我们暂时不会创建任务来使用它。我们需要定义其他几个方法来构建和模拟 Cordova 应用。首先，我们需要向 `gulp/settings.js` 添加一些额外的设置：

```javascript
var settings = { …,
  PLATFORM = gutil.env.platform ? gutil.env.platform : "ios",
  BUILD_MODE = gutil.env.mode ? gutil.env.mode : "debug",
  BUILD_PLATFORMS = (gutil.env.for ? gutil.env.for
                                   : "ios,android").split(","),
  TARGET_DEVICE = gutil.env.target ? "--target=" +
                  gutil.env.target : ""
}
```

接下来，让我们继续向 `cordovaTasks` 对象添加我们需要的其他方法：




# Cordova 任务配置

```javascript
var cordovaTasks = {
  create: function create() {
    /* as above */
  },
  cdProject: function cdProject() {
    process.chdir(paths.makeFullPath("www", paths.DEST));
  },
  cdUp: function cdUp() {
    process.chdir("..");
  },
  // cordova plugin add ...
  addPlugins: function addPlugins() {
    cordovaTasks.cdProject();
    return cordova.plugins("add", pkg.cordova.plugins)
      .then(cordovaTasks.cdUp);
  },
  // cordova platform add ...
  addPlatforms: function addPlatforms() {
    cordovaTasks.cdProject();
    function transformPlatform(platform) {
      return path.join(process.cwd(), "node_modules",
        "cordova-" + platform);
    }
    return cordova.platforms("add",
      pkg.cordova.platforms.map(transformPlatform))
      .then(cordovaTasks.cdUp);
  },
  // cordova build <platforms> --release|debug
  //                           --target=...|--device
  build: function build() {
    var target = settings.TARGET_DEVICE;
    cordovaTasks.cdProject();
    if (!target || target === "" ||
      target === "--target=device") {
      target = "--device";
    }
    return cordova.build({
      platforms: settings.BUILD_PLATFORMS,
      options: ["--" + settings.BUILD_MODE, target]
    }).then(cordovaTasks.cdUp);
  },
  // cordova emulate ios|android --release|debug
  emulate: function emulate() {
    cordovaTasks.cdProject();
    return cordova.emulate({
      platforms: [settings.PLATFORM],
      options: ["--" + settings.BUILD_MODE, 
        settings.TARGET_DEVICE]
    }).then(cordovaTasks.cdUp);
  },
  // cordova run ios|android --release|debug
  run: function run() {
    cordovaTasks.cdProject();
    return cordova.run({
      platforms: [settings.PLATFORM],
      options: ["--" + settings.BUILD_MODE, "--device",
        settings.TARGET_DEVICE]
    }).then(cordovaTasks.cdUp);
  },
  init: function() {
    return cordovaTasks.create()
      .then(cordovaTasks.copyConfig)
      .then(cordovaTasks.addPlugins)
      .then(cordovaTasks.addPlatforms);
  }
};
```

## 注意

如果你不熟悉 Promise，可能想进一步了解它们。[`www.html5rocks.com/en/tutorials/es6/promises/`](http://www.html5rocks.com/en/tutorials/es6/promises/) 是一个极好的资源。

大多数前面的任务应该相当不言自明；它们直接对应各自的 Cordova CLI 命令。然而，有几个任务需要更多解释：

### `cdProject` / `cdUp`

这些函数会更改当前工作目录。`create` 之后的所有 `cordova-lib` 命令都需要在 Cordova 项目目录中执行，而不是在我们项目的根目录中。你应该会在多个任务中注意到它们。

### `addPlatforms`

平台是直接从我们项目的依赖中添加的，而不是从 Cordova CLI。这使我们能够控制所使用的平台版本。因此，`addPlatforms` 需要做一些额外工作来指定每个平台的实际目录名称。

### `build`

这将执行 `cordova build` 命令。默认情况下，CLI 会构建每个平台。既然我们想要控制要构建的平台，因此可以使用 `BUILD_PLATFORMS` 来控制此行为。在 iOS 上，为模拟器构建与为物理设备构建是不同的。所以我们也需要一种方式来指定这一点，这就是 `TARGET_DEVICE` 的作用。它将查找为 `TARGET_DEVICE` 指定名称的模拟器。但我们可能想为物理设备构建；在这种情况下，我们将查找 `device`（或根本不指定目标），并切换到 `--device` 标志，这会强制 Cordova 为物理设备构建。

### `init`

这完成了创建 Cordova 项目、复制配置文件（并执行替换）、向 Cordova 项目添加插件，然后添加平台的繁重工作。

## Gulp 命令行开关

现在也是提及我们可以通过 Gulp 命令行开关指定各种设置的好时机。在前面的代码片段中，我们支持使用 `--platform` 指定要模拟或运行的平台，`--mode` 指定构建模式（`debug` 或 `release`），`--for` 确定 Cordova 将为哪些平台构建，以及 `--target` 指定目标设备。如果未指定这些开关，代码会指定合理的默认值；但它们也允许开发者对工作流程有额外的控制，这非常有用。例如，我们将能够使用如下命令：

```
$ gulp build --for ios,android --target device
$ gulp emulate --platform ios --target iPhone-6s
$ gulp run --platform ios --mode release
```

## Gulp 任务文件

接下来，让我们编写实际执行各种 Cordova 任务的代码。这并不难，但我们需要创建许多文件。下面代码中的每个文件名都在注释中：

### `gulp/tasks/clean.js`

```javascript
var paths = require("../utils/paths"),
    config = require("../config"),
    rimraf = require("rimraf");
function clean(cb) {
    var BUILD_PATH = paths.makeFullPath(".", paths.DEST);
    rimraf(BUILD_PATH, cb);
}
module.exports = {
    task: clean
}
```

### `gulp/tasks/init.js`

```javascript
var cordovaTasks = require("../utils/cordova-tasks");
module.exports = {
  deps: ["clean"],
  task: cordovaTasks.init
};
```

### `gulp/tasks/build.js`

```javascript
var cordovaTasks = require("../utils/cordova-tasks");
module.exports = {
  deps: ["copy"],
  task: cordovaTasks.build
};
```

### `gulp/tasks/emulate.js`

```javascript
var cordovaTasks = require("../utils/cordova-tasks");
module.exports = {
  deps: ["copy"],
  task: cordovaTasks.emulate
};
```

### `gulp/tasks/run.js`

```javascript
var cordovaTasks = require("../utils/cordova-tasks");
module.exports = {
  deps: ["copy"],
  task: cordovaTasks.run
};
```

## 任务依赖

`init` 任务有个陷阱：如果 `build/` 目录中已有任何内容，它将会失败。正如你所料，这很容易发生；所以我们还创建了一个 `clean` 任务。它使用 `rimraf` 删除指定目录，相当于使用 `rm -rf build`。然后我们确保 `init` 依赖于 `clean`。因此，每当我们执行 `gulp init` 时，旧的 Cordova 项目会被移除，并为我们创建一个新的项目。

最后，请注意所有的 `build`（和其他）任务都依赖于 `copy`。这意味着在执行所需的 Cordova 命令之前，我们 `src/` 中的所有文件都将被复制（并在必要时转换）到 `build/` 中。正如你所见，我们的任务已经变得非常复杂，但单独来看仍然易于理解。

## 可用的 Gulp 命令

这意味着我们现在可以在 Gulp 中使用以下任务：

```
$ gulp init                   # 创建 cordova 项目；
                              # 如果需要先清理
$ gulp clean                  # 移除 cordova 项目
$ gulp build                  # 复制 src 到 build；应用
                              # 转换；cordova build
$ gulp build --mode release   # 执行上述操作，但以
                              # release 模式构建
$ gulp build --for ios        # 仅为 iOS 构建
$ gulp build --target=device  # 构建设备版本而不是
                              # 模拟器版本
$ gulp emulate --platform ios # 复制 src 到 build；应用
                              # 转换；
                              # cordova emulate ios
$ gulp emulate --platform ios --target iPhone-6
                              # 同上，但打开
                              # iPhone 6 模拟器
$ gulp run --platform ios     # 复制 src 到 build；
                              # 应用转换；
                              # cordova run ios --device
```




## 使用 `cordova-tasks` 包简化 Gulp 配置

现在，你可以继续使用之前的代码，也可以使用一个 NPM 包来处理 `cordovaTasks` 部分。这样做的好处是能大幅简化你的 Gulp 配置。我们已经在 `package.json` 文件和 Gulp 配置中包含了这个包。它名为 `cordova-tasks`，由作者创建。它与之前的代码有很多相似之处。要了解它的工作原理（以及任务变得多么简单），请参阅代码包中的 `logology-v01/gulp`。

这是一个比较复杂的部分；如果你已经读到这里，不妨喝杯咖啡休息一下。接下来，我们将担心如何管理应用版本号。

## 使用 `gulp-bump` 管理应用版本号

虽然我们已经设置了 `copy-config` 和 `copy-code` 任务，在遇到 `{{{VERSION}}}` 时替换版本号，但我们没有任何任务真正去修改版本。我们当然可以直接编辑 `package.json`。但这很繁琐，而且无法自动包含在其他 Gulp 任务中。相反，让我们使用 `gulp-bump` 插件来处理这件事。

`gulp-bump` 是一个非常简单的插件：它旨在获取 `package.json`（或类似）文件，并根据特定命令编辑 `version` 属性。大多数版本采用 `major.minor.patch` 形式，我们可以要求它将任一部分加一。如果你愿意，可以增加版本的 `patch` 部分以自动跟踪构建号。

操作非常简单。首先，让我们创建另一个工具文件，这次叫作 `gulp/utils/bump.js`：

```
var gulp = require("gulp");
var bump = require("gulp-bump");

module.exports = function bump(importance) {
  return gulp.src([path.join(process.cwd(), "package.json")])
             .pipe(bump({type: importance}))
             .pipe(gulp.dest(process.cwd()));
}
```

`importance` 变量可以是以下字符串之一：`major`、`minor` 或 `patch`。接下来，让我们创建三个任务，以便直接调用此方法（同样，它们位于三个单独的文件中，在注释中指明）：

```
// gulp/tasks/version-bump-patch.js
var bump = require("../utils/bump");
module.exports = {
    task: bump.bind(null, "patch")
}

// gulp/tasks/version-bump-minor.js
var bump = require("../utils/bump");
module.exports = {
    task: bump.bind(null, "minor")
}

// gulp/tasks/version-bump-major.js
var bump = require("../utils/bump");
module.exports = {
    task: bump.bind(null, "major")
}
```

现在你可以通过执行 `gulp version-bump-patch` 直接增加版本号。不过，这只会编辑 `package.json`。如果你想让 `build/` 中的文件反映此更改，还需要执行 `gulp copy`（或 `build` 等）。

## 使用 Babel 转译 ES2015

我们在本章中已经提到了 ES2015（或 EcmaScript 2015）。现在是开始使用它的时候了。不过首先，我们需要修改 `copy-code` 任务，将 ES2015 转译为 ES5，否则我们的代码将无法在任何不支持新语法的浏览器上运行（目前仍有不少移动平台不支持）。

有几种转译器可用。我倾向于使用 Babel（[`babeljs.io`](https://babeljs.io)）。

### Tip

我们使用的是 Babel 5.x。虽然 Babel 6 最近已发布，但截至本书撰写时，演示应用和相应的 Gulp 配置尚未更新到 Babel 6.x。

我们可以使用一个 Gulp 插件，使这种转译变换变得极其简单。为此，我们需要在 `gulp/tasks/copy-code.js` 顶部添加以下内容：

```
var …,
    babel = require("gulp-babel"),
    sourcemaps = require("gulp-sourcemaps"),
    gutil = require("gulp-utils");
```

### Tip

Source maps 是调试拼图中重要的一块。当我们的代码在设备上运行时已被转换，调试可能会变得更困难，因为行号等不匹配。Sourcemaps 为浏览器提供了 ES2015 代码与最终结果之间的映射，使调试容易得多。

接下来，让我们修改 `projectTasks.copyCode` 方法：

```
function copyCode() {
    var isRelease = (settings.BUILD_MODE === "release");
    return gulp.src([paths.makeFullPath(config.assets.code.src, 
      paths.SRC)])
               .pipe(cordovaTasks.performSubstitutions())
               .pipe(settings.BUILD_MODE === "debug" ? 
                 sourcemaps.init() : gutil.noop())
               .pipe(babel())
               .pipe(concat("app.js"))
               .pipe(settings.BUILD_MODE === "debug" ? 
                 sourcemaps.write() : gutil.noop())
               .pipe(gulp.dest(paths.makeFullPath(
                 config.assets.code.dest, paths.DEST)));
}
```

我们的任务现在稍微复杂一些；但这只是因为我们想控制何时生成 source maps。调用 `babel()` 时，它会将 ES2015 代码转换为 ES5，并生成变更的 sourcemap。这使调试更容易，但也会增加文件大小（有时增加不少）。因此，在发布模式下构建时，我们不想包含 sourcemaps。所以我们将调用 `gutil.noop`，它什么都不做。

Sourcemap 功能要求我们在任何可能生成 sourcemaps 的 Gulp 插件之前调用 `sourcemaps.init`。在创建 sourcemaps 的插件执行后，我们还必须调用 `sourcemaps.write` 将 sourcemap 保存回流中。我们也可以通过调用 `sourcemaps.write(".")` 将 sourcemap 写入单独的 `.map` 文件。但在创建发布构建时，你需要小心清理该文件。

`babel` 完成将 ES2015 代码转换为 ES5 的实际繁重工作。但它需要一个小型支持库的帮助。我们将通过从 `gulp-babel` 模块复制，将此库添加到 `src/www/js/lib/`：

```
$ cp node_modules/babel-core/browser-polyfill.js src/www/js/lib
```

### Note

如果你还没有 `src/www/js/lib/` 目录，需要在执行上述命令前创建它。

接下来，我们需要编辑 `src/www/index.html` 以包含此脚本。顺便再做一些其他更改：

```
<!DOCTYPE html>
<html>
  <head>
    <script src="cordova.js" type="text/javascript"></script>
    <script src="./js/lib/browser-polyfill.js"
            type="text/javascript"></script>
    <script src="./js/app/app.js"
            type="text/javascript"></script>
  </head>
  <body>
    <p>This is static content...,
       but below is dynamic content.</p>
    <div id="demo"></div>
  </body>
</html>
```

最后，让我们在 `src/www/js/app/index.js` 中编写一些 ES2015 代码：

```
function h(elType, ...children) {
  let el = document.createElement(elType);
  for (let child of children) {
    if (typeof child !== "object") {
      el.textContent = child;
    } else if (child instanceof Array) {
      child.forEach(el.appendChild.bind(el));
    } else {
      el.appendChild(child);
    }
  }
  return el;
}

function startApp() {
  document.querySelector("#demo").appendChild(
    h("div",
      h("ul", h("li", "Some information about this app..."),
        h("li", "App name: {{{NAME}}}"),
        h("li", "App version: {{{VERSION}}}")
      )
    )
  );
}

document.addEventListener("deviceready", startApp, false);
```

本章不是关于*如何*编写 ES2015 代码，所以我不会用所有细节让你感到厌烦。我们将在下一章介绍。简而言之，前面的代码在应用运行时使用一种非常简单的 DOM 模板形式生成几个列表项。但它使用了 `…`（spread）语法处理可变参数、`for … of` 循环以及 `let` 代替 `var`。虽然它看起来很像 JavaScript，但 ES2015 差异足够大，需要一些时间来学习如何最好地使用新特性。





## 添加代码检查与代码风格检查

你现在可以执行 `gulp emulate --platform ios`（或 `android`）；应用应该能正常运行。但我们如何知道代码构建后是否能工作？更进一步说——如果代码无效，如何*阻止*构建？

我们通过向 `Gulp` 配置添加 `lint` 任务来实现这一点。代码检查很像编译；检查器会检查代码中的明显错误，如果发现任何错误则中止。有各种检查器可用（有些比其他的好），但并非所有都支持 `ES2015` 语法。目前支持得最好的是 `ESLint` ([`www.eslint.org`](http://www.eslint.org))。幸运的是，有一个非常简单的 `Gulp` 插件可以使用它。

我们本可以只做代码检查就结束了，但代码风格也很重要，也能捕捉潜在的严重问题。因此，我们还将使用 JavaScript 代码风格检查器，即 `JSCS` ([`github.com/jscs-dev/node-jscs`](https://github.com/jscs-dev/node-jscs))。

### 配置代码检查与代码风格路径

让我们创建任务来检查代码和代码风格。首先，我们需要向 `gulp/config.js` 添加一些额外配置：

```
var config = { …,
    lint: "src/www/js/app/**/*.js",
    "code-style": "src/www/js/app/**/*.js"
}
```

### 创建代码检查任务

现在，让我们创建关联的任务。从代码检查开始——它将位于 `gulp/tasks/link.js` 文件中：

```
var gulp = require("gulp"),
    eslint = require("gulp-eslint"),
    config = require ("../config"),
    settings = require("../settings"),
    paths = require("../utils/paths");
function lintCode() {
    return gulp.src(paths.makeFullPath(config.lint))
        .pipe(eslint(paths.makeFullPath("eslint.json", 
          paths.CONFIG)))
        .pipe(eslint.format());
}
module.exports = {
    task: lintCode
}
```

### 创建代码风格任务

用于检查代码风格的任务将命名为 `gulp/tasks/code-style.js`。它应包含以下代码：

```
var gulp = require("gulp"),
    jscs = require("gulp-jscs"),
    config = require ("../config"),
    settings = require("../settings"),
    paths = require("../utils/paths");
function checkCodeStyle() {
    return gulp.src(paths.makeFullPath(config["code-style"]))
        .pipe(jscs({
            configPath: paths.makeFullPath("jscs.json", 
                          paths.CONFIG),
            esnext: true
        }))
        .pipe(jscs.reporter())
        .pipe(jscs.reporter('fail'));
}
module.exports = {
    task: checkCodeStyle
}
```

### 配置文件

现在，在运行任一任务之前，你需要两个配置文件来告诉每个任务什么应该是错误，什么不应该是。我们建议暂时使用本章代码包中的文件（你可以在 `logology-v01/config` 目录中找到它们）。如果你想更改设置，可以自行修改；`ESLint` 和 `JSCS` 的网站上都有关于如何修改配置文件的信息。

### 提示

`config/eslint.json` 必须包含 `"parser": "babel-eslint"` 以强制其使用 `ES2015` 语法。不过，这在 `Gulp` 配置中是为 `JSCS` 设置的。

`config/jscs.json` 必须存在且不能为空。如果你不需要指定任何规则，请使用空的 JSON 对象（`{}`）。

现在，如果你执行 `gulp lint` 而源代码有语法错误，你会收到错误消息。代码风格同理；如果 `gulp code-style` 不喜欢代码的外观，就会生成错误。

### 注意

代码检查和风格检查**不能**保证你的代码在*逻辑上*工作。它只确保没有语法或风格错误。如果你的程序对手势响应不正确或错误处理某些数据，检查器不一定能捕捉到这些问题。

## 集成到构建流程

接下来，你应该将这两个任务添加到我们的构建、模拟和运行任务中。以下是添加后 `gulp/tasks/build.js` 的 `module.exports` 样子：

```
module.exports = {
    deps: ["copy", "lint", "code-style"],
    task: …
}
```

现在，如果你执行 `gulp build` 且存在代码检查或代码风格错误，构建将失败并报错。这在分发或运行代码前，能稍微增加一些代码至少在语法上有效的保证。

## 代码丑化（压缩）

代码丑化或压缩听起来有点痛苦，但这是我们可以添加到工作流中的一个非常简单的步骤。它会在我们以发布模式构建时减小应用程序的体积。丑化也倾向于稍微混淆代码，但不要依赖这一点来保证安全——混淆很容易被逆向。

要添加代码丑化，请将以下代码行添加到 `gulp/tasks/copy-code.js` 文件的顶部：

```
var …,
    uglify = require("gulp-uglify");
```

然后我们可以通过在 `projectTasks.copyCode` 方法中 `.pipe(concat("app.js"))` 后立即添加以下代码来丑化代码：

```
.pipe(isRelease ? uglify({preserveComments: "some"}) :
gutil.noop())
```

注意，我们仅在构建模式为 `release` 时添加了 `uglify` 方法。这意味着只有当我们执行 `gulp build --mode release` 时才会触发它。

当然，你可以指定其他选项。如果想查看所有文档，请访问 [`github.com/mishoo/UglifyJS2/`](https://github.com/mishoo/UglifyJS2/)。我们的选项包括保留某些注释（很可能是与许可证相关的），同时剥离所有其他注释。

## 默认任务

你已经完成了不少工作，但还有一件事要提：`default` 任务。如果 `gulp` 在没有参数的情况下运行，它会寻找要执行的默认任务。这可以是你想要的任何东西。

要指定它，只需在 `gulp/tasks/default.js` 添加以下任务：

```
module.exports = {
    deps: ["build"]
}
```

现在，如果你不带特定任务执行 `gulp`，实际上会启动 `build` 任务。你想用作默认任务的内容很大程度上取决于你的偏好。

## 总结

你的 `Gulp` 配置现在已经相当庞大和复杂。我们在代码包中添加了一些额外功能，所以查看本书代码包中的最终版本是个好主意。我们还向配置中添加了其他一些功能，你可能想进一步研究。它们如下：

*   用于快速迭代和测试的 `BrowserSync`
*   控制错误是否阻止后续任务执行的能力
*   帮助文本

在本章中，你学习了为什么任务运行器很有用，以及如何安装 `Gulp` 并创建不同复杂度的任务来自动化项目的构建过程和其他有用任务。我们还没玩完 `Gulp`。在下一章，我们将讨论使用 `Browserify` 进行模块化，以及如何将其集成到我们的构建任务中。
