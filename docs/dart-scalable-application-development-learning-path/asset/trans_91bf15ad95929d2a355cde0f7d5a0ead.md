


## 结构化、测试和部署应用程序

### 准备工作

一个旨在独立运行的应用程序，无论是命令行应用程序还是 Web 应用程序，都属于应用程序包；它需要一个 `main()` 入口点。库包将被用作其他应用程序中的依赖项。所有 Dart 项目都依赖于配置文件 `pubspec.yaml`，它描述了应用程序及其依赖项，以及 `pubspec.lock` 文件。这决定了哪些库将被包含在顶层的 `packages` 文件夹中。该文件和 `packages` 文件夹由 pub 工具（更具体地说是 `pub get` 和 `pub upgrade` 命令）生成，不应手动编辑。

### 如何做...

如果您在 Dart Editor 中开发应用程序，在启动新项目时，您需要选择一个项目模板开始，如下表所示：

| 项目类型 | 模板 | 项目文件夹 |
|---|---|---|
| 独立运行的客户端或服务器应用 | 命令行 | `bin` |
| Web 或 Polymer 应用 | Web 应用程序或使用 Polymer 的 Web 应用程序 | `web` |
| Chrome 应用 | Chrome 打包应用 | `web` |
| 库 | 包 | `lib` |

`bin` 文件夹包含一个带有 `main()` 函数的启动脚本。它还可以包含 shell 脚本，例如启动服务器的脚本。在 `web` 文件夹中，您通常会有 `index.html` 和一个 `main.dart` 文件，或者通常是 `app.html` 和 `app.dart` 文件。其他资源文件如 CSS、JavaScript 文件和图片可以放在各自的文件夹 `css`、`js` 和 `images` 中。Polymer 项目通常会包含一个 `web\component` 子文件夹。不要在 `lib` 文件夹中放置带有 `main()` 的脚本。

然后，您希望按如下方式增强项目的结构：

| 文件夹 | 项目类型 | 文件 |
|---|---|---|
| 顶层 | 全部 | `README.md`、`LICENSE`、`CHANGELOG` 和 `AUTHORS` 文件 |
| `doc` | 全部 | `getting_started.md`、`todo.txt` |
| `example` | 全部 | 示例脚本，展示如何使用该应用 |
| `lib` | Web | `src`、`view` 和 `model` 的文件夹 |
| `src` | 服务器 | `src` 和 `model` 的文件夹 |
| `test` | 全部 | 单元测试脚本 |

### 它是如何工作的...

Dart Editor 中的模板为您提供了一个基本结构，但通常您希望添加一些之前指定的文件夹，以提供一个可识别且专业的结构，您可以在其中轻松找到所需的内容，例如项目中包含哪些测试。正如我们在下一个配方中将会看到的，为了使用应用程序库，它们必须放在 `lib` 文件夹中。

### 还有更多...

README 文件（`readme.md`，采用 markdown 语法；请参阅 http://en.wikipedia.org/wiki/Markdown）和 CHANGELOG 文件会显示在 pub 仓库中您包的页面上，因此其内容很重要。

可选文件夹如下：

- `mock`：此文件夹包含用于在测试环境中模拟应用程序某些行为的类，例如使用文本文件代替真实数据库。
- `tool`：此文件夹包含项目所需的工具脚本，如构建脚本、测试运行器等。
- `benchmark`：当性能至关重要时，此文件夹可以包含用于测试性能的示例。

一个同时具有客户端和服务器组件的应用程序的替代结构可以基于之前的结构放置：

- `client`
- `server`
- `core`（或 shared）

Dart 网站上的 `https://www.dartlang.org/tools/pub/package-layout.html` 链接包含一些额外的信息。

### 另请参阅

- 您可能还想阅读本章中的 *发布和部署您的应用* 配方

## 在应用程序中使用库

如前一配方所述，每种类型的应用程序都可以包含一个 `lib` 文件夹，其中至少包含模型类。这些模型类非常重要，因为它们构成了项目的骨干，因此必须在整个应用程序中可访问。您可以通过将它们放在顶层的 `lib` 文件夹中，或更好的是放在 `lib/model` 中来实现。这中心的位置也会使它们突出，并让您代码的其他阅读者更容易找到。

### 如何做...

查看 `bank_terminal` 项目的结构。模型类 `Person` 和 `BankAccount` 放在 `lib\model` 文件夹中。在 `pubspec.yaml` 文件中为您的项目命名：

```
name: bank_terminal
```

然后，使用相同的名称为 `lib` 文件夹中创建的库脚本 `bank_terminal.dart`，其中包含以下代码：

```
library bank_terminal;

import 'dart:convert';

part 'model/bank_account.dart';
part 'model/person.dart';
```

> 库的名称与您的应用名称相同！这对 Dart 脚本本身不是必需的，但这是一种常见且建议的做法。

现在当执行 `pub get` 或 `pub upgrade` 命令时，`packages` 文件夹中会出现一个 `bank_terminal` 文件夹。然后，您可以通过像导入从 pub 下载的任何托管包一样导入它，使该库在其他 Dart 脚本中使用。例如，在 `web\bank_terminal.dart` 中我们有以下代码：

```
import 'package:bank_terminal/bank_terminal.dart';
```

在这种情况下，模型类变得可用。项目结构如下图所示：

![](img/52c05b53a56abd53584e4fed738ed509_583_0.png)

### 它是如何工作的...

pub 工具就是这样设计的，以便于在应用程序中使用内部库。每个在 `lib` 文件夹（或其子文件夹）中声明库的脚本都将被 `pub get` 或 `pub upgrade` 命令拾取。其结果是，带有其所有代码的库被视为一个单独的“内部”包，因此与应用程序依赖的其他包一起放在 `packages` 文件夹中。这使得代码模型更清晰，也使 pub 更容易部署您的代码。

### 还有更多...

pub 工具可以扩展到 `lib` 的几个子文件夹，每个子文件夹包含自己的库文件（例如，位于 model 文件夹中的以 library model 开头的脚本 `model.dart`），然后是一个顶层库文件 `project_name.dart`，其前几行代码如下：

```
library project_name;
import 'model/model.dart';   // 导入 library model
import 'view/view.dart';     // 导入 library view
...
```

这样，来自 `model`、`view` 等的不同库被导入到一个大的库中，然后应用程序按如下方式导入它：

```
import 'package:project_name/project_name.dart';
```

## 使用 assert 对代码进行微测试

为您的应用程序编写测试是必要的，但花费大量时间在琐碎的测试上是不高效的。经常被低估的 Dart 关键字是 `assert`，它可用于测试代码中的条件。

### 如何做...

查看代码文件 `microtest.dart`，其中 `microtest` 是一个如前一配方中所述的内部包：

```
import 'package:microtest/microtest.dart';

void main() {
  Person p1 = new Person("Jim Greenfield", 178, 86.0);
  print('${p1.name} weighs ${p1.weight}' );
  // 许多其他代码和方法调用
  // p1 = null;
  // 再次使用 p1：
  assert(p1 is Person);
  p1.weight = 100.0;
  print('${p1.name} now weighs ${p1.weight}' );
}
```

我们导入 `microtest` 库，其中包含 `Person` 类的定义。在 `main()` 中，我们创建一个 `Person` 对象 `p1`，经过大量代码后，又想再次使用 `p1`，可能在另一个类的不同方法中。我们如何知道 `p1` 仍然引用一个 `Person` 对象？在之前的片段中很明显，但在更复杂的情况下可能更困难。如果 `p1` 例如被解除引用，没有 assert 我们会得到异常 **NoSuchMethodError: method not found: 'weight='**。

然而，如果我们使用 assert 语句，我们会得到一个更清晰的消息：**AssertionError: Failed assertion: line 9 pos 9: 'p1 is Person' is not true.** 你可以通过取消注释行 `p1 = null` 来测试它。

### 它是如何工作的...

`assert` 参数是一个逻辑条件或任何表达式（例如调用返回布尔值的函数），其解析结果为 false 或 true。如果其值为 `false`，则正常执行会通过抛出 `AssertionError` 而停止。

使用 `assert` 测试代码中任何不明显的条件；它可以替代许多简单的单元测试或难以设置的单元测试。可以放心，`assert` 仅在检查模式下工作；它不会影响部署应用程序的性能，因为它在生产模式下会被忽略。

### 还有更多...

使用 `assert` 进行测试在进入方法时测试参数条件（前置条件），以及在离开方法时测试返回值（后置条件）时非常有用。您也可以从 `assert` 中调用一个 `test` 函数（它必须返回布尔值），例如 `assert(testfunction());`。

## 单元测试 Polymer Web 应用

项目应包含一些自动化测试，可以在每次代码更改后运行，以确保之前的功能仍然有效。Dart 的 `unittest` 框架是最佳工具，Dart 网站上有一些优秀的文章可供入门。然而，测试 Polymer Web 应用程序要困难得多，因为 Polymer 的工作方式隐藏了 HTML 在 shadow DOM 中，并且它以异步方式工作，与测试代码无关。

### 准备工作

我们将在 `ClickCounter` 示例中创建一些测试（使用 `polymer` 库的标准 Web 应用程序模板）。您可以在 `polymer1` 应用中找到代码。我们在 `pubspec.yaml` 文件中包含 `unittest` 库，并创建 `test` 文件夹。

