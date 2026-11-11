
2. 首先，我们将创建在地图被点击时随机更改地图底图透明度的函数。这仅在我们的切换控件处于激活状态时发生。在 `map.addControl(control_panel);` 行之前插入以下内容：

```javascript
//Function that the map will call when the map is clicked (only when
// the toggle button is active though)
var map_event_function = function(){
    map.layers[0].setOpacity(Math.random());
}
```

3. 在创建 `TYPE_TOGGLE` 按钮之前，我们需要创建两个函数：一个在激活时调用，一个在停用时调用。这与我们在前一个示例中所做的类似。让我们在上一段代码之后这样做：

```javascript
//Create a function for the toggle button
var toggle_button_activate_func = function(){
    //Attach the map_event_function to the map
    map.events.register('click', map, map_event_function);
}

var toggle_button_deactivate_func = function(){
    //Remove the map_event_function from the map
    map.events.unregister('click', map, map_event_function);

    //Restore the layer's opacity
    map.layers[0].setOpacity(1);
}
```

4. 现在，我们需要创建切换控件本身。格式与之前类似，但我们将使用一些不同的属性。因为这是一个切换控件，我们不会使用触发函数。相反，我们将添加两个事件监听器——一个用于激活事件，另一个用于停用事件。我们还将类型设置为 `OpenLayers.Control.TYPE_TOGGLE`（`TYPE_TOGGLE` 类型——我们也可以将类型设置为 2 来实现同样的效果，但使用内置的常量名称可读性更高）。在下一行添加以下内容：

```javascript
//Create the toggle button object
var custom_toggle_button = new OpenLayers.Control.Button({
    displayClass: 'olControlCustomButtonToggle',
    eventListeners: {
        'activate': toggle_button_activate_func,
        'deactivate': toggle_button_deactivate_func
    },
    type: OpenLayers.Control.TYPE_TOGGLE
})
```

5. 你大概能猜到下一步——将控件添加到面板中。很容易！

```javascript
control_panel.addControls([custom_toggle_button]);
```

6. 现在，只需确保 control_panel 已添加到地图中：

```javascript
map.addControl(control_panel);
```

7. 最后，我们只需要添加适当的 CSS 类。我们给切换控件的类名是 olControlCustomButtonToggle，因此我们需要添加相应的类。因为这是一个切换控件，实际上我们需要两个 CSS 类——一个用于激活状态，另一个用于非激活状态。在 <head> 部分的 <style> 标签内添加以下内容：

```css
/*Custom Toggle Button*/

.olControlCustomButtonToggleItemActive {
    background:#336699;
    border:5px solid #202020;
    cursor: pointer;
    height: 28px;
    width: 28px;
}
.olControlCustomButtonToggleItemInactive {
    background:#003366;
    border:5px solid #202020;
    cursor: pointer;
    height: 28px;
    width: 28px;
}
```

8. 全部完成！当你点击新创建的控件时，activate() 事件会被触发，并且由于我们为其添加了事件监听器，相应的 toggle_button_activate_func 将被调用。它将向地图添加一个事件处理程序，等待点击。当鼠标点击事件触发时，底层的透明度将被设置为 0（完全透明）到 1（完全不透明）之间的某个随机值。然后，当再次点击切换控件时，它将被停用，图层将恢复完全不透明。

![](img/1b8cd151c9f278555f9cf255b20d244e_187_0.png)

## 发生了什么？

你现在可能已经对我们刚刚制作的控件的工作原理有了一个很好的了解。快速回顾一下，发生以下情况：

- 1. `custom_toggle_button` 切换控件等待 `activate` 或 `deactivate` 事件被触发（这些是自定义的 OpenLayers 事件，而不是像鼠标点击那样的通用 JavaScript 事件）。这是通过使用 `map.events.register` 注册事件来完成的。
- 2. 当点击 `custom_toggle_button` 控件时（这会将其切换为开启），`activate_control` 事件被检测到，对应的 `toggle_button_activate_func` 函数将被调用。
- 3. 被调用的 `toggle_button_activate_func` 函数随后向地图添加一个事件监听器（再次使用 `map.events.register`）。
- 4. 此事件监听器将等待 `click`（鼠标点击，一个通用 JavaScript 事件），然后每次收到点击时触发 `map_event_function`。
- 5. 当 `custom_toggle_button` 控件按钮接收到 `deactivate` 事件时（即它被点击并切换为关闭），对应的 `toggle_button_deactivate_func` 函数将被调用。
- 6. `toggle_button_deactivate_func` 函数使用 `map.events.unregister` 移除之前添加的地图事件监听器。因此，监听器已不存在，地图上的鼠标点击将不再导致 `map_event_function` 被触发。

就这些了！

![](img/1b8cd151c9f278555f9cf255b20d244e_188_0.png)

> 使用面板内的面板和控件时，请确保控件通过 `activate()` 或面板的 `activateControl()` 方法被激活。如果控件未被激活（或用户无法通过点击控件等方式激活），则控件无法使用。

## 摘要

本章我们涵盖了相当多的内容。我们讨论了控件背后的概念以及它们在 OpenLayers 中的使用方式。然后我们演示了如何将控件添加到地图中。我们深入介绍了 Control 类及其各种子类。我们还介绍了如何设置面板以及如何向其添加控件。为了结束本章，我们介绍了事件并学习了如何创建自己的自定义控件。

尽管本章已经相当详尽，但关于控件仍有更多内容需要学习——例如如何设置它们的样式。在第 9 章《Vector 层类》中，我们将介绍一些我们忽略的特定于 Vector 层的控件。本章内容可能很多，但下一章会短一些。我们将讨论使用 CSS 设置控件样式，并制作看起来像是应该被点击的控件，而不仅仅是大的绿色和蓝色方块。

## 7 控件样式

*到目前为止，我们已经看到如何自定义 OpenLayers 提供的几乎所有内容——图层、控件等。然而，我们并没有太关心地图界面的实际外观。*

*OpenLayers 提供了外观相当不错的默认控件，因此你可能不需要更改它们。不过，与 OpenLayers 中的几乎所有其他内容一样，更改 Control 类使用的样式和图像也很简单。*

*我们在上一章讨论了 Control 类，在本章中我们将探讨 OpenLayers 如何为控件应用样式以及如何自定义控件。*

本章我们将：

- 讨论 CSS 是什么以及它的作用
- 讨论 HTML 如何使用 CSS
- 介绍 OpenLayers 如何使用 CSS
- 介绍 OpenLayers 为控件使用的命名方案
- 为控件提供一些自定义样式

让我们开始吧！

## 什么是 CSS？

**CSS** 是 **Cascading Style Sheets**（层叠样式表）的缩写。它是一种用于指定 HTML 元素外观的标记语言。CSS 其实相当简单，如果你能跟上本书到目前为止的内容，你绝对不会有任何问题。在更深入地了解之前，让我们先谈谈 CSS 的工作原理。

## CSS 和 HTML 背后的思想

**HTML**、**CSS** 和 **JavaScript** 各自服务于三个不同的目的。

- **HTML** 如我们所见，用于创建网页的结构和 *内容*
- **CSS** 另一方面，用于控制网站的 *呈现*，即页面应如何显示
- **JavaScript** 如本书所述，用于处理网站背后的 *逻辑*

因此，当你制作一个网站时，至少需要考虑三个独立的部分——背后的 HTML、为 HTML 赋予样式的 CSS，以及处理任何逻辑或用户交互的 **JavaScript**。目前，我们将重点关注 HTML 和 CSS。

理想情况下，HTML、CSS 和 JavaScript 不应混合（即，你不应在 HTML 中使用 style 标签或像 `<center>` 这样的标签）。你的 HTML 页面应链接到外部 JavaScript 和 CSS 文件，尤其是在生产环境中。遵循这一原则的一个优点是，如果你想更改网站的外观，只需在一个地方（外部文件）编辑 CSS，而不是在每个页面上编辑。缺点是如果所有内容都包含在同一个 HTML 文件中，编辑起来会稍微更容易、更快；这就是我们到目前为止一直这样做的原因。

但我们如何编辑 CSS，这到底是什么意思？

## 编辑 CSS

如果你熟悉 CSS，请随意跳过接下来的几页，直接进入关于 *OpenLayers 如何使用 CSS* 的第二部分。如果你不熟悉，也不用担心！它非常简单。

CSS 不是编程语言；事实上，你已经熟悉它的结构。让我们看看应用 CSS 样式的“模板”代码：

```
element {
    property: value;
    property-two: value;
}
```
