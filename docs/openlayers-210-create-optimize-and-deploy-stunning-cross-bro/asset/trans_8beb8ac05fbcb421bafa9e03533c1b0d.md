

/*Zoom to Max Extent Control*/
#panel_div .olControlZoomToMaxExtentItemInactive {
  width: 18px;
  height: 18px;
  background:#333399 url('http://dev.openlayers.org/releases/OpenLayers-2.9.1/img/zoom-world-mini.png');
}
</style>

- 5. 现在打开页面，你应该会看到类似以下内容：

![](img/1b8cd151c9f278555f9cf255b20d244e_174_0.png)

## 发生了什么？

我们刚刚创建了一个面板并在其中放置了三个控件。通过点击面板中的其中一个按钮，你可以激活该控件（具体来说，会调用 activateControl 方法）。当创建希望以编程方式激活的面板内部控件时，你必须调用面板的 activateControl 方法，并传入期望的控件对象。如果你点击 ZoomBox 控件按钮，你会注意到 Navigation 控件被停用了——一次只能有一个控件处于活动状态。

> 由于面板控件是一个添加到地图上的控件对象，因此面板控件可在 `map.controls` 数组中使用。对于添加到面板中的控件也是如此。

## 小测验 – zoomBox 控制类型

ZoomBox 控制是哪种类型的控件？

- 1. TYPE_BUTTON
- 2. TYPE_TOGGLE
- 3. TYPE_TOOL
- 4. 导航控件没有控制类型
- 5. TYPE_I_DONT_KNOW

提示：如果你不确定，可以查看 ZoomBox 控件对象的 `type` 属性。

## OpenLayers.Control.Panel

正如我们在前面的示例中看到的，我们能够创建一个面板并将其放置在地图外部的 `<div>` 元素内。如果我们不指定 `div` 属性，面板将直接添加到地图中。我们制作的面板非常简单，但即使创建更高级的面板也不是太困难。

在我们开始变得过于花哨之前，让我们先回顾一下面板类的属性和方法，这样我们就知道自己在处理什么。

## 面板属性

| 名称 | 类型 | 描述 | 默认值 |
|---|---|---|---|
| controls | {Array {OpenLayers.Control}} | 指定添加到面板中的控件对象数组。如前例所示，你可以在调用本身中实例化对象（通过 new OpenLayers.Control）或使用之前创建的对象。创建面板时，你需要使用此属性指定面板接收的控件，或使用面板的 addControls 方法。示例：controls: [new OpenLayers.Control.Navigation(), new OpenLayers.Control.ZoomBox()] | [] |
| autoActivate | {Boolean} | 确定面板在添加到地图时是否应被激活。如果未将其设置为 true，则在使用面板之前必须调用面板的 activate() 方法。 | true |
| defaultControl | {OpenLayers.Control} | 指定要激活的默认控件。这应指向面板将拥有或已拥有的控件。如上例所示，你应传入一个已经实例化的控件对象。如果你指定值时实例化对象，而不是传入已创建的对象，则不会发生任何事情，因为默认控件将指向与面板拥有的对象不同的对象。示例：defaultControl: my_control_object | null |
| saveState | {Boolean} | 如果此属性为 true，则面板内部控件的活动状态（即其状态）在面板被停用时将被保存，并在激活时恢复。它还将在首次激活后覆盖 defaultControl 属性（如果已设置），而是恢复之前处于活动状态的控件。 | false |

## 面板函数

Panel 类目前我们只需要知道两个函数。

-   - **Activate()**：此函数将激活面板控件本身。默认情况下，`autoActivate` 属性设置为 `true`，此函数将自动调用。但是，如果面板默认未激活，则必须调用此函数才能使用面板。
-   - **activateControl({OpenLayers.Control})**：此函数将激活传入的控件。传入的控件必须是属于该面板的控件。这是在用户点击面板内部控件时被调用的函数。如果你希望手动激活控件，将调用此函数。

## 现在怎么办？

呼！我们已经涵盖了 OpenLayers 提供的大多数控件，并介绍了如何使用面板。有很多内容需要消化，但不要感到不知所措。我们涵盖如此多的原因之一是为了以后如果需要复习如何使用控件或可以用控件做什么时，可以回头参考本章。

因此，现在我们已经完成了这些细枝末节的细节，让我们继续进行一些稍微复杂一点的内容。

## 创建自己的控件

到现在为止，你已经熟悉（或可能厌倦了）控件描述。让我们让事情变得更有趣一些，自己创建一个控件。计划如下：

- 1. 快速讨论 Button 控件类。
- 2. 创建一个具有自定义功能的 Button 控件。
- 3. 将按钮添加到面板中。
- 4. 哇！你刚刚创建了自己的自定义控件。

因此，在我们讨论创建自定义按钮之前，让我们先回顾一下上面部分中未涵盖的 **Button** 子类，我们将用它来创建自定义控件。

## OpenLayers.Control.Button

这里其实没什么好讲的，因为我们已经介绍过基础控件类。我们需要讨论的两个属性是 `type` 和 `displayClass` 属性。我们在创建自己的自定义控件时会使用这两个属性。

## 按钮属性

| 名称 | 类型 | 描述 | 默认值 |
|---|---|---|---|
| type | {Integer} | 这是一个整数，表示此按钮控件的类型。这是我们之前讨论的内容——按钮类型。OpenLayers.Control.TYPE_BUTTON 用 1 表示，OpenLayers.Control.TYPE_TOGGLE 用 2 表示，OpenLayers.Control.TYPE_TOOL 用 3 表示。默认情况下，作为 OpenLayers.Control.TYPE_BUTTON 控件，其值为 1。但是，通过更改此属性的值，你可以更改其控件类型。你可以通过使用 {Integer} 数字（如 2）或使用内置常量名称来指定类型。常量名称以 OpenLayers.Control 开头，后跟控件类型（如 OpenLayers.Control.TYPE_BUTTON）。两种方法都能达到相同的效果，但使用常量名称更简洁、更易读。 | 1 (or OpenLayers.Control.TYPE_BUTTON) |
| displayClass | {String} | 此字符串将指定按钮将被赋予的 CSS 类名称。例如为 'olControlMyButton'。你指定的任何类，OpenLayers 都会在末尾自动加上 'ItemInactive'。如果它是可切换的按钮，则在按钮激活时会在末尾加上 'ItemActive'。我们将在下一章详细介绍这一点。如果未定义此属性，它将获得默认值 'olControlButtonItem'（末尾加上 'Inactive' 或 'Active'）。 | olControlButtonItemInactive |

## 按钮函数

这里我们也只需要介绍一个函数。此函数的主要用途之一是我们通常在实例化按钮对象时将其传入。

-   - **trigger()**：此函数在点击控件按钮时被调用（假设它是一个 `OpenLayers.Control.TYPE_BUTTON` 控件）。如果我们在创建按钮时未定义它（或在按钮创建后定义它），点击按钮时不会发生任何事情。当在创建按钮对象时将此作为属性传入时，应传入函数返回代码（函数名称），而不是实际的函数。因此，要在实例化时定义它，请在 `new OpenLayers.Control.Button` 调用之外创建一个函数，然后将该函数名称作为 trigger 引用。例如，

```
var my_func = function(){ alert( 'Stop clicking me'); }
var my_button = new OpenLayers.Control.Button({ trigger: my_func
});
```

## 创建自定义按钮

你现在对 Button 类有了更多的了解，让我们将这些知识付诸实践。我们将创建一个相当简单的自定义按钮（控件类型为 `TYPE_BUTTON`），它将更新底图层的透明度并将地图缩放到随机位置。我们将介绍创建按钮时应包含的几个基本内容：

-   - 指定按钮的 `displayClass` 以赋予其样式（可选）
-   - 创建一个函数并将其设为触发函数
-   - 将按钮添加到面板中，然后将该面板添加到地图中

## 动手实践 – 创建一个简单按钮

让我们创建一个具有 CSS 类并且包含触发函数、点击时执行一些操作的简单按钮。

- 1. 使用第 1 章中的地图模板创建一个新页面。我们将创建一个按钮和一个放置按钮的面板（不过这次面板将在地图内部）。

