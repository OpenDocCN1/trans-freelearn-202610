
So, the process for making an object global is to:

- 1. 在 `init()`（或任何函数）外部使用 var `variable_name` 定义对象/变量；
- 2. 在 `init()` 函数内部创建对象时，引用 `variable_name` 时不要指定 var。

注意我们创建 `wms_layer_map` 层对象的代码：`var wms_layer_map = new OpenLayers.Layer.WMS(...)` 由于我们在 `init` 函数内部使用了 var，对象只在此处创建，并且只能在 `init` 函数内部引用。`wms_layer_map` 对象只能在其被定义的相同作用域内访问。在本例中，由于它是在 `init()` 内部定义的，我们只能用 `init()` 函数内部的代码来引用它。要将其定义为全局以便在函数外部访问，我们可以这样做：

```
var map;
var wms_layer_map
function init(){
  map = new OpenLayers.Map ...
  wms_layer_map = new OpenLayers.Layer.WMS( ...
}
```

![](img/1b8cd151c9f278555f9cf255b20d244e_102_0.png)

> 使用全局变量可以让事情变得更容易，但如果可能的话请尽量避免使用。如果确实使用它们，请跟踪它们并在命名时小心。

## 测验 – 变量作用域的工作原理

请看下面的代码。变量 `final` 的值会是什么？

```
var a = 3;

//Create a function that will set a variable called 'b' equal to 5
function set_variable(){
  var b = 5;
}

//Call the function
set_variable()
final = a + b;
```

- 1. 8
- 2. 3
- 3. 5
- 4. 将会报错，因为变量 b 不能在测试函数外部访问。

## 层类方法

我们已经介绍了基础层类的属性，并讨论了大多数层子类。到目前为止，剩下的主题是基础层类的函数或方法。由于我们将讨论层类的函数，所有层子类都继承并使用这些函数。在进入函数之前，让我们先介绍如何在 Firebug 中访问层对象。

> 函数和方法有什么区别？方法只是一个术语，指属于对象或类的函数。

## 动手实践 – 定义全局层对象变量

让我们使层对象成为全局变量，访问层的某些属性，并调用其中一个层的方法。

- 1. 复制 `chapter3_ex2_options_config.html`。在 `init()` 函数正上方添加以下行：
- 2. 在你的 `init()` 函数内部，删除每个层对象名称前面的 `var` 声明。例如，你的 `wms_layer_map` 定义的第一行现在应该看起来像这样：
- 3. 保存文件（我们将称其为 `chapter3_ex5_global_variables.html`）。
- 4. 在 Firefox 中打开它并启用 Firebug 控制台。
- 5. 在控制台面板中输入并运行：`wms_layer_map;`。你应该会看到类似这样的输出：
- 6. 在 Firebug 中输入并运行以下命令：

```
var wms_layer_map, wms_state_lines, wms_labels, wms_water_depth, wms_roads;
```

```
wms_layer_map = new OpenLayers.Layer.WMS(
```

```
Object { options=Object, more... }
```

```
wms_layer_map.name
```

- 7. 这里我们直接访问层的一个属性。你应该会看到层的 name 属性输出：
  
  `Base layer`
  
- 8. 在 Firebug 中输入并运行以下命令：
  
  `wms_layer_map.setVisibility(false);`
  
- 9. 假设你处于完整范围，现在你应该看不到该层了，因为你刚刚关闭了基础层的可见性。

## 发生了什么？

我们刚刚将 `wms_base_ground` 设为全局变量，访问了它的一个属性，并调用了它的方法。我们之前讨论的任何 Layer 类属性都可以通过 `layer_object.property;` 访问。

因此，现在我们对如何即时访问层属性和函数有了更多的了解，让我们回顾一下基础层类的方法。现在，就像属性一样，大多数层子类（例如 `Layer.WMS`）都有自己的函数集，但也继承了基础层类的所有函数。

## 层类方法定义

以下所有函数都可以像我们调用 `setVisibility()` 函数那样调用；通过调用 `layer_object.function_name()`；某些方法需要你传入参数。我们不会介绍基础层类的所有函数，因为有些已弃用、存在于 Map 类函数列表中，或超出本书范围。

所需的参数（如有）列在参数列下。

| 函数 | 描述/操作 | 参数 | 返回值 |
|---|---|---|---|
| `setName( newName )` | 根据传入的字符串为层设置新名称。这也会更新图层切换控件中层的显示名称，并可能触发 changelayer 事件。 | `newName`: `{String}` 层的新名称。 | - |
| addOptions( options ) | 为层添加附加选项。参数的行为类似于 options 参数，允许你设置层的属性。 | options: {Object} 包含层属性和值的键值对的匿名对象。 | - |
| redraw() | 重绘层。如果层成功重绘则返回 true，如果层无法重绘则返回 false。在向矢量层添加要素时需要调用此方法，我们将在第 9 章介绍。 | - | {Boolean} |
| getVisibility() | 检查层是否可见。返回 true 表示层可见，false 表示不可见。 | - | {Boolean} |
| setVisibility( visibility ) | 将可见性设置为 true 或 false，隐藏/显示层并重绘它。可见性决定层是否应显示。 | visibility: {Boolean} true 显示层，false 隐藏层。 | - |
| calculateInRange() | 如果层可以在地图当前分辨率下显示，则返回 true 或 false。如果 alwaysInRange 属性为 true，则始终返回 true。 | - | {Boolean} |
| setIsBaseLayer( is_base ) | 如果传入 true，将层转为基础层；如果传入 false，则不再使其成为基础层。 | is_base: {Boolean} 设置为 true 使层成为基础层，false 使其成为覆盖层。 | - |
| `setOpacity( opacity )` | 根据传入的值设置层的不透明度。传入的 `opacity` 值是一个浮点数，有效范围为 0 到 1，0 表示完全透明，1 表示完全不透明。0.5 表示 50% 透明。 | `opacity:` `{Float}` 期望的不透明度值。 | - |

## 勇敢尝试 – 调用一些函数

现在我们已经介绍了本书中将使用的一些 Layer 类函数，请尝试自己调用它们。打开 Firefox，启用 Firebug，并尝试在层对象上调用上述一些函数（通过 `map.layers` 数组或全局层对象本身访问层对象）。再次提醒，不要害怕尝试或破坏东西！

## 总结

本章我们讨论了 Layer 类。我们介绍了层的概念以及基础层和覆盖层之间的区别。我们深入探讨了 WMS Layer 类。我们讨论了 Layer 类的属性和方法，并介绍了各种层子类。

呼！这可能信息量很大。我们单独介绍了许多 Layer 类，但尚未使用不同类型的 Layer 类制作地图。这是 OpenLayers 擅长的一项功能，也是地图“拼贴”驱动因素。不过，要了解如何将不同层类组合在一起，我们需要对投影有一个基本了解——这是下一章的主题。

## 4

## 理解投影

当你看地图时，你看到的是一个三维物体（地球）的二维表示。因为我们在创建地图时本质上“丢失”了一个维度，所以没有地图能完美地表示地球。所有地图都有一些失真。

失真取决于你使用的投影（将地球表面表示在二维平面上的方法）。在本章中，我们将更详细地讨论投影是什么、为什么重要以及如何在 OpenLayers 中使用它们。我们还将介绍一些其他基本地理原理，以帮助更好地理解 OpenLayers。

本章将介绍：

- 地图投影的概念
- 投影类型
- 经度、纬度和其他地理概念
- OpenLayers 投影类
- 坐标转换

开始吧！
