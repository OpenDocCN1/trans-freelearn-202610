# Backbone.js 应用程序教程

## 全局 App 对象与 Directory 视图

我们的全局 `App` 对象获得一个 `Views` 属性，用于存储对视图的引用。`Directory` 属性是对主应用程序视图实例的引用。由于只有一个实例且它相当重要，拥有全局引用使其更易于操作（并在控制台中调试）。

```
App.Directory = new App.Views.Directory({
  el: $('#display')
});
```

此代码执行 `Directory` 视图实例化，传入对现有 DOM 元素的引用，我们希望视图附加到该元素。

```
App.Directory.render();
```

实例化 `Directory` 视图后，我们继续执行 `render()` 函数，这最终会运行 Contacts 集合中每个 Contact 的 `render()` 函数。

```
App.Contacts.on('add remove', function() {
  App.Directory.render();
});
```

此代码向 Contacts 集合添加两个事件监听器，当 Contact 被添加或移除时触发。使用此语法，我们可以指定任意数量的事件，用空格分隔。事件触发时，Directory 重新渲染，显示更新后的内容。

这意味着，每当我们添加或移除项目时，DOM 会自动为我们更新！这在很大程度上减少了临时的 jQuery 调用和混乱的代码。

最后我们来看 `index.html`。这些更改反映了新的模板 `<script>` 标签，以及一些用于加载新文件的 `<script>` 标签。

```
<script id="template-contact" type="template">
```

在 `<script>` 标签中存储 HTML 使得访问内容更加容易，无需处理转义问题。`id` 属性给我们提供了一个引用代码的句柄。最好用通用的命名空间为模板元素添加前缀，在本例中我们将它们全部称为 `#template-`。否则，我们稍后可能会添加一个名为 `#contact` 的元素并导致混淆。`type` 属性设置为 `template`，这是浏览器无法识别的语言，因此会忽略它。

```
<div class="contact">
  <h2><%-firstName%><% if (lastName) { %> <%-lastName%><% } %></h2>
```

### 注意

最好始终用单个元素包裹模板，而不是让多个顶级元素相邻。这样，当我们稍后从 DOM 中删除视图时，只需移除一个元素，因为子元素会自动被移除。

这里我们打印联系人的名字变量，然后检查是否有姓氏。如果有，我们也打印姓氏；否则不做任何操作。

如果您以前使用过其他模板语言，您可能已经知道这一点：请务必不要在 View 文件中放入大量逻辑，否则它们可能会变得难以控制。将应用程序分解为模型和视图等不同部分的最大好处是，我们保持了代码类型归属的明显区分，而视图中的大量逻辑会破坏这一点。可以运行的类型包括简单的循环和条件语句，也许还有一些减少重复编写标记的辅助风格函数等。

```
<a href="mailto:<%-email%>"><%-email%></a><br />
<a href="tel:<%-phoneNumber%>"><%-phoneNumber%></a><br />
```

这两行将电子邮件地址和电话号码显示为可点击的链接，分别告诉设备发送电子邮件或拨打电话。这里没有什么特别的，我们只是利用了传统的浏览器锚标签前缀。

```
<button class="delete">Delete</button>
</div>
</script>
```

这里有一个用于联系人的 **删除** 按钮；不过，它目前是无功能的。然后我们关闭 `View` 元素和包含它的 `<script>` 标签。

## 事件监听器示例

既然您已经看到了基于我们最初输入的数据渲染的页面文档，看到 Backbone 的动态特性以及我们如何稍后更新数据并查看生成的 DOM 变化将会非常不错。

在联系人 View 的 `initialize()` 函数中，我们运行了 `this.listenTo()` 方法，它会在重新执行 `render()` 方法之前查找模型的变化。要查看此操作，请打开控制台，然后输入并执行以下命令：

```
App.Contacts.get('c1').set('lastName', 'Wiggum');
```

您应该立即看到文档发生变化。第一个联系人的姓氏已更改为 **Wiggum**！此变化同时反映在 Model 和 DOM 本身中。您不必运行任何临时的 jQuery 命令！

### 注意

`c1` 是 Backbone 的约定，用于引用集合的第一项。有更好的方法引用这些对象，例如真实的 ID 值或在创建时保持对 Model 的引用。

在我们的主 JavaScript 文件中，我们向 Contacts 集合添加了事件监听器，以便每当添加新联系人时，运行 Directory View 的 `render()` 方法。要证明这有效，请在 JavaScript 控制台中输入并执行以下命令：

```
App.Contacts.add({
  firstName: 'Wednesday',
  lastName: 'Addams',
  phoneNumber: '9895551313',
  email: 'wednesday@example.com'
});
```

### 提示

如果您想在控制台中输入命令的多行，请在每个命令后按 *Shift* + *Enter*，因为按 *Enter* 将执行未完成的命令。

您应该会看到文档立即更新并显示新联系人！

最后，如果您想查看移除联系人时会发生什么，可以在控制台中运行以下命令：

```
App.Contacts.pop();
```

随 Underscore.js 提供的模板渲染引擎相当不错。如果您喜欢大于号、小于号和百分号，您会喜欢它。但是，如果您喜欢不同的语法，我建议研究一个名为 Mustache 的替代模板引擎。

Mustache 引擎使用大括号定义变量，并且方便地在几种流行的编程语言中拥有实现库，例如 PHP 和 Ruby。这很方便，因为作为开发人员，您可以学习单一模板引擎并在任何地方使用。

许多其他替代引擎也存在，例如 Handlebars（与 Mustache 非常相似）和 HAML（具有非常简洁的语法，可轻松创建复杂的 HTML 文档）。

## View 事件

事件是一种在视图中获取用户交互并在发生时运行代码的方法。事件使用类似 `click .controls .add` 的语法指定。字符串的第一部分（直到第一个空白字符）是我们要查找的事件类型，在本例中是 `click` 事件。其后的所有内容都是选择器。

以我们前面的示例为例，如果我们使用传统的 jQuery 调用，我们可能会搜索位于具有 `controls` 类的元素内的每个具有 `add` 类的元素，并添加 `click` 事件处理程序。这有两个问题。首先，随着 DOM 的变化，我们需要再次运行此代码；否则，与选择器匹配的新元素将被遗漏。其次，我们可能会在页面上同时拥有数百个事件监听器，导致页面变慢。

请记住，视图指向 DOM 中的单个元素，即使该元素可能包含更多元素。事件绑定应用于主元素，并使用称为事件委托的机制来检查何时调用了适当的事件。因此，对于前面的示例事件，整个视图监听其任何子元素上的 `click` 事件。发生该事件时，触发事件的元素将与选择器进行匹配。如果通过，则执行我们的代码；否则，将其忽略。

### 步骤 1 – 准备工作



在正式开始编码前，我们先讨论应用程序应具备的功能，并梳理代码的职责划分。我们需要实现向目录添加新联系人的功能。添加条目这一操作不属于某个特定联系人，而是针对整个目录的。因此，我们将“Add New Contact”按钮放置在 `Directory` 视图中。同时，我们也需要支持删除现有联系人。删除操作显然是针对单个联系人的，因为每个联系人都需要专属的删除按钮。因此，我们将在 `Contact` 视图中添加删除按钮。

### 步骤 2：更新文档

使用以下代码更新 `index.html` 文件。将其置于 `div#display` 内部，`div.listing` 之前：

```
<div class="controls">
  <button class="add">Add New Contact</button>
  <form method="" action="" style="display: none;">
    <input type="text" placeholder="First Name" class="firstName" /><br />
    <input type="text" placeholder="Last Name" class="lastName" /><br />
    <input type="text" placeholder="Email" class="email" /><br />
    <input type="text" placeholder="Phone" class="phoneNumber" /><br />
    <input type="submit" value="Save Contact" />
  </form>
</div>
```

### 步骤 3：更新 Contact 视图

在 `initialize` 方法之前，按如下方式更新 `scripts/views/contact.js` 文件：

```
events: {
  'click .delete': 'remove'
},
```

此外，在主 View 对象末尾新增一个方法（需在前面的 `insert()` 方法后添加一个尾随逗号）：

```
remove: function() {
  this.model.destroy();
}
```

确保更新 `initialize` 中的 `_.bindAll()` 调用，使其包含 `remove` 方法：

```
_.bindAll(this, 'render', 'insert', 'remove');
```

### 步骤 4：更新 Directory 视图

通过在主 View 对象顶部附近添加以下代码（在 `initialize` 方法之前）来更新 `scripts/views/directory.js` 文件：

```
events: {
  'click .controls .add': 'addForm',
  'submit .controls form': 'addSubmit'
},
```

同样在该文件中，在现有的 `render()` 方法之后、主 View 对象末尾添加以下方法（别忘了在 `render` 闭合大括号后添加尾随逗号）：

```
addForm: function() {
  this.$('.controls form').show()
    .find('input.firstName').focus();
},

addSubmit: function(event) {
  event.preventDefault();
  var $form = this.$('.controls form');

  var newContact = new App.Models.Contact({
    firstName: $('input.firstName', $form).val(),
    lastName: $('input.lastName', $form).val(),
    phoneNumber: $('input.phoneNumber', $form).val(),
    email: $('input.email', $form).val()
  });

  if (newContact.isValid()) {
    App.Contacts.add(newContact);
    $form.hide();
    $('input[type=text]', $form).val('').blur();
  } else {
    alert(newContact.validationError);
  }
}
```

确保也将 `addSubmit` 和 `addForm` 添加到 `initialize` 中的 `_.bindAll()` 调用中：

```
_.bindAll(this, 'render', 'addForm', 'addSubmit');
```

我们所做的更改使我们能够使用标准表单控件与联系人 Collection 进行交互。

让我们看看 `index.html`：

```
<div class="controls">
  <button class="add">Add New Contact</button>
```

新的 `div.controls` 元素将包含应用程序中创建新联系人的所有控件和输入框。该按钮用于切换添加联系人表单的可见性。

```
<form method="" action="" style="display: none;">
```

这里的 `<form>` 标签实际上不会向服务器提交任何内容，因此我们可以将 `method` 和 `action` 属性留空。我们只是想利用它来捕获用户尝试提交新数据的时机。记住，用户通常有两种提交表单的方式：一种是按 *Enter* 键，另一种是点击 **Submit** 按钮。

### 提示

作为一个通用的 JavaScript 和 HTML 专业建议，不要添加监听按钮点击或按键的事件，并尝试检测 *Enter* 键是否被按下。

首先，你是在处理两种不同的用户操作。其次，用户可能通过其他方式提交表单，例如屏幕阅读软件或移动设备，这些设备的键盘上附有特殊的提交表单按钮。

我们希望表单默认隐藏，所以我添加了一些内联 CSS 来实现隐藏（如果这是一个真实的应用程序，你会使用外部 CSS 文件来处理这个问题，但我为了简洁起见将其保留为内联）。

```
<input type="text" placeholder="First Name" class="firstName" /><br />
<input type="text" placeholder="Last Name" class="lastName" /><br />
<input type="text" placeholder="Email" class="email" /><br />
<input type="text" placeholder="Phone" class="phoneNumber" /><br />
```

这些是用户输入信息的输入框。注意我添加了 `type` 属性并将其设置为 `text`。通常这是自动完成的，但如果省略，代码中其他地方用于清除元素的选择器将无法正确定位这些元素。

```
<input type="submit" value="Save Contact" />
</form>
</div>
```

当然，我们需要一个提交按钮，以便在表单提交时运行我们的代码。没有这个按钮，按 *Enter* 键将不会提交表单。当然，我们也需要结束表单和 View 元素标签。

接下来我们看 `scripts/views/contact.js`：

```
events: {
  'click .delete': 'remove'
},
```

View 对象上的 `events` 属性是 Backbone 的一种约定，用于指定监听哪些事件以及触发时执行哪些方法。该属性是一个对象，包含每个被监听事件的不同键值对。键代表特殊的事件委托语法，值是一个字符串，包含要执行的方法名称。

在这种情况下，当具有 `delete` 类的元素被点击时，将执行 `remove` 方法。确保使用字符串来表示方法名称，而不是对方法的实际引用，如 `this.remove`。

当事件触发且代码执行时，传递给方法的第一个参数将是事件对象，你可以对其进行进一步检查（例如，如果你想确切知道 View 中哪个元素触发了该事件）。

```
remove: function() {
  this.model.destroy();
}
```

这段代码将在 Model 上执行 `destroy()` 方法。此方法由 Backbone 提供，用于对数据执行删除操作。它不仅会删除模型，还会将其从 collection 中移除。

Backbone 实际上足够智能，能更进一步。当这段代码执行时，它甚至会为我们从 DOM 中移除相关的联系人元素。打开 Chrome 的 Element Inspector，导航到 `.listing` 元素，在删除（和添加）元素时留意 DOM 变化，亲眼见证这一过程。

然后我们看 `scripts/views/directory.js`：

```
events: {
  'click .controls .add': 'addForm',
  'submit .controls form': 'addSubmit'
},
```

这段代码为我们的 Directory View 添加了两个不同的事件监听器。由于 **Add New Contact** 按钮只是切换联系人表单的可见性，而提交联系人表单才是真正的工作，因此我们需要两个单独的事件。

```
addForm: function() {
  this.$('.controls form').show()
    .find('input.firstName').focus();
},
```




`addForm()` 方法使输入表单可见。我们使用了一点 jQuery 函数链式调用来实现这一切。首先，我们选择当前 View 中 `.controls` 元素内的表单元素。然后运行 jQuery `show()` 方法，使该元素可见（换句话说，它禁用了 `<form>` 标签上的内联 CSS）。接下来我们查找 `firstName` 输入框（这是表单中的第一个输入框，很可能是用户想要输入的那个），并让 jQuery 将焦点放在该元素上。这让用户在点击按钮后可以立即开始输入。

```
addSubmit: function(event) {
  event.preventDefault();
  var $form = this.$('.controls form');
}
```

我们通过获取事件（表单提交事件）并立即通过 `preventDefault()` 方法告诉浏览器停止其原本计划执行的操作来开始此方法。如果不这样做，表单仍会提交，我们的单页应用程序将被销毁。

接下来，我们获取表单元素并将其缓存到局部变量中。该变量名为 `$form`，并不一定是 Backbone 的约定。通过用 `$` 符号作为前缀来标识那些使用 jQuery 捕获的 DOM 元素引用变量，可以更容易地查看它们并知道它们包含什么类型的数据。Backbone 在某些地方也遵循此约定，例如 View 上的 `$el` 属性。

```
var newContact = new App.Models.Contact({
  firstName: $('input.firstName', $form).val(),
  lastName: $('input.lastName', $form).val(),
  phoneNumber: $('input.phoneNumber', $form).val(),
  email: $('input.email', $form).val()
});
```

这里我们获取表单中的所有数据并将其传递给新实例化的 `Contact` 模型。注意，当我们创建此模型时，尚未将其放入 Collection 中。它目前只是一个孤立的模型。

```
if (newContact.isValid()) {
  App.Contacts.add(newContact);
  $form.hide();
  $('input[type=text]', $form).val('').blur();
} else {
  alert(newContact.validationError);
}
```

这就是我们获取用户的原始数据并将其添加到模型中，然后再检查错误的原因。我们将使用提供给模型的 `isValid()` 方法来检查数据是否有效。

如果发现模型有效，我们将其插入 `Contacts` 集合，隐藏表单，然后使用 jQuery 提供的 `blur` 方法移除所有输入字段的焦点。如果不这样做，由于焦点仍在其中可能会发生奇怪的事情。

如果发现模型无效，我们向用户显示错误消息。此消息是先前执行的 `isValid()` 函数的结果。当然，如果这是真实的应用程序，您会希望以一种不那么侵入性的方式显示消息，例如在表单旁切换显示红色文本消息。

### Note

我遗漏了此应用程序功能中非常重要的一点。我们可以添加和删除联系人，但无法编辑它们！利用您迄今为止获得的关于 Backbone 的知识，尝试向此应用添加编辑功能。
