

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

