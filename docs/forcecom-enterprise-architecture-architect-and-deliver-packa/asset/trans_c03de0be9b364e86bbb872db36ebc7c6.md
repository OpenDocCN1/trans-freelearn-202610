

<h2>成功！您的第一个 Lightning 应用</h2>
</div>
</div>
</aura:application>

> 请注意前面代码中 role 属性和 slds-assistive-text 类的使用。虽然这些在技术上并非必需，但它们旨在确保与需要屏幕阅读协助的用户兼容。Lightning 包含并鼓励对 HTML5 标准中关于 **无障碍访问** 的支持。我们将在本章后面进一步讨论这一点。

如果您刷新浏览器，现在将看到以下屏幕截图中所示的内容。前面的 HTML 代码基于 LDS 网站提供的代码：

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_313_0.png)

Lightning Design System (https://www.lightningdesignsystem.com) 网站包含大量精彩的示例 HTML 标记，供您探索、设计和静态测试您希望 UI 呈现的效果。使用类似前面的简单 Lightning 应用程序，您可以继续复制粘贴示例来尝试不同的组合。

> 但是，请牢记作为开发者要保持高效。您真正需要的是一个组件库，其中的组件封装了前面的 HTML 代码，以便您可以定义一次并复用，而不是反复复制粘贴 HTML。如我们将在后面看到的，您可以构建自己的组件，并在其中利用 SLDS。幸运的是，对于更简单、更基础的组件需求，平台提供了越来越多已经使用 SLDS 样式设计的基本组件，称为 **Lightning Base Components**。我们将在本章后面探讨这些组件 (https://developer.salesforce.com/blogs/developer-relations/2017/01/base-lightning-components.html)。

## 构建您的第一个组件

最后，让我们创建一个 Lightning 组件来封装我们的成功消息，该组件例如可以作为更通用信息消息的可重用组件的基础：

- 1. 在 Force.com IDE 中，导航到 File | New | Lightning Bundle。
- 2. 在创建新的 Lightning 组件对话框中，从 Type 字段中选择 Lightning Component。
- 3. 为您的组件命名为 mycomponent，然后单击 Finish。
- 4. 将 myapp.app 文件中 aura:application 标签内包含的标记复制到新 mycomponent.cmp 文件中如下所示的新 aura:component 标签，同时修改消息：

```
<aura:component>
    <div class="slds-notify_container">
        <div class="slds-notify slds-notify--alert
            slds-theme--success
            slds-theme--alert-texture" role="alert">
            <span class="slds-assistive-text">Success</span>
            <h2>Success your first Lightning Component</h2>
        </div>
    </div>
</aura:component>
```

- 5. 最后修改 myapp.app 文件以使用您的新组件：

```
<aura:application extends="force:slds">
    <c:mycomponent />
</aura:application>
```

刷新浏览器，您将看到以下内容：

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_314_0.png)

前面的代码是 Lightning 开发的基本介绍，旨在帮助您了解 Lightning 的高层架构及其如何适应互联网使用的标准 HTML 页面模型，同时还介绍了被称为 Salesforce Lightning Design System 的样式和设计框架。

## Lightning 与其他 UI 框架有何不同？

如果您之前使用过其他 UI 框架（例如 Salesforce 自身的 Visualforce 或 Java Server Pages (JSP)、ASP.NET 等），那么到目前为止，这些内容可能感觉相当熟悉。我们已经定义了一些标记，使用了 CSS，并了解了如何封装可重用的 HTML 部分。当用户导航到 URL 时，Lightning 平台服务器端代码会提供适当的 HTML 来启动指定的应用程序并呈现引用的组件。如果您是 Visualforce 开发者，可能会觉得这与使用 Visualforce 页面和 Visualforce 组件并没有太大区别。

虽然与其他框架还有其他相似之处，例如使用表达式，但与这些技术最大的区别在于页面本身的生命周期。服务器端架构通常要求在用户执行操作时刷新整个页面。页面上的信息（即页面状态）会在用户每次执行操作时在客户端和服务器之间来回传输。服务器端控制器代码控制验证、数据加载和 UI 的条件显示。

越来越多的用户界面设计生成的 HTML 页面代表用户打开并保持打开的单一应用程序或工作区。用户在其中访问不同的模块并执行各种操作。这通常称为单页应用程序 (SPA) 架构。传统的服务器端页面刷新模型不够灵敏，无法满足这些需求，例如仅需要更新页面一部分的情况。即使部署了传统的异步 JavaScript 和 XML (AJAX) 方法，通常也需要将整个页面状态传回服务器端控制器。服务器端 UI 框架也不适合开发在临时离线场景中容忍缺乏连接性的用户体验。

相比之下，Lightning 应用程序在浏览器中长期存在，并依赖 JavaScript 代码来操作页面的 DOM 以加载新内容和功能。用户正在执行的操作的状态存储在浏览器中，只有在服务器需要检索和更新数据库时才会传输。

## Lightning 架构

在本节中，我们将讨论 Lightning 架构的关键层，这将使您在深入学习时拥有更好的理解框架。

最初让我感到惊讶的一个关键方面是需要使用 JavaScript 编写 **客户端控制器**。如果您是 Visualforce 开发者，起初这可能特别令人困惑，但这是成为 Lightning 开发者的重要组成部分，反映了其客户端架构。如前一章所见，Apex 服务器端控制器仍然发挥作用，但主要用于访问后端 Apex 服务和选择器。

一般来说，Lightning 开发更加组件化。就 UI 的设计方式和代码的组织方式而言，两者更加一致，使代码更易于维护和导航。这也更加强调 UI 层内的关注点分离。

### 容器

如前一节所示，我们使用独立的 Lightning 应用程序创建了一个名为 `myapp` 的基本容器。它包含我们 `mycomponent` 组件的单个实例。虽然看起来可能不多，但 `<aura:application>` 为其组件和您作为开发者提供了许多服务。

- • 它构建了页面 `<HTML>` 标签并加载了适当的 JavaScript、CSS 资源，例如 SLDS 和 Lightning JavaScript 运行时，这使得您的组件可以通过客户端控制器代码对用户交互做出反应。

> `extends` 属性允许从其他基于 `<aura:application>` 的应用程序继承。前面的示例扩展了平台的 `force:slds` 应用程序，以注入 SLDS 的 CSS。

- • 它加载了其主体中的组件，即 `mycomponent` 组件，同时页面中存在的任何 HTML 标记也会被注入到页面中。每个组件在容器中启动自己的生命周期。就像 Apex 中的对象一样，它会被构建、初始化、跟踪，并可以被销毁。

> 组件管理是一项重要的服务，因为它有助于避免开发富客户端应用程序时的常见陷阱，即资源管理。每个 HTML 标记及其关联数据都会占用浏览器内存，如果不妥善管理，在页面的整个生命周期内很容易攀升并引起问题。

- URL 参数会自动映射到 aura:application 主体中指定的 aura:attribute 组件。例如：
- /c/myapp.app?myparam=myvalue

```
<aura:application extends="force:slds">
<aura:attribute name="myparam" type="String"/>
<h2>The value of myparam is '{!v.myparam}'</h2>
</aura:application>
```

在浏览器中显示以下文本：

myparam 的值为 'myvalue'

- URL 为中心的导航是用户体验的一个重要方面，当用户将 Web 应用程序中的记录或某些页面加入书签或共享链接时，这是一种预期行为。由于应用程序设计为长期存在，且更改 URL 会导致整个页面重新加载，因此需要一种替代方案。

修改 URL 的书签部分不会重新加载页面。因此，Lightning 应用程序利用这一点为用户存储的链接添加更多上下文，并在应用程序自身的导航中使用它。

在浏览 Lightning Experience 时，您可以亲眼看到这一点。该框架提供 aura:locationChange 事件，您的客户端逻辑可以监听该事件以确定何时发生更改。

下面是使用的 URL 约定的简单示例：/c/myapp.app#showraceresults-2016

您可以在 Lightning 组件开发指南中阅读有关 aura:application 属性的更多内容。这些服务是否全部相关取决于您使用的容器。但是，即使您只使用 Salesforce 容器，了解事物的工作原理也是好的。

