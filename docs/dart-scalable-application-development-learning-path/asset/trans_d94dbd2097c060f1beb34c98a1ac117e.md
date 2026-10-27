

## 工作原理...

我们想要在页面上查看 Map 的内容。在步骤 2 中，我们使用了 polymer 包中的 `toObservable` 函数来实现这一点。该函数将一个字面量 map 转换为 Observable map。然而，`Map companies` 在 `created` 事件中被修改，该事件在组件实例化时被调用。此外，点击按钮会调用 `addCompanies` 方法，从而修改 `companies`。

步骤 3 展示了如何使用 `<template repeat="{{key in companies.keys}}"> content </template>` 创建模板循环。

这意味着对于列表 `companies.keys` 中的每个键，都会渲染模板内的内容（此处为 `<p>{{key}}: {{companies[key]}}</p>`）。

我们同样可以使用 `companies.values`，或其他 map getter 和方法，如 `companies.length`。从运行中的应用（如前一个截图所示）可以看到，每当被观察的 map 发生变化时，这些变化都会反映在网页上。

## 另请参阅

- 请参阅本章中的“绑定并循环列表”方案，了解如何使用重复模板结构。

## 使用自定义属性和模板条件

在本方案中，我们将探讨 polymer.dart 的两个特性：

- **自定义属性**：类似于普通标签的 HTML 属性，这些属性用于你的 Polymer 组件，并且可以在代码中修改。
- **模板条件**：UI 可以通过 `<template if={{condition}}></template>` 形式的声明式条件来控制，其中条件是一个涉及被观察或已发布变量的表达式。

你可以在项目 `custom_attrib` 中找到相关代码。

## 如何操作...

在本例中，我们模拟一个烤箱，以温度作为其属性。我们以文本形式显示温度，并测试温度以显示相应的提示信息，如下面的截图所示：

![](img/52c05b53a56abd53584e4fed738ed509_823_0.png)

自定义属性和模板条件

- 1. 脚本从 `web\index.html` 开始，其中通过以下行导入名为 `pol-oven` 的组件：
```
<link rel="import"href="pol_oven.html">
```
该组件在页面上使用了三次：
```
<body>
<pol-oven temperature="210"></pol-oven>
<pol-oven temperature="500"></pol-oven>
<pol-oven temperature="3000"></pol-oven>
</body>
```
从这段代码可知，组件定义在 `pol_oven.html` 中，其背后的代码位于名为 `pol_oven.dart` 的文件中。关于其他标签的讨论，请参阅第一个方案。

- 2. `pol-oven` 的代码定义在 `pol_oven.dart` 中：
```
import 'package:polymer/polymer.dart';

@CustomTag('pol-oven')
class Poloven extends PolymerElement {
  @published int temperature = 0;

  Poloven.created() : super.created() { }
}
```

- 3. 组件的结构在 `pol_oven.html` 中概述：
```
<link rel="import"href="packages/polymer/polymer.html">
<polymer-element name="pol-oven">
<template>
<p>The oven is heated to {{temperature}} degrees Celsius.  </p>
<template if="{{temperature > 400}}">
<p>This is too hot. </p>
</template>
<template if="{{temperature > 1000}}">
<p>We will burn here. </p>
</template>
</template>
<script type="application/dart" src="pol_oven.dart"></script>
</polymer-element>
```

## 工作原理...

在步骤 1 中，我们看到同一个组件 `pol-oven` 可以在同一页面上多次使用；每次出现都是该组件的不同实例。当然，也可以组合不同的组件，每次组件都包含具有不同值的 `temperature` 属性。

在步骤 2 中，展示了自定义属性必须使用 `@published` 注解；这意味着它不仅会被显示（并在更改时更新，这一点由 `@observable` 表示），而且是用于 polymer 标签的属性。

最后，在步骤 3 中，我们看到属性值被显示，同时还会根据求值条件的真假来显示一段 UI 代码。如果属性值在代码中发生变化，条件模板会自动重新求值，这可能会改变 UI。

## 另请参阅

- 请参阅本章中的“绑定到复选框”方案以获取另一个示例。

## 绑定到输入文本字段或文本区域

在本方案中，我们将向你展示如何将文本字段或文本区域中的输入值绑定到一个变量；因此，实际上我们现在从 UI 到代码执行数据绑定，并且拥有双向数据绑定。你可以在项目 `pol_text` 中找到相关代码。

## 如何操作...

- 1. 脚本从 `web\index.html` 开始，其中通过以下行导入名为 `pol-text` 的组件：
```
<link rel="import"href="pol_text.html">
```
由此可知，组件定义在 `pol_text.html` 中，其背后的代码位于名为 `pol_text.dart` 的文件中。关于其他标签的讨论，请参阅第一个方案。

- 2. `pol-text` 的代码定义在 `pol_text.dart` 中：
```
import 'package:polymer/polymer.dart';

@CustomTag('pol-text')
class Poltext extends PolymerElement {
  @observable String comps;

  Poltext.created() : super.created() { }
}
```

- 3. 组件的结构在 `pol_text.html` 中概述：
```
<link rel="import"href="packages/polymer/polymer.html">
<polymer-element name="pol-text">
<template>
<div>
Which company do you want to work for?
<input type="text" value="{{comps}}">
<p> or type a few companies:
<textarea value="{{comps}}"></textarea>
</p>
</div>
<div>
These are your favorite companies: {{comps}}
```
```
</div>
</template>
<script type="application/dart" src="pol_text.dart"></script>
</polymer-element>
```

![](img/52c05b53a56abd53584e4fed738ed509_826_0.png)

## 工作原理...

简单文本字段或文本区域的输入（内容）通过表达式 `value = {{variable}}` 绑定到变量。它会显示在页面上的简单 `{{variable}}` 表达式中，同时也会立即反映在另一个输入字段中，这证明我们实现了双向数据绑定。

## 绑定到复选框

在本方案中，我们将向你展示如何将复选框的输入值绑定到变量；因此，实际上我们现在从 UI 到代码执行数据绑定，并拥有双向数据绑定。你可以在项目 `pol_check` 中找到相关代码。

## 如何操作...

- 1. 脚本从 `web\index.html` 开始，其中通过以下行导入名为 `pol-check` 的组件：

```html
<link rel="import" href="pol_check.html">
```

由此可知，组件定义在 `pol_check.html` 中，其背后的代码位于名为 `pol_check.dart` 的文件中。关于其他标签的讨论，请参阅第一个方案。

- 2. `pol-check` 的代码定义在 `pol_check.dart` 中：
```
import 'package:polymer/polymer.dart';
@CustomTag('pol-check')
class Polcheck extends PolymerElement {
  @observable bool receive = false;

  Polcheck.created() : super.created();
}
```

- 3. 组件的结构在 `pol_check.html` 中概述：
```
<link rel="import" href="packages/polymer/polymer.html">
<polymer-element name="pol-check">
<template>
<div>
Do you want to receive our jobs newsletter?
<input type="checkbox" checked="{{receive}}">
</div>
<div>
You will receive the newsletter: {{receive}}
<p>Confirmed:</p>
<template if="{{receive}}">You will receive the newsletter</template>
<template if="{{!receive}}">You will not receive the newsletter</template>
</div>
</template>
<script type="application/dart" src="pol_check.dart"></script>
</polymer-element>
```

运行应用时，你会看到以下截图：

![](img/52c05b53a56abd53584e4fed738ed509_827_0.png)

复选框的双向绑定

