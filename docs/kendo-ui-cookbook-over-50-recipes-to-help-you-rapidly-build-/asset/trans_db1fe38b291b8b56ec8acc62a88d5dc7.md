

让我们来看看每个场景。attr 绑定用于在以下代码片段中将标签属性与模型属性绑定：

```
<a data-bind="attr: {href: websiteLink}"
target="_blank">
Click Here
</a>
```

在前面的代码片段中，data-bind 属性的值为 attr: {href: websiteLink}。这里，attr 表示标签属性绑定到 ViewModel。在此示例中，锚元素的 href 属性绑定到模型属性 websiteLink。相同的 ViewModel 对象如下面的代码片段所示：

```
<script>
var viewModel = kendo.observable({
websiteLink: 'http://www.packtpub.com',
});
kendo.bind($('a'),viewModel);
</script>
```

在代码片段中，ViewModel 对象仅包含一个属性 websiteLink。该模型属性的值是一个外部链接，即 http://www.packtpub.com。当 View 组件绑定到 ViewModel 对象时，锚元素会被更新，从而拥有一个指向 ViewModel 中提到的外部链接的 href 属性。

同样，可以通过使用绑定属性 visible 在页面上隐藏或显示 View 中的组件：

```
<div id="view">
<span id="container"
data-bind="visible: isVisible">
Some content here....
</span>
<br/>
<button id="toggleVisible"
data-bind="click: updateVisible">
Toggle Visible
</button>
</div>
```

在前面的代码片段中，span 元素的 data-bind 属性设置为 visible: isVisible。这里，isVisible 模型属性的值应为布尔类型。如果为 false，则 span 元素的 style 属性将设置为 display: none。此外，按钮的 data-bind 属性设置为 click: updateVisible。这里，按钮的 click 事件绑定到模型属性 updateVisible。前面标记的 ViewModel 对象如下面的代码片段所示：

```
var viewModel = kendo.observable({
isVisible: true,
updateVisible: function() {
this.set('isVisible', !this.get('isVisible'));
}
});
kendo.bind($('div#view'), viewModel);
```

ViewModel 中 isVisible 的初始值设置为 true，因此 span 内的文本 Some Content here... 将可见。当你点击 toggleVisible 按钮时，它会切换 isVisible 模型属性。这将显示或隐藏 span 元素。

## 使用 Source 和 Template 绑定与 ViewModels 生成 HTML 内容

在前面的示例中，探讨了 View 组件与 ViewModel 属性之间的数据绑定。绑定不仅可以在表单元素的值上执行，也可以在属性上执行，还可以用于隐藏或显示页面上的元素。在此示例中，我们将探讨 source 和 HTML 绑定的组合使用。
考虑一个下拉列表（用于选择元素），你希望用数据（option 标签）填充它。这些数据可以由 ViewModel 对象提供；使用模板生成这些 option 标签。

### 操作方法...

首先构建一个 ViewModel 对象：

```
var viewModel = kendo.observable({
 optionsData: [
 {optionValue:1, optionName: "Test1"},
 {optionValue:2, optionName: "Test2"},
 {optionValue:3, optionName: "Test3"}
 ]
});
```

此处的 ViewModel 对象包含一个 optionsData 属性，它是对象的集合（数组）。optionsData 中的每个对象包含两个属性，即 optionValue 和 optionName。optionValue 属性将用于 option 标签的 value 属性，而 optionName 将用于在下拉列表中显示的 HTML 内容。包含 option 标签绑定的模板如下面的代码片段所示：

```
<script id="select-template" type="text/x-kendo-template">
 <option data-bind=
 "attr: {value: optionValue}, html: optionName">
 </option>
</script>
```

这里，id 属性设置为 select-template 的模板包含一个 option 标签。option 标签包含 data-bind 属性，其值为 attr: {value: optionValue}, html: optionName。我们在前面的示例中已经看到属性绑定的实际效果。此处指定的另一个绑定是 html。此绑定用于为标签生成 HTML 内容。这些 option 标签应绑定到 ViewModel 对象，并封装在 select 标签内。

select 标签可以引用前面提到的模板，并使用 source 属性绑定到 ViewModel 对象：

```
<select data-template="select-template"
 data-bind="source: optionsData">
</select>
```

这里，select 标签指定了两个属性，即 data-template 和 data-bind。data-template 属性的值是我们先前定义的模板（script 标签）的 id 属性，即 select-template。这指示 kendo 将模板绑定到 select 标签，生成的内容将成为此 select 标签的 HTML 内容。

## Kendo UI 应用程序框架

data-bind 属性指定一个绑定源，该源引用 ViewModel 中的 optionData 模型属性。现在，唯一剩下的步骤是将 View 元素绑定到 ViewModel，这通过执行以下代码行来实现：

```
kendo.bind($('select'), viewModel);
```

### 工作原理...

select 标签指定了引用 optionsData 模型属性的 source 绑定。这将使对象集合可供其内部的所有标签使用。

引用页面中模板的 data-template 属性会提供对象集合（optionsData），并且模板会针对数组中的条目数进行迭代。在此示例中，optionsData 集合包含三个对象，因此模板生成三次，并作为 select 标签的 HTML 内容插入。生成的内容将如下面的代码片段所示：

```
<select data-template="select-template" data-bind="source: optionsData"> <option data-bind=" attr: {value: optionValue}, html: optionName" value="1"> Test1 </option> <option data-bind=" attr: {value: optionValue}, html: optionName" value="2"> Test2 </option> <option data-bind=" attr: {value: optionValue}, html: optionName" value="3"> Test3 </option> </select>
```

data- 属性将保持不变，生成的内容现在具有 value 属性和 HTML 内容。请注意，前面的代码可以通过在基于哈希的模板中使用 for 语句，然后将生成的输出追加到 select 标签来完成。但是，使用 data-template 属性和 source 绑定可以让你无缝地完成此操作，并编写更直观的代码。

此模式可应用于各种情况。另一个示例包括生成列表标签（li 元素）并将其插入无序列表或有序列表中。

## 使用内置验证器验证用户输入

验证用户输入数据是任何项目中的常见任务。客户端验证包括检查表单输入数据的类型或缺失数据。这并不是说服务器端验证不重要。但是，客户端验证负责执行基本验证，并在将表单数据提交到服务器进行处理之前显示错误或验证消息。因此，它为用户提供即时反馈并改善整体用户体验。

随着 HTML5 的出现，表单中的输入字段可以为其定义类型。例如，只应接受电子邮件地址的输入字段可以指定为 type=email。同样，输入字段可以具有 URL、NUMBER、RANGE、DATE、SEARCH 和 COLOR 等类型。当用户为这些字段中的任何一个指定错误数据时，浏览器将显示适当的验证消息。除了类型之外，表单元素还可以通过向输入元素添加 required 等附加属性来标记为必填。但是，一些旧浏览器不支持 HTML5 功能，包括前面提到的表单验证。Kendo UI 库带有一个内置验证器，可用于执行客户端验证。

Kendo UI 验证器鼓励用户使用类似 HTML5 的语法，并提供开箱即用的支持，以使表单验证在旧浏览器上正常工作。

### 操作方法...

让我们来看一个示例，其中一个输入文本字段标记为必填，另一个电子邮件类型的输入字段也标记为必填。请注意，这非常是 HTML5 语法，用户不必更改标记来将字段标记为必填或不同类型：

```
<form id="testView">
<label for="firstName">First Name</label>
<input id="firstName"
name="First Name"
type="text"
required
validationMessage="Please specify First Name">
<br>
```

