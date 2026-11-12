

- 放弃对不支持原生功能的浏览器的支持
- 使用原生功能的自定义实现

完全不使用该功能显而易见，并且大多数情况下不是首选方式。开发人员希望利用现代浏览器及其功能来提供更好的性能和最佳功能。

仅因为缺少一小段原生代码就排除某些浏览器也是令人沮丧的。别误会，我不是在说支持 IE7。新的原生功能通常首先以前缀（例如 moz、webkit、ms 等）形式在某个 JavaScript 引擎中出现，然后才在所有现代浏览器中无前缀提供。这个过程通常需要几年时间，并且不能保证所有功能都会在所有引擎中实现。

## Polyfill – 实现缺失的功能

我们希望在包含原生功能的同时，不破坏应用程序对旧版浏览器和不同 JavaScript 引擎的向后兼容性。可以通过在 JavaScript 中自己实现原生功能，并在原生功能不可用时将其用作回退解决方案来实现——这也就是所谓的 Polyfill。

需要注意的是，当然，用 JavaScript 对不同原生功能进行 Polyfill 存在很大差异。通过实现 forEach、indexOf、map 等数组方法来提供 ECMAScript 5 兼容性的 Polyfill，比为完整的 SVG 1.1 规范或 CSS Media Query 规范编写 Polyfill 要容易得多。

Polyfill 通常是一个函数，用于为不支持该功能的 JavaScript 引擎实现原生功能。如果我们谈论支持整个 API（如前面提到的 SVG 或 Media Query 规范），我们通常谈论的是抽象层而不是 Polyfill。下面是一个示例（略微过时）：Internet Explorer 8 及更低版本不支持 SVG 规范，而支持其自己的矢量标记语言（VML）。要在这些浏览器中使用矢量图形，必须使用一个抽象层，该层提供一致的 API，并根据用户的浏览器生成 SVG 或 VML；RaphaëlJS 就是 SVG 的此类抽象层。

> 可以在 Github 上的 Modernizr 项目 WIKI 中找到流行 Polyfill 列表，网址为 https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-Browser-Polyfills。

## Shims – 拦截 API 调用

如果您在互联网上查找关于跨浏览器兼容性的资源，将会遇到许多不同的术语，它们都以类似的方式使用。Shim 是这些术语之一，常与 Polyfill 一起使用。

Shim 是一段代码的通用表达，用于拦截原生 API 调用（例如函数），以提供该函数的修改后行为。这种行为范围从实现函数、修改参数列表和调整返回值，到您所能想到的任何内容。因此，Polyfill 是实现旧版浏览器缺失功能的 Shim。

## Wrappers – 抽象 API

我需要再次为提及 IE8 而道歉——当 SVG 变得重要时，这个话题非常流行。为 IE8 对 SVG API 进行 Polyfill 简直是疯狂，并且经常会导致不兼容。因此，在两个 API 之上实现一个 Wrapper 要好得多。

Wrapper 抽象了 API（例如 SVG）的行为，并为开发者提供更高级别的 API。目标是能够更改 Wrapper 的底层技术，例如输出 VML 而不是 SVG，从而在不修改开发者 API 的情况下创建跨浏览器矢量图形。

## D3 的兼容性

有人可能会问 D3 作为库的兼容性如何；然而，这个问题没有简单的答案。D3 是一个用于 DOM 转换的库，旨在使用 CSS、HTML 和 SVG 等底层技术。它只是一个支持框架，让您访问原生 API 和功能。当然，用户不必在 D3 中使用 SVG，也可以同样使用 Canvas。用户可以只使用地图投影、事件系统或缩放。因此，在谈论 D3 的跨浏览器兼容性时，我们必须更加概括。

> D3 的许多内部功能（例如缩放、计时器、颜色、插值等）可作为额外的包在 Github 上获得，网址为 `https://github.com/d3`。当您只需要应用程序中 D3 功能的某个部分时，这非常方便。

如果您使用 D3，必须检查应用程序中使用的底层技术的兼容性。如果使用 D3 操作 SVG，必须检查 SVG 兼容性；如果使用 SMIL，则检查 SMIL 兼容性；如果使用 Canvas，以此类推。您应该明白我的意思。

如果您只想使用 D3 的单个部分，并且对内部核心的兼容性感到好奇，那么必须检查 ES5 数组函数的兼容性。下面的截图显示了 ES5 的一般跨浏览器支持。可以看出，IE8 是问题制造者：

![](img/72559bd4713b2b89631dcca187d7e2db_232_0.png)

ES5 的跨浏览器兼容性（来源：http://caniuse.com/#feat=es5）

在 IE8 和 IE7（老天保佑！）中使用 D3 功能推荐的 Polyfill 是 `Aight` 库，它主要包含 ES5 数组函数和 API。该库可通过 `https://github.com/shawnbot/aight` 在 Github 上找到。

## 跨浏览器 Media Queries

在创建跨浏览器响应式可视化时，很可能需要处理 Media Query 的支持、度量和单位问题。后两者可以使用 Modernizr（`cssvhunit`、`cssvmaxunit`、`cssvminunit`、`cssvwunit` 等）和回退解决方案轻松检测和修复。

但是，在 Media Query 支持不可用时修复它相当棘手。为什么呢？如果检测 Media Queries 的可用性很简单（使用 Modernizr），那么对其进行 Polyfill 则非常麻烦。主要原因是 Media Queries 通常出现在 CSS 样式中，而不是 JavaScript 代码中。到目前为止，您只了解到 JavaScript 函数和 API 可以使用 JavaScript 进行 Polyfill。因此，如果我们想要 CSS 的 Polyfill（必须用 JavaScript 编写），则必须始终解析完整的 CSS 文档以扩展其功能。这意味着，我们不能依靠原生样式性能，而必须使用 JavaScript 解析 CSS，应用所有已知样式，并修复新选择器、属性、样式等，然后再应用它们。这通常会导致次优解决方案（性能方面）。

## Window.matchMedia() 及其 Polyfill

CSS Media Queries 的 JavaScript 对应物是 window.matchMedia() 函数。如果该函数可用，则支持 CSS Media Queries；如果不可用，我们必须为该函数提供 Polyfill，并使用 JavaScript Polyfill 修复 CSS 样式。

在下面的截图中，可以看到不同浏览器中 matchMedia 函数的支持情况。可以识别出导致问题的 IE9 和 Opera Mini 8。因此，如果应用程序支持这两个版本，则需要包含 matchMedia 的 Polyfill。

![](img/72559bd4713b2b89631dcca187d7e2db_233_0.png)

matchMedia 的跨浏览器兼容性（来源：http://caniuse.com/#feat=matchmedia）

可以通过下载最新源代码或使用 bower 包管理器并从终端运行以下命令来安装 matchMedia Polyfill：

```
bower install matchmedia
```

让我们回顾第 1 章（使用响应式设计、Bootstrap 和 D3.js 入门）中的以下示例。使用 matchMedia 函数，可以在 JavaScript 中仅对某些设备类型应用代码：

```
if (matchMedia('screen and (max-width: 480px)').matches) {
    // user is on a smartphone
}
```

但是，当我们查看样式表时，通常会找到以 CSS 编写的样式，例如以下代码片段：

```
@media screen and (max-width: 480px) {
    body {
        /* user is on a smartphone */
    }
}
```

如前一章所发现的，现在需要使用 matchMedia Polyfill 将 Media Query 功能添加到 CSS 样式中。以下部分将完全做到这一点。

