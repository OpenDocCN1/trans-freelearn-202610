# 基于 URL 的路由

当用户在传统的请求/响应网站中导航时，他们完全期望后退和前进历史记录导航能够工作。由于传统网站的无状态特性，这很容易实现。浏览器只需跟踪之前请求的 URL，并在用户请求时再次请求它们。

使用 JavaScript 构建的单页应用程序是有状态的，将需要程序员干预以允许这种历史记录功能的表象。幸运的是，Backbone 提供了机制使此过程更容易。

用户确实期望应用程序的 URL 在屏幕内容改变时发生变化。用户也希望与他人共享 URL，以及将这些 URL 加入书签以供稍后浏览。通常，传统 Web 应用程序具有指向服务器上资源的 URL，以正斜杠开头。每次请求新页面时，服务器都会根据该 URL 响应一个全新的页面。

长期以来，JavaScript 无法在不重定向用户到另一页面的情况下更改 URL，除非更改 `#` 符号后的 URL（通常称为锚点）。然而，有一个相对较新的 JavaScript API，允许在不需要锚点的情况下更改 URL，只要用户保持在当前域内即可。这称为 `pushState`，与许多令人兴奋的 Web 技术一样，它仅在现代浏览器中可用，因此可能不符合您项目的标准。幸运的是，Backbone 可以使用任一方法，并为两者提供通用接口。

当使用 `pushState` 时，当用户首次请求应用程序深处的页面时，浏览器将向服务器发出完整请求，因为它无法知道 URL 是正常的服务器请求 URL，还是部分 URL 是使用 `pushState` 构建的。为了使这些请求正常工作，服务器端应用程序需要以考虑到这一点的方式构建。由于我们仍在使用本地文件系统在浏览器中构建此应用程序，我们将放弃使用 `pushState`，改用传统的基于锚点的 URL（Backbone 的默认方式）。

## Step 1 – 创建路由器脚本

创建 `scripts/router.js` 文件并添加以下代码：

```
App.Routers.Contact = Backbone.Router.extend({
  routes: {
    'contacts': 'contacts',
    'contacts/add': 'contactsAdd',
    'contacts/remove/:id': 'contactsRemove'
  },

  contacts: function() {
    App.Directory.render();
    App.Directory.addFormHide();
  },

  contactsAdd: function() {
    App.Directory.addForm();
  },

  contactsRemove: function(id) {
    App.Contacts.remove(id);
    App.Directory.addFormHide();
  }
});
```

## Step 2 – 更新文档

更新 `index.html`，在加载 `scripts/main.js` 之后添加以下行，以便我们可以访问这个新脚本文件：

```
<script src="scripts/router.js"></script>
```

## Step 3 – 更新主脚本

更新 `scripts/main.js`，使主 `App` 对象拥有两个更多的键，如下所示（不要忘记尾随逗号）：

```
var App = {
  …,
  Routers: {},
  Browser: null
};
```

同样在 `scripts/main.js` 中，在我们的 document-ready 函数末尾添加以下两行：

```
App.Browser = new App.Routers.Contact;
Backbone.history.start();
```

## Step 4 – 更新 Contacts 视图

更新 `scripts/views/contact.js`，在 `remove()` 方法开头添加以下代码行：

```
App.Browser.navigate('contacts/remove/' + this.model.cid);
```

## Step 5 – 更新 Directory 视图

更新 `scripts/views/directory.js`，在 `addForm()` 方法开头添加以下代码行：

```
App.Browser.navigate('contacts/add');
```

此外，在现有 `addForm()` 方法之后向目录视图文件添加此新方法：

```
addFormHide: function() {
  App.Browser.navigate('contacts');
  this.$('.controls form').hide();
},
```

请务必将 `addFormHide` 添加到此对象 `initialize()` 方法中的 `bindAll()` 调用中：

```
_.bindAll(this, 'render', 'addForm', 'addSubmit', 'addFormHide');
```

让我们也快速重构一下 `addSubmit()` 方法。将包含 `$form.hide()` 的代码行替换为以下代码行：

```
this.addFormHide();
```

执行这些步骤后，重新加载网页。在应用程序上执行一些不同的操作，例如添加新联系人、删除联系人等。注意 URL 在您执行不同操作时如何变化。




现在，点击按钮添加新联系人，在表单显示时，完全刷新页面。或者更好的是，复制 URL，关闭浏览器窗口，然后在另一个新的浏览器窗口中打开该 URL。注意当页面加载时添加联系人表单是如何显示的！虽然我们的应用程序相当简单，但这将允许共享我们的 URL 以及将 URL 加入书签。

### `scripts/router.js`

让我们首先看看 `scripts/router.js`：

```
App.Routers.Contact = Backbone.Router.extend({
```

我们首先扩展内置的 `Backbone.Router` 对象。

```
  routes: {
    'contacts': 'contacts',
    'contacts/add': 'contactsAdd',
    'contacts/remove/:id': 'contactsRemove'
  },
```

这里我们定义了三个不同的路由。当 URL 仅为 `contacts` 时，将执行第一个路由。由于我们使用基于锚点的 URL，URL 将显示为 `index.html#contacts`。如果使用 Web 服务器，URL 将是 `example.com/#contacts`。如果我们同时使用 Web 服务器和 `pushState` URL，URL 将是 `example.com/contacts`，并类似于传统的网页请求。

路由的值是我们对象上要执行的方法的名称（这与 View 对象中的 events 对象非常相似）。

这里有趣的是第三个路由。注意第三个 URL 参数 `id`。这告诉 Backbone 我们期望 URL 的这部分包含某种字符串（基本上是除正斜杠以外的任何字符）。如果你确实想包含斜杠，可以在标识符前加上星号，例如 `*id`。你为这些动态路由中的标识符选择的名称并不重要，因为它们会按照出现的顺序作为参数传递给路由方法。不过，请给它们起一个你容易记住的名字。

```
  contacts: function() {
    App.Directory.render();
    App.Directory.addFormHide();
  },
```

`contacts()` 路由方法（当 URL 匹配 `contacts` 时执行）用于显示联系人列表。它将再次渲染我们的 `Directory`，并为我们执行一种页面刷新。如果添加表单是打开的，它也会将其隐藏。将其视为应用程序的默认状态。如果应用程序执行其他多个功能，我们将使用它将用户带回联系人列表。

```
  contactsAdd: function() {
    App.Directory.addForm();
  },
```

`contactsAdd()` 路由方法（当 URL 匹配 `contacts/add` 时执行）用于显示**添加新联系人**表单。

```
  contactsRemove: function(id) {
    App.Contacts.remove(id);
    App.Directory.addFormHide();
  }
```

`contactsRemove()` 路由方法（当 URL 匹配 `contacts/remove/:id` 时执行）用于对指定联系人执行删除操作。我们第一个参数的名称是 `id`，巧合的是这与动态路由中使用的标识符相同。这两者中的任何一个本可以命名为其他名称，代码仍将按预期执行。此代码按预期执行 `Contact` 模型的 `remove()` 方法，并在添加联系人表单打开时将其隐藏。

### `scripts/main.js`

```
scripts/main.js:
var App = {
  …,
  Routers: {},
  Browser: null
};
```

我们添加到主 `App` 对象中的 `Routers` 对象将包含我们在应用程序中使用的不同路由器的列表。如果你的应用程序变得更加复杂，以这种方式保持不同组件的分离并不是坏主意。

`Browser` 对象将包含我们实例化的 `Contact` 路由器。目前应用程序相当简单，拥有一个全局指针使其使用方便。

### `scripts/views/contact.js`

我们现在来看看 `scripts/views/contact.js`：

```
    App.Browser.navigate('contacts/remove/' + this.model.cid);
```

这行代码在点击删除按钮时执行。它更新 URL 以显示正在删除哪个项目。然而，仅运行此代码不会导致相应的路由更新。如果我们想那样做，我们需要向 `navigate()` 传递第二个参数，如下所示：

```
{trigger: true}
```

我们不需要传递第二个参数的原因是因为我们在下一行已经要运行 Model `destroy` 代码了。然而，如果文档包含如下链接，它将触发路由器删除：

```
<a href="#contacts/remove/c1">Delete First Contact</a>
```

### `scripts/views/directory.js`

我们还将看看 `scripts/views/directory.js`：

```
    App.Browser.navigate('contacts/add');
```

这当然会更改我们添加联系人表单的 URL。如果有人访问我们的应用程序只是为了快速添加新联系人，这个 URL 对他们来说变得非常方便。

```
  addFormHide: function() {
    App.Browser.navigate('contacts');
    this.$('.controls form').hide();
  },
```

这里我们稍微重构了一下代码。现在我们添加了路由器的概念，从其他地方切换添加联系人表单的可见性变得很重要。我们稍后更新 `addSubmit()` 中的调用以使用这个新函数来完成重构。

动态更改 URL 锚点足以允许用户将页面加入书签、与朋友分享页面，并使用浏览器的前进和后退按钮浏览历史记录。然而，如果你想摆脱锚点符号 `#`（这只是看起来更美观），你将需要寻找服务端解决方案来实现这一点。

### Note

实现此功能的方法将因你为项目选择的后端服务器软件而异。但作为一般规则，你将需要执行相同的页面生成脚本，并使用正则表达式跟踪动态 URL 规则。Apache 有 `mod_rewrite`。Nginx 有 HTTP location 指令。
