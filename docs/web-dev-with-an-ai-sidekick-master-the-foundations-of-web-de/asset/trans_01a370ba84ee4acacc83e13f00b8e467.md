
# CSS 样式概述

## 内部和外部 CSS

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>My First CSS</title>
  <style>
    p {
      color: blue;
    }
  </style>
</head>
<body>
  <p>This paragraph is blue.</p>
</body>
</html>
```

内部 CSS 比内联 CSS 更易于维护，但它仍然将样式定义绑定到单个 HTML 文件，因此如果你需要在多个网页中使用相同的样式，就必须复制它们。

良好实践：学习时应默认使用外部 CSS，因为它将结构和表现分离。稍后你可能会看到少量内部样式用于性能优化，特别是首屏 CSS，但不要从一开始就这么做。这是因为样式随网页一起下载并在 `<head>` 中定义，因此在内容渲染时样式会立即应用。

外部 CSS 编写在单独的 `.css` 文件中并链接到 HTML。下载网页后，浏览器会下载 CSS 文件并将其样式应用到内容。

让我们看一个示例：

1. 创建一个名为 `Chapter03` 的新文件夹。
2. 在 `Chapter03` 文件夹中，创建一个名为 `StyleBasics` 的新文件夹。
3. 在 `StyleBasics` 文件夹中，从以下链接下载名为 `index.html` 的文件：[https://github.com/markjprice/web-dev-ai/blob/main/Code/Chapter03/StyleBasics/index.html](https://github.com/markjprice/web-dev-ai/blob/main/Code/Chapter03/StyleBasics/index.html)
4. 在 VS Code 中，打开 `StyleBasics` 文件夹。
5. 在 `StyleBasics` 文件夹中，创建一个名为 `styles.css` 的新文件并添加样式定义：

```css
p {
  color: green;
}
```

6. 在 `index.html` 的 `<head>` 元素中，添加指向 CSS 文件的链接：

```html
<link rel="stylesheet" href="styles.css">
```

7. 实时预览页面。段落现在应该是绿色的，如图 3.1 所示：

![](Images/B34553_03_01.png)

图 3.1：外部 CSS 文件将段落颜色更改为绿色

如果没有变化，请检查文件名、检查路径，并确保 `<link>` 标签位于 `<head>` 内部。

从现在开始，除非另有明确说明，否则假设所有 CSS 都位于外部样式表中。

良好实践：外部 CSS 是你应该默认使用的。它允许你一次性定义样式并在网站的所有网页中重复使用。

## 选择器和规则

CSS 规则包含两个主要部分：选择器和声明块：

```css
p {
  color: navy;
  font-size: 18px;
}
```

在前面的代码中：

* `p` 是选择器。它告诉浏览器要定位哪些元素。
* 花括号 `{ }` 内的所有内容都是声明块。
* 每个声明都是一个属性（如 `color` 或 `font-size`）和一个值（如 `navy` 或 `18px`），由冒号分隔并以分号结尾。
* 此规则将深蓝色和 18px 字体大小应用于页面上的所有 `<p>` 元素。

尝试将这些规则添加到你的样式表文件中：

```css
h1 {
  color: darkslateblue;
}
p {
  color: green;
  font-size: 18px;
  line-height: 1.8;
}
```

前面的代码展示了元素选择器的示例。这些选择器选择相同标签类型的所有元素。在这些示例中，规则应用于所有 `<h1>` 元素和所有 `<p>` 元素。但如果你只想选择一部分标题和段落怎么办？我们可以使用另外两种类型的选择器来实现。

## 类和 ID

定位每个段落或标题通常是不够的。你经常希望以不同的方式设置特定元素的样式。CSS 类可以在多个元素中重复使用。你可以将此示例添加到你的网页和样式表文件中。

要添加的 HTML：

```html
<h2>Example of a class selector</h2>
<p class="intro">This will be bold and 14px.</p>
<p>This is regular content.</p>
```

要添加的 CSS 如下代码所示：

```css
.intro {
  font-size: 14px;
  font-weight: bold;
}
```

类选择器总是以点开头，如前面代码中的 `.intro`。

类或 ID 名称是一个标识符。关键规则如下：

* 不得包含空格。
* 可以包含字母、数字、连字符和下划线。
* 不得以数字开头。
* 可以以字母或连字符开头。
* 区分大小写，但依赖大小写差异是个坏主意。

还有实用的命名约定：

* 仅使用小写，因为混合大小写会增加认知负担并引发错误。
* 使用连字符而非下划线，因为连字符是 CSS 中的主流约定，且与属性命名（如 `font-size` 和 `background-color`）一致。
* 按用途命名，而非外观。例如，`.red-text` 不好，`.error-message` 好。此准则能避免后续麻烦。

元素的 ID 在页面内应唯一。因此 ID 选择器应仅选择页面上的那个唯一元素并将其样式应用于它。

要添加的 HTML 在下面的代码中高亮显示：

```html
<h1 id="main-title">My Styled Web Page</h1>
```

要添加的 CSS 如下代码所示：

```css
#main-title {
  text-transform: uppercase;
}
```

ID 选择器以井号开头，如前面代码中的 `#main-title`。谨慎使用 ID。类的扩展性更好，也更易于重用。

良好实践：避免使用 ID 进行样式设置，因为它们只能应用于一个元素。即使你目前只有一个元素需要某种样式，你也可能想改用类来应用。这样，如果稍后添加另一个需要相同样式的元素，你可以轻松应用。

## 层叠和具体性

CSS 代表层叠样式表，层叠很重要。当多条规则针对同一元素时，浏览器必须决定哪一条生效。

考虑以下 CSS：

```css
p {
  color: black;
}
.intro {
  color: darkgreen;
}
```

以及此 HTML：

```html
<p class="intro">Hello world</p>
```

元素选择器将段落设为 `black`。类选择器规则规定将带有 `.intro` 类的任何元素着色为 `darkgreen`。哪一个生效？

段落将呈深绿色而非黑色，因为类选择器比标签选择器更具体。

> **提示**：请用真实示例解释 CSS 层叠，展示元素、类和 ID 选择器之间的冲突。展示 CSS 规则冲突的示例并解释为何一条规则胜过另一条。

具体性规则非常复杂，让我们简化它们：

* 元素选择器最弱
* 类选择器覆盖元素选择器
* ID 选择器覆盖两者
* 内联样式覆盖其他所有

```
Element < Class < ID < Inline
```

顺序也很重要。如果两条规则的具体性相等，样式表中靠后编写的那条生效。例如，以下 CSS 有两条类规则：

```css
.intro {
  color: darkgreen;
}
.branding {
  color: darkred;
}
```

如果我们有一个同时应用了这两个类的元素：

```html
<p class="intro branding">Hello world</p>
```

然后先应用 `.intro` 样式，接着应用 `.branding` 样式，使文本颜色变为 `darkred`。

要对同一元素应用多个类，请用空格分隔。class 属性中类名的顺序无关紧要。`class="intro branding"` 和 `class="branding intro"` 效果相同。

具体性解释了许多“为什么我的 CSS 不生效？”的时刻。

> **提示**：什么是 CSS 具体性，如何逐步计算它？

## 盒模型

网页上的每个元素都是一个矩形框。如果你想要行为可预测的布局，理解这一点很重要。
