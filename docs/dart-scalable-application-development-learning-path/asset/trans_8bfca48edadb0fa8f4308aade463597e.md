

# 工作原理...

在步骤3中，复选框的`checked`属性的值通过表达式`checked="{{receive}}"`绑定到变量`receive`。通过步骤2，该值会显示在页面上的简单`{{receive}}`表达式中，但同时也用于此处的模板条件判断。

## 另请参阅

- 有关模板条件判断的更多信息，请参阅本章中的*使用自定义属性和模板条件判断*小节。

## 绑定单选按钮

在本节中，我们将向你展示如何将单选按钮的输入值绑定到一个变量；因此，实际上我们现在执行从UI到代码的数据绑定，并实现双向数据绑定。你可以在项目`pol_radio`中找到相关代码。

## 操作步骤...

- 1. 脚本从`web\index.html`开始，通过以下行导入名为`pol-radio`的组件：

```html
<link rel="import"href="pol_radio.html">
```

由此可知，该组件定义在`pol_radio.html`中，其背后的代码位于名为`pol_radio.dart`的文件中。关于其他标签的讨论，请参阅第一个小节。

- 2. `pol-radio`的代码定义在`pol_radio.dart`中：

```dart
import 'dart:html';
import 'package:polymer/polymer.dart';

@CustomTag('pol-radio')
class Polradio extends PolymerElement {
  @observable String favoriteJob = '';

  Polradio.created() : super.created();

  void getFavoriteJob(Event e, var detail, Node target) {
    favoriteJob = (e.target as InputElement).value;
  }
}
```

3. 组件的结构在`pol_radio.html`中概述：

```
<link rel="import"href="packages/polymer/polymer.html">
<polymer-element name="pol-radio">
<template>
<div on-change="{{getFavoriteJob}}">
What is your favorite job?
<div>
<label for="sd">Software developer <input name="job" type="radio" id="sd" value="Software developer"></label>
</div>
<div>
<label for="wd">Web Developer <input name="job" type="radio" id="wd" value="Web developer"></label>
</div>
<div>
<label for="tt">Tester<input name="job" type="radio" id="tt" value="Tester"></label>
</div>
</div>
<div>
You selected: {{favoriteJob}}
</div>
</template>
<script type="application/dart" src="pol_radio.dart"></script>
</polymer-element>
```

运行应用时，你将看到以下截图：

![](img/52c05b53a56abd53584e4fed738ed509_829_0.png)

## 工作原理...

在网页上（步骤3），我们看到三个单选按钮通过给它们的name属性赋予相同的值`name="job"`而被分组在一起。它们的值是字面字符串，可以将这些值传递给代码。这种连接之所以成立，是因为这些单选按钮被封装在一个带有change事件处理器的`<div>`标签中：

```html
<div on-change="{{getFavoriteJob}}">
```

步骤2中的`getFavoriteJob`方法将提取所选单选按钮的值，并将其赋值给变量`favoriteJob`：

```dart
favoriteJob = (e.target as InputElement).value;
```

该变量被定义为一个@observable，因此它会通过`{{favoriteJob}}`显示在页面上。

## 绑定选中的字段

在本节中，我们将向你展示如何将`<select>`标签中的选定值绑定到一个变量；因此，实际上我们现在执行从UI到代码的数据绑定，并实现双向数据绑定。我们展示一个公司列表。你可以在项目`pol_select`中找到相关代码。

## 操作步骤...

- 1. 脚本从`web\index.html`开始，通过以下行导入名为`pol-select`的组件：

```html
<link rel="import" href="pol_select.html">
```

由此可知，该组件定义在`pol_select.html`中，其背后的代码位于名为`pol_select.dart`的文件中。关于其他标签的讨论，请参阅第一个小节。

- 2. `pol-select`的代码定义在`pol_select.dart`中：

```dart
import'package:polymer/polymer.dart';

@CustomTag('pol-select')
class PolSelect extends PolymerElement {
  final List companies = toObservable(['Google', 'Apple',
    'Mozilla', 'Facebook']);
  @observable int selected = 2; // Make sure this is not null;
  // set it to the default selection index.
```

```
@observable String value = 'Mozilla';

    Polselect.created() : super.created();
}
```

- 3. 组件的结构在`pol_select.html`中概述：

```
<link rel="import"href="packages/polymer/polymer.html">
<polymer-element name="pol-select">
<template>
<select selectedIndex="{{selected}}" value="{{value}}">
<option template repeat="{{comp in companies}}"> {{comp}} </
option>
</select>
<div>
You selected company {{selected}} <br/>
or from index:  {{companies[selected]}} <br/>
or from binding: {{value}}
</div>
</template>
<script type="application/dart" src="pol_select.dart"></script>
</polymer-element>
```

运行应用时，你将看到以下截图：

![](img/52c05b53a56abd53584e4fed738ed509_831_0.png)

使用select字段进行绑定

## 工作原理...

步骤2定义了要显示的公司列表。它还声明了要在List中选择的索引以及作为变量的value。步骤3展示了可以使用本章中“绑定并在列表上重复”小节中的重复模板来填充`<select>`标签。它还展示了selectedIndex属性是从代码中设置的。当选择另一家公司时，这也会改变selected和value的值。

## 事件处理

在本节中，我们将向你展示如何在Polymer组件中处理事件。你可以在项目`event_handling`中找到相关代码。

## 操作步骤...

- 1. 脚本从`web\index.html`开始，通过以下行导入名为`pol-select`的组件：

```html
<link rel="import" href="pol_events.html">
```

由此可知，该组件定义在`pol_events.html`中，其背后的代码位于名为`pol_events.dart`的文件中。关于其他标签的讨论，请参阅第一个小节。

- 2. `pol-events`的代码定义在`pol_events.dart`中：

```
import 'package:polymer/polymer.dart';

@CustomTag('pol-events')
class PolEvents extends PolymerElement {
  @observable String which_event = "no event";
  @observable String thing = "";
  @observable String message = "";

  PolEvents.created() : super.created();

  enter(KeyboardEvent e, var detail, Node target) {
    if (e.keyCode == KeyCode.ENTER) {
      which_event = "you pressed the ENTER key";
    }
  }

  btnClick(MouseEvent e, var detail, Node target) {
    message = (target as Element).attributes['data-msg'];
    which_event = "you clicked the button";
  }

  txtChange(Event e, var detail, Node target) {
    var inp = (target as InputElement).value;
    which_event = "you entered $inp in the text field";
  }

  cbClick(Event e, var detail, Node target) {
```

## 工作原理...

在步骤3中，我们看到许多事件处理器定义在不同的HTML元素中。它们都具有相同的形式，即`on-event={{nameOfHandler}}`。

在Dart Editor和大多数插件中，你可以通过按`Ctrl + Space bar`（在HTML标签内）来了解哪些标签可以处理哪些事件。记住在`on`和事件名称之间加连字符；`onevent`会在运行时抛出`ReferenceError`错误。

在步骤2中，我们看到许多不同的处理器；它们都具有相同的签名：

```
nameOfHandler(Event e, var detail, Node target) {
  // code to be executed when event occurs
}
```

`target`节点是触发事件的HTML节点。如果是`InputElement`，你可以用`varinp = (target as InputElement).value;`获取输入值；注意如何通过`message = (target as Element).attributes['data-msg'];`读取按钮的`data-msg`属性。

## 与JavaScript互操作的Polymer元素

在本节中，我们将向你展示如何在Polymer组件中使用JavaScript对象。你可以在项目`pol_js`中找到相关代码。

## 操作步骤...

- 1. 脚本从`web\index.html`开始，其中通过`<link rel="import"href="pol_js.html">`导入名为`pol-js`的组件，并通过`<pol-js></pol-js>`实例化。
由此可知，该组件定义在`pol_js.html`中，其代码位于名为`pol_js.dart`的文件中。关于其他标签的讨论，请参阅第一个小节。

- 2. `index.html`文件还包含一个JavaScript对象`person.js`：

```
function Person(name, gender) {
  this.name = name;
  this.gender = gender;
  this.greeting = function(otherPerson) {
    alert('I greet you ' + otherPerson.name);
  };
}
```

```
Person.prototype.sayHello = function(times) {
```

