
# 无障碀示例

## 导航和屏幕阅读器

我们不必担心在这些（或大多数）示例中的表现，因为从任何辅助技术的角度来看，这其实并不重要。这并不是说辅助技术完全忽略 CSS；它们通常会检测 `display:none` 并忽略那些元素，但会忽略大多数其他样式。

请注意，我们在这个示例中并没有摆脱 `div` 元素。但在语义上，它们是最贴切的匹配（尽管你也可以为 `span` 找到充分的理由）。即使没有类名，我们也能大致了解 `div` 元素主要是为了将导航栏中的相关项目分组。

那么，屏幕阅读器将如何渲染呢？`nav` 元素会自动获得 `navigation` 角色，因此用户会知道他们正处于导航栏中。如果用户使用辅助技术选择 `Menu` 按钮，他们将听到“Menu，button（较长停顿）Tap to open sidebar”。如果他们浏览导航栏，他们将听到应用程序的名称（`Logology`），然后听到他们聚焦在一个输入元素上（“Search，text field… double tap to edit"）。

此例还说明了代码顺序如何影响结果的重要性。如果我们将标题置于 `Menu` 按钮之前，用户将按该顺序听取信息。在这个例子中，这不是什么大问题；上下文很容易推断。但对于其他元素，确保顺序正确至关重要。请记住，您的用户可能无法看到您应用程序的视觉层级，从而无法推断上下文。

此外，请注意，这里只出现了两个 WAI-ARIA 角色实例，而且只是为了正确处理搜索控件。首先，包裹图标和相关输入元素的 `label` 元素获得 `search` 角色；图标本身也被标记为 `presentation` 角色。这种角色告诉屏幕阅读器为了无障碀目的完全忽略该元素，这完全没问题；图标实际上没有除视觉提示之外的任何真实用途。一般来说，如果我们不提供这个第二个角色，也不会发生什么坏事：`div` 元素通常会被辅助技术忽略，除非指定了角色。

如果您担心兼容性，以下版本将详细列出 WA-ARIA 角色：

```html
<nav role="navigation" class="y-nav" >
  <div role="group" class="y-widget-group">
    <button role="button" class="y-menu-glyph" 
            title="Tap to open sidebar">Menu</button>
  </div>
  <div role="group" class="y-widget-group y-flex">
    <h1 role="heading" aria-level="1" 
        class="y-title">Logology</h1>
  </div>
  <div role="group" class="y-widget-group y-group-align-right">
    <form method="POST" role="form">
      <label role="search">
        <div role="presentation" 
             class="y-search-glyph">Search</div>
        <input role="textbox" type="text" />
      </label>
    </form>
  </div>
</nav>
```

## 无障碀列表

移动应用程序中常见的设计模式是列表视图。每个列表项均可选择以深入查看并获取更多信息。这通常用视觉上的箭头或其他图标标记。通常，这些都能很好地映射到现有的 HTML 标签，如 `ul` 和 `li`。让我们看一个示例：

```html
<ul class="y-list" >
  <li class="y-list-item">
    <button class="y-list-contents">
      <div>1.</div>
      <div class="y-flex">A list item.</div>
      <div class="y-indicator"></div>
    </button>
    <div class="y-list-actions" >
      <button class="y-fav-glyph" 
              title="Save this item as a favorite.">
              Favorite</button>
      <button class="y-share-glyph" 
              title="Share this item with a friend.">
              Share</button>
    </div>
  </li>
    ...
</ul>
```

同样，这里的样式其实并不重要，除了一个重要细节：在前面的示例中，我们还包含了列表操作。在某些平台上，这些操作可能通过在列表项上滑动来触发；在其他平台上，它们可能显示在列表项旁边。滑动是一个潜在的问题手势，因此您应确保有其他方式访问这些功能，前提是这些项目并非始终可见。此外，如果它们不在屏幕上可见，则应标记 `aria-hidden="true"` HTML 属性，否则它们仍可能被辅助技术选中。

### 注意

如果您尝试使用 `VoiceOver` 或类似技术，您可能会发现这并不像原生那样反应。在 `iOS` 上，具有附加操作的列表项会收到类似“swipe up or down to select a custom action, then double tap to activate（向上或向下滑动以选择自定义操作，然后双击激活）"的额外提示。不幸的是，无法使用 `WAI-ARIA` 精确模拟这一点。

请注意，我们用 `button` 包裹了列表内容。这是因为整个列表项是可选的（不仅仅是列表项内部的元素），我们需要向最终用户传达这一点。

## 无障碀警报和对话框

大多数应用程序在运行过程中都需要向用户显示警报或对话框。虽然您可以通过适当修改 `DOM` 和样式来实现，但这不会通知任何辅助技术有什么重要事情发生。一个简单的警报结构可能如下：

```html
<div class="y-alert-container">
 <div class="y-alert-dialog">
  <h1>Important!</h1>
  <p>An important message!</p>
  <button>OK</button>
 </div>
</div>
```

### 注意

此结构仅具代表性；通常，当需要显示警报对话框时，这将以编程方式添加。

为了提醒用户有新的警报对话框，我们需要添加一些属性：

```html
<div class="y-alert-container">
 <div class="y-alert-dialog" role="alertdialog">
  <h1>Important!</h1>
  <p role="alert">An important message!</p>
  <button>OK</button>
 </div>
</div>
```

请注意这两个新的 `role` 属性。`alertdialog` 角色标记包含消息的容器，而 `alert` 角色指示包含消息本身的元素。

这将生效，除了底层内容仍然对辅助技术可见。大多数警报对话框不会填满整个屏幕，但*是*模态的。所以，我们不希望用户对哪些控件可用、哪些不可用感到困惑。要修复此问题，我们可以向需要向辅助技术隐藏的内容添加 `aria-hidden="true"`，然后在对话框关闭时移除该属性。

还有一点：对话框显示后，需要以编程方式将焦点设置在第一个按钮上。如果我们不将焦点放在警报内的元素上，辅助技术仍会认为用户停留在先前选定的项目上。

通常，所有这些都是通过 `JavaScript` 实现的。这是一个示例：请注意，我们假设应用程序的主内容包含在一个具有 `mainWindow` 作为 `id` 属性的元素中：
