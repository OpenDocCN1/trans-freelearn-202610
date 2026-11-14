

# 模块

本节我们将介绍的最后一个 ES2015 新特性是模块。在 JavaScript 中实现模块化的方法有很多，但 ES2015 将模块化直接引入了语言本身。语法非常简单；但如果你不熟悉模块，可能需要一段时间才能完全消化吸收。

在 ES2015 中，模块是一个独立的 JavaScript 文件，其中有一个或多个方法或变量标记了 `export`。这是一个新关键字，允许你指明某个值或方法应当可供程序的其他部分使用。你可以选择使用 `export default` 提供默认导出。

相应地，你的代码通过 `import` 来使用模块。你可以选择用 `import *` 导入模块导出的所有内容，也可以按需导入特定部分。

让我们看一个例子。首先，定义一个简单的模块：

```
// math.js
export function add(a, b) {
  return a + b;
}
export function sub(a, b) {
  return a - b;
}
```

我们可以在另一个 JavaScript 文件中使用该模块，如下所示：

```
// another.js
import * from "math.js";
// 上面这行等同于下面这行
// import {add, sub} from "math.js"
console.log(add(2, 5));
```

现在，假设我们想创建一个高级数学库，但希望将简单的方法直接暴露给使用者。我们可以直接从另一个文件导出方法和值，如下所示：

```
// advancedMath.js
export * from "math.js"
export function mul(a, b) {
  return a * b;
}
`

这些内容并没有什么惊天动地的——大多数开发者都在使用类似的模式，但将其内置到语言中确实很不错。此外，它确实提供了更多灵活性，因为你可以按需选择要导入和导出的内容。

遗憾的是，你需要一个提供该特性支持的打包工具——Babel 默认不支持。我们将在本章后面的 *使用 Browserify* 一节中介绍这一点。

## 更多信息

你已经阅读了大量信息，但这些对于理解本书其余部分的代码至关重要。请花些时间熟悉本章提供的信息。然后，我建议你阅读一些我们未涉及的其他特性。以下网站提供了良好的切入点：

*   [http://www.2ality.com](http://www.2ality.com)
*   [https://babeljs.io/docs/learn-es2015/](https://babeljs.io/docs/learn-es2015/)

### 提示

并非每个 ES2015 特性都被我们使用的转译器（即 Babel）支持。关于 Babel 支持哪些特性的具体信息，请参阅 [http://kangax.github.io/compat-table/es6/](http://kangax.github.io/compat-table/es6/)。

为了测试你对所有 ES2015 特性的理解，或者如果你只是想尝试一下，Babel 提供了一个优秀的沙箱，你可以在其中编写 ES2015 代码，查看转译结果，并验证代码是否正常工作。你可以在线访问 [https://babeljs.io/repl/](https://babeljs.io/repl/)。

极少有应用程序需要完全自定义的代码。大多数应用程序都能从模块化和代码复用中受益，重要的是要善用模块化，以便更高效地构建应用。

许多开发者（甚至一些公司）都患有一种常见的综合症：非我发明综合症。患者极其犹豫使用自己未创建的代码、算法或任何其他资产。这种综合症的成因显而易见——它确保了一个人不会因第三方代码而承担责任，不会因不当授权资产而被起诉，等等；但这也意味着必须不断重复造轮子。这不可避免地导致算法更多漏洞、安全性更低，因为每个人都在尝试完全从零开始构建应用。

如果你读过其他关于移动应用开发的书籍，很可能已经接触过模块化自己的代码。与其拥有一个包含*所有内容*的大型单体 JavaScript 文件，不如分离代码，使其更易于理解（并使每个关注点清晰分离）。

然而，将这一理念扩展到更广泛的 JavaScript 世界极其有用；那里有大量采用宽松许可证的开源模块，可供你作为应用程序的基础。这意味着必须担心妥善遵守许可要求，但也使你能够组装经过充分测试的构建模块，以此作为应用程序的基石。

Browserify ([http://browserify.org](http://browserify.org)) 是一个工具，允许你使用庞大的 Node.js 包仓库。许多包原样即可在浏览器中正常工作，但 Node.js 拥有不同的全局环境。Browserify 将这个 Node.js 环境垫片化到浏览器中，这意味着几乎任何 Node.js 包都可以在浏览器环境中工作。大多数包会在文档中说明如果不支持浏览器。

### 注意

Browserify 还用于将应用的所有依赖打包成一个或多个大文件。这在生产环境中很有用，此时你可能想提供一个包含所有代码的压缩文件。

此外，既然我们可以使用 Node.js 包，不妨也使用 Node.js 包管理器 `npm`。我们已经在用它安装开发依赖，如 Cordova、Gulp 等。但我们也可以用 `npm` 来管理应用程序的依赖。

你应该花点时间浏览 [https://www.npmjs.com](https://www.npmjs.com)。应该很明显，那里有*海量*资源。利用 `npm` 作为资源能极大地加速你自己的开发。

## 修改我们的 Gulp 配置

遗憾的是，要想将 Browserify 和 ES2015 结合使用，我们需要修改 Gulp 配置。以前我们直接调用 `babel` 来转换代码，现在得稍微花点心思；必须调用 `browserify`，然后由 `browserify` 自行调用 `babel`。这是一个微妙的区别，但很重要。

首先，安装新的开发依赖：

```
$ npm --save-dev install vinyl-source-stream@1.1.0 vinyl-buffer@1.1.0 browserify@11.2.0 babelify@6.3.0
$ npm --save install babel@5.8.23
$ npm --save-dev uninstall gulp-babel
```

### 注意

我们安装的前两个模块将允许我们将 `browserify` 的输出转换为 Gulp 可处理的格式。

然后，我们需要在配置文件 (`gulp/config.js`) 顶部添加一些配置：

```
assets: { ...,   
     code: {src: "www/js/app/index.js", dest: "www/js/app"}
}
```

接下来，我们需要修改 `gulp/tasks/copy-code.js` 文件。首先，在顶部添加额外的 require 语句：

```
var ...,
    babelify = require("babelify"),
    browserify = require("browserify"),
    buffer = require("vinyl-buffer"),
    source = require("vinyl-source-stream");
```

然后，修改 copyCode 使其如下所示：

