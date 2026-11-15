# 视图与模板

视图代表一个 DOM 元素，以及该元素内的子元素。模板是一堆嵌入了特殊语法的 HTML，它允许你轻松创建视图。

模板需要以某种形式供你的脚本访问。从技术上讲，它们只是一串文本。向脚本提供模板的最常用方法是通过 `<script>` 标签将它们嵌入主 SPA 的 DOM 中。这可能会让你感到惊讶，因为 `<script>` 标签通常用于加载 JavaScript。然而，那个 `type` 属性是有原因的。几年前，为 Internet Explorer 执行而嵌入 Visual Basic 脚本并不罕见，也许有一天我们会看到浏览器直接执行 Dart。

只需指定一个浏览器未曾听闻的类型，浏览器就不会尝试执行 `<script>` 标签内的代码。通过在 `<script>` 标签上提供 `id` 属性，我们可以使用代码访问其内容。我们不将模板代码存储在隐藏的 `textarea` 中，是因为模板中的双引号需要转义，这有点麻烦。我们也不将模板作为隐藏元素存储在 DOM 中，因为用未使用的模板元素使 DOM 变得复杂是没有必要的。

在使用视图时，我们将利用 Underscore.js 内置的模板渲染功能。此模板渲染器受 Ruby ERB 语法启发。如果你的 HTML 包含 `<%= variable %>`，它将打印出变量的值。如果你想转义该代码以防止 HTML 注入，请使用 `<%- variable %>`。你甚至可以通过使用 `<% expression() %>` 来运行任意 JavaScript 代码。

## 步骤 1 – 创建一些文件和文件夹

在 `scripts` 中创建一个名为 `views` 的新目录，以及两个新文件 `scripts/views/contact.js` 和 `scripts/views/directory.js`。

## 步骤 2 – 更新联系人视图

将以下代码添加到 `scripts/views/contact.js`：

```javascript
App.Views.Contact = Backbone.View.extend({
  template: _.template($('#template-contact').html()),
  $container: null,

  initialize: function(options) {
    _.bindAll(this, 'render', 'insert');

    this.$container = options.$container;

    this.listenTo(this.model, 'change', this.render);
    this.insert();
  },

  render: function() {
    this.$el.html(this.template(this.model.attributes));

    return this;
  },

  insert: function() {
    this.$container.append(this.$el);
  }
});
```

## 步骤 3 – 更新目录视图

将以下代码添加到 `scripts/views/directory.js`：





## 步骤 4 – 更新文档

将以下代码添加到 `index.html` 中，位于现有的 `<div>` 标签之后：

```
<script id="template-contact" type="template">
  <div class="contact">
    <h2><%-firstName%><% if (lastName) { %> <%-lastName%><% } %></h2>
    <a href="mailto:<%-email%>"><%-email%></a><br />
    <a href="tel:<%-phoneNumber%>"><%-phoneNumber%></a><br />
    <button class="delete">Delete</button>
  </div>
</script>
```

此外，将以下代码添加到 `index.html` 中，位于结束标签 `</body>` 之后现有的 `<script>` 标签之后：

```
<script src="scripts/views/contact.js"></script>
<script src="scripts/views/directory.js"></script>
```

## 步骤 5 – 更新主脚本

更新 `main.js` 中的 `App` 对象，使其如下所示：

```
var App = {
  Models: {},
  Collections: {},
  Views: {},
  Contacts: null,
  Directory: null
};
```

另外，在 `main.js` 中，移除添加空联系人的代码，以及遍历联系人并将它们显示在 DOM 中的代码。

用以下新代码替换这些代码：

```
  App.Directory = new App.Views.Directory({
    el: $('#display')
  });

  App.Directory.render();

  App.Contacts.on('add remove', function() {
    App.Directory.render();
  });
```

## 步骤 6 – 刷新浏览器

添加完所有新代码后，回到浏览器并刷新页面。你应该会看到类似下面截图的内容：

![步骤 6 – 刷新浏览器](img/5668OS_04_03.jpg)

正如你所见，尽管我们只有一个 Collection 和一个 Model，但现在这个应用程序中已经使用了两个 View。在你的 Backbone 应用程序中，这种情况会经常发生；不同类型的类之间不一定存在一对一的映射关系。在我们的代码中，Directory View 代表我们的主应用程序视图。它将始终代表单个元素，该元素将始终存在于页面中。然而，Contact View 可能根本不存在，也可能存在多个。

我们再次详细查看代码。首先让我们看看 `scripts/views/contact.js`。这段代码与我们单个的 Contact View 相关。

```
App.Views.Contact = Backbone.View.extend({
```

同样，我们首先扩展相应的内置 Backbone 对象。

```
  template: _.template($('#template-contact').html()),
  $container: null,
```

`template` 属性是用于存储模板对象的 Backbone 惯例。在这种情况下，你可以看到我们正在 DOM 中搜索一个标识为 `#template-contact` 的元素（即我们之前添加的 `<script>` 标签），然后获取其中包含的 HTML。接着我们将此 HTML 传递给 `_.template` 方法，该方法会解析字符串并对其进行一些处理，有点像编译模板。这样，消耗 CPU 的计算只需执行一次，我们稍后就可以重复使用它。

`$container` 属性将指向我们的 Contact View 所在的容器元素。这不是 Backbone 惯例，只是一个方便的方法。

```
  initialize: function(options) {
    _.bindAll(this, 'render', 'insert');

    this.$container = options.$container;

    this.listenTo(this.model, 'change', this.render);
    this.insert();
  },
```

每次实例化此视图时都会运行 `initialize()` 方法，因此它类似于许多基于类的语言中的构造函数。实例化类时可以传入额外参数。正如你在前面的代码中所见，我们将此参数称为 `options`，尽管这不一定是 Backbone 惯例。

`_.bindAll()` 方法是一个便捷函数，用于快速指定我们希望重新绑定的该对象的其他方法。前面的代码将 `this` 重新绑定为指向整个 View 对象，而不是仅指向正在使用它的局部方法（`render()` 或 `insert()`）。

我们传入 View 对象的选项之一是它们应被添加到的容器元素，因此我们获取此元素并将其作为 `$container` 分配给我们的类，以便稍后引用。

调用 `listenTo()` 的那一行是真正神奇之处的开始。实例化 View 时，我们可以传入一个 `model` 属性，该属性随后会被分配为该视图的模型（Backbone 会自动处理此分配；我们不必在 `initialize` 方法中手动执行）。然后，View 会向模型添加一个事件处理程序，以便每当该模型的数据更新时，View 都会收到通知并执行 `render()` 方法。这非常有用，避免了我们在每个可能更新数据的地方都编写临时的 DOM 更新代码。

之后，我们运行 `insert()` 方法，它会将我们的元素插入到容器中。我们不在 `render()` 方法中执行此操作，因为我们不想不必要地反复将元素插入容器中！

```
  render: function() {
    this.$el.html(this.template(this.model.attributes));

    return this;
  },
```

这是 `render()` 方法，是 Backbone 的一项惯例。每当我们希望 View 更新其 DOM 元素时，就会执行此方法。在我们的示例中，我们只需获取模型的属性（还记得之前检查过它吗？那是存储模型数据的地方），将其传递给本地缓存的模板，获取生成的 HTML，并将其赋值给 View 对象上的 `$el` 属性。`$el` 是用于存储当前 View 的 DOM 元素引用的 Backbone 惯例。最后我们返回对象引用以便于链式调用。

```
  insert: function() {
    this.$container.append(this.$el);
  }
```

这段代码只是将我们的元素追加到容器元素中。如果我们每次运行 `render()` 都运行这段代码，我们的元素会不断地乱序跳动。

接下来我们将注意力转移到 `scripts/views/directory.js`。这段代码与我们的主应用程序视图相关。

```
App.Views.Directory = Backbone.View.extend({
  initialize: function() {
    _.bindAll(this, 'render');
  },
```

这些都应该是不言自明的。我们扩展 Backbone View 对象，并有一个仅处理绑定 `this` 变量的 `initialize` 函数。

```
  render: function() {
    var $container = this.$('.listing').empty();
```

此视图中的 `render()` 方法做的第一件事是清空现有元素。第一次运行此函数时，它将移除我们的 `Houston, we have a problem` 文本。后续调用将移除各个联系人视图的标记。

```
    App.Contacts.each(function(contact) {
      new App.Views.Contact({
        model: contact,
        $container: $container
      }).render();
    });
```

这段代码获取我们的 Contact Collection 并遍历每个单独的联系人。在此过程中，我们为该联系人实例化一个新的 View。我们传入的 `model` 属性是我们从 Collection 中遍历出的模型。我们还传入了对该视图主元素的引用，它将作为我们子视图的 `$container`。

一旦我们的 View 被实例化，我们立即运行它们的 `render()` 方法，它们就会在 DOM 中变得可见。

接下来我们将查看 `scripts/main.js`。这些更改为我们提供了一种更好的方式来遍历联系人并将它们显示在 DOM 中。

```
var App = {
  Models: {},
  Collections: {},
  Views: {},
  Contacts: null,
  Directory: null
};
```



