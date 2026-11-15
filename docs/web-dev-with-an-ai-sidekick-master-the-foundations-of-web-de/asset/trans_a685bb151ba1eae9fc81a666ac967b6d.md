
* 首选 CSS 而非内联属性来设置 `fill` 和 `stroke`。这能保持 SVG 的可维护性。
* 尽可能对图标使用 `currentColor`。这是与页面其余部分集成最简洁的方式。当你更改页面上其他元素的颜色（尤其是文本）时，你的图标也会采用该颜色。
* 学习 `stroke-dasharray` 和 `stroke-dashoffset`。它们能让你创建出看起来比实际复杂得多的动画。

你不需要死记硬背所有这些。了解有哪些可用，需要时再查阅。或者让你的 AI 来实现你已知可行的功能。一旦理解了路径，SVG 的其余部分就不再神秘。

生成标记很少是主要难点。更难的工作是决定图形应传达什么、如何缩放，以及如何与页面其余部分契合。

**提示**：请解释如何使用 `currentColor` 和 CSS 变量使 SVG 图标具有主题感知能力。

在下一节中，我们将介绍 `<defs>`、`<use>`、渐变和图案，这正是 SVG 从静态绘图变为可复用设计系统的地方。

# SVG 作为可复用的设计系统

到目前为止，你编写的每个 SVG 都相当直白：形状只出现一次，样式直接应用，绘图大多是静态的。`<defs>` 让 SVG 的工作方式更像一个小型的可复用部件库。它允许你定义一次，多次复用。

`<defs>` 本身不渲染任何内容。它是 *定义* 的容器：形状、渐变、图案、滤镜和符号，它们可以在其他地方被引用。

## 使用 `<defs>` 和 `<use>` 定义和复用形状

`<defs>` 最简单的用法是定义一次形状，然后用 `<use>` 多次复用，如下面的代码和 *图 4.10* 所示：

```xml
<svg viewBox="0 0 300 120" width="300" 
     xmlns="http://www.w3.org/2000/svg">
  <defs>
    <path
      id="star"
      d="
        M 50 0
        L 61 35
        L 98 35
        L 68 57
        L 79 91
        L 50 70
        L 21 91
        L 32 57
        L 2 35
        L 39 35
        Z
      "
    />
  </defs>
  <use href="#star" x="20" y="15" fill="#f4c430" />
  <use href="#star" x="120" y="15" fill="#ff8c00" />
  <use href="#star" x="220" y="15" fill="#e63946" />
</svg>
```

![](Images/B34553_04_10.png)

*图 4.10：三颗颜色不同但复用同一形状的星星*

在前面的示例中：

* `star` 路径在 `<defs>` 内定义一次。
* 每个 `<use>` 都会创建该路径的一个副本。
* 你可以独立地为每个实例设置位置、填充、缩放、旋转和样式。

这对于图标、重复的 UI 元素、图表和装饰性图案很有用。

**警告！** 现代 SVG 使用 `href`，而非 `xlink:href`。你仍会在网上看到使用 `xlink` 的旧示例，以及设计工具导出的使用 `xlink` 的 SVG 文件，但你不应再复制那些代码。虽然它们能工作，但在 SVG 2 中已被废弃。

**提示**：请展示如何使用 `<symbol>` 和 `<use>` 构建可复用的 SVG 图标系统。

## 渐变：非平涂的填料

渐变在 `<defs>` 中定义，并通过 `url(#id)` 作为 `fill` 或 `stroke` 来引用。

下面是一个线性渐变填充矩形的示例，如 *图 4.11* 所示：

```xml
<svg viewBox="0 0 300 120" width="300" 
     xmlns="http://www.w3.org/2000/svg">
  <defs>
    <linearGradient id="sunset" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" stop-color="#ff6a00" />
      <stop offset="50%" stop-color="#ffb703" />
      <stop offset="100%" stop-color="#ffd166" />
    </linearGradient>
  </defs>
  <rect x="20" y="20" width="260" height="80" fill="url(#sunset)" />
</svg>
```

![](Images/B34553_04_11.png)

*图 4.11：线性渐变填充矩形*

前面代码中的关键概念：

* 渐变是一个命名的填料。
* `stop` 元素定义颜色过渡。
* 渐变可在多个形状间复用。

下面是一个径向渐变的示例：

```xml
<defs>
  <radialGradient id="spotlight" cx="50%" cy="50%" r="50%">
    <stop offset="0%" stop-color="white" />
    <stop offset="100%" stop-color="#90dbf4" />
  </radialGradient>
</defs>
<circle cx="150" cy="60" r="45" fill="url(#spotlight)" />
```

径向渐变适合用于高光、气泡和柔和的 UI 强调效果。

## 图案：作为填料的重复设计

图案让你用重复的图形填充形状，而非颜色或渐变。SVG 图案通常比试图用背景图片或重复的 CSS 规则伪造相同效果更简洁。

```xml
<svg viewBox="0 0 300 120" width="300" 
     xmlns="http://www.w3.org/2000/svg">
  <defs>
    <pattern id="diagonal-stripes" width="10" height="10"
      patternUnits="userSpaceOnUse"
      patternTransform="rotate(45)"
    >
      <rect width="10" height="10" fill="#f1faee" />
      <line x1="0" y1="0" x2="0" y2="10" 
            stroke="#457b9d" stroke-width="4" />
    </pattern>
  </defs>
  <rect x="20" y="20" width="260" height="80" 
        fill="url(#diagonal-stripes)" />
</svg>
```

注意前面代码的以下几点：

* 图案定义一个瓦片。
* SVG 会自动重复该瓦片。
* 图案可包含任何 SVG 内容：形状、路径，甚至文本。

图案非常适合用于图表、背景、无障碍友好的填充和微妙的 UI 纹理。

## 将 `<use>` 与渐变和图案结合

一旦所有内容都在 `<defs>` 中定义好，你就可以用简短的引用来复用它们：

```xml
<use href="#star" x="40" y="20" fill="url(#sunset)" />
<use href="#star" x="140" y="20" fill="url(#diagonal-stripes)" />
```

修改一次渐变、图案或基础形状，所有引用它的地方都会随之更新。

要记住以下几点：

* `<defs>` 是你的定义库。
* ID 用于命名和引用。
* `<use>` 是形状的实例化。
* `url(#id)` 是填料的引用。

这听起来可能很高大上，但这确实是实际项目中构建 SVG 的方式。

## 使用 `<symbol>` 创建可复用图标

在构建可复用的 SVG 图形（如图标）时，`<symbol>` 通常比配合单个形状使用 `<defs>` 更好。

`<symbol>` 元素定义一个可复用的图形，类似于 `<defs>`，但有一个重要区别：它可以拥有自己的 `viewBox`。这意味着 symbol 表现得像一个独立的 SVG。

例如：

```xml
<svg style="display: none;">
  <symbol id="icon-star" viewBox="0 0 24 24">
    <path d="M12 2l3 7h7l-5.5 4.5L18 21l-6-4-6 4 1.5-7.5L2 9h7z"/>
  </symbol>
</svg>
```

然后你可以在任何地方用 `<use>` 复用它：

```xml
<svg width="24" height="24">
  <use href="#icon-star"></use>
</svg>
```

为什么 `<symbol>` 很有用：

* 它允许每个图标使用 `viewBox` 定义自己的坐标系
* 它保持图标一致且可缩放
* 它避免在页面中重复 SVG 标记

许多图标系统将 `<symbol>` 与 `<use>` 结合使用，这样就不必重复相同的图标标记。把 `<defs>` 想象成一个存储容器，而 `<symbol>` 是一个带有自己布局规则的可复用组件。

**提示**：请举例解释 SVG 滤镜（模糊、阴影、发光）是如何工作的。

# 让 SVG 无障碍

SVG 是 DOM 的一部分，这意味着它以普通图片不具备的方式参与无障碍访问。这是好事，但也意味着你需要有意识地处理。没有无障碍文本的 SVG 对屏幕阅读器用户实际上是不可见的，而标签不当的 SVG 则会嘈杂且令人困惑。

目标很简单：如果 SVG 传达含义，提供清晰的文本替代。如果它纯粹是装饰性的，则从辅助技术中隐藏它。

## 使用 `<title>` 提供文本替代
