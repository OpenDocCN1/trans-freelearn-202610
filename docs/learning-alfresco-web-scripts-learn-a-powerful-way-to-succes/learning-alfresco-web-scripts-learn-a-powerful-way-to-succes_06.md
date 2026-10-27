# 第7章：详细了解基于 JavaScript 的 Web 脚本

在本章中，我们将介绍以下主题：

- 你可以使用 JavaScript API 做哪些事情？
- 如何查找 Alfresco 中可用的 JavaScript API？
- 可用于访问 JavaScript API 的根对象
- Alfresco 中 Web 脚本框架提供的其他根对象
- JavaScript 控制器中一些常见功能的代码示例
- 创建你自己的根对象

## 详细了解基于 JavaScript 的 Web 脚本

Alfresco 中的 Web 脚本框架使你能够轻松地使用基于 JavaScript 的 Web 脚本来创建、访问、更新和删除存储在存储库中的内容。当你需要在不关闭生产系统的情况下进行某种热修复时，基于 JavaScript 的 Web 脚本将非常有用。基于 JavaScript 的 Web 脚本的另一个酷炫功能是，你可以将它们部署在存储库中，并使用 Alfresco 用户界面直接在线编辑它们。在开发具有基于 JavaScript 控制器的 Web 脚本时，无需依赖任何特定的编辑器。你可以使用你最喜欢的任何编辑器来创建它们。使用基于 JavaScript 的 Web 脚本的主要优势在于它可以加快 Web 脚本的开发周期。你无需像处理基于 Java 的 Web 脚本那样担心编译它们。在开发基于 JavaScript 的 Web 脚本时，你可以利用 Alfresco 提供的 JavaScript API。Alfresco 提供了广泛的 API，使从基于 JavaScript 控制器的 Web 脚本访问存储库变得简单便捷。

在本章中，我们首先将了解可以使用 JavaScript API 执行哪些操作。我们将学习如何识别 Alfresco 中可用的 JavaScript API。我们将查看 Alfresco 中可用的根对象。这将帮助我们访问可用的 JavaScript API，并在更短的时间内有效地开发 Web 脚本；同时，它还将使我们更容易与 Alfresco 存储库进行交互。我们还将了解 Alfresco 中 Web 脚本框架提供的其他有用根对象。我们将看到一些代码示例，帮助你了解如何在 Web 脚本中使用 Alfresco JavaScript API。最后，我们将逐步了解如何创建自定义 JavaScript 扩展，这是一个非常重要且有用的知识点。

### 了解你可以使用 JavaScript API 做什么

在 Alfresco 中开发 Web 脚本时，Alfresco 提供的存储库 JavaScript API 可以帮助你执行以下列出的许多高级操作：

- 创建一个文件夹
- 创建内容
- 复制一个节点
- 将节点从一个位置移动到另一个位置
- 删除一个节点
- 查找一个节点
- 对存储库执行搜索
- 遍历节点层次结构
- 修改节点上的属性
- 在节点上添加或删除方面（aspect）
- 为节点创建、修改或删除关联
- 获取节点的权限
- 修改节点的权限
- 创建和删除组
- 创建和删除用户

基本上，你可以将 Alfresco JavaScript API 视为从基于 JavaScript 的控制器访问 Alfresco 服务的包装器。Alfresco 使用 Mozilla Rhino JavaScript 引擎来处理 Java，该引擎允许 JavaScript 文件通过脚本中简单的 bean 风格方式访问 Alfresco Java 对象。此外，这使得开发与 JavaScript（ECMA Script）1.6 兼容的文件变得更加简单，从而可以使用 Alfresco JavaScript API 创建、更新、检索和删除 Alfresco 存储库对象。

### Alfresco 中的 JavaScript API

作为开发者，你总是应该对通过代码进行调试感兴趣，以便理解其工作原理，而不是直接拥有 API 列表并简单地使用它。因此，这里我们将了解如何识别 Alfresco 中的 JavaScript API。现在，让我们找出 Alfresco 提供的、在开发 Web 脚本时可以在 JavaScript 控制器中使用的可用 JavaScript API。

### 识别 JavaScript API

在 Alfresco 代码库中，识别任何 JavaScript API 的最简单方法是通过 bean 定义，其中父项指定为 `baseJavaScriptExtension`。bean 条目的工作方式是——任何 JavaScript API 都可以通过为其定义的根对象来访问。在 `extensionName` 属性下定义的属性名称是访问相关 API 的根对象。

让我们讨论 JavaScript API 的 bean 定义条目，以便更好地理解 JavaScript API 是如何定义的。以下是 Alfresco 中一个此类 API 的代码片段，它将让你清楚地了解 JavaScript API 如何使用，以及如何在控制器中使用它：

```
<bean id="actionsScript" parent="baseJavaScriptExtension" class="org.alfresco.repo.jscript.Actions">
    <property name="extensionName">
        <value>actions</value>
</property>
<property name="serviceRegistry">
    <ref bean="ServiceRegistry"/>
</property>
</bean>
```

以下是我们可以从这段代码片段中理解的一些要点：

- 它的父项定义为 `baseJavaScriptExtension`。
- 所有可用于 JavaScript 控制器的方法都定义在 Java 类 `org.alfresco.repo.jscript.Actions` 中。
- 任何 Alfresco 服务的依赖都可以注入到 bean 定义中，这些依赖可以在 Java 类中用于对存储库执行操作。
- JavaScript 控制器将使用 bean 定义中 `extensionName` 属性下提供的值来访问 Java 类公开的方法。`extensionName` 属性下提供的这个值被称为 JavaScript 控制器中可用于访问 JavaScript API 的根对象。

现在，有了这些理解，你就掌握了在 Web 脚本中使用 JavaScript API 的关键。你刚刚解锁了如何从 JavaScript 控制器访问 Java 对象。

### 用于访问 JavaScript API 的根对象

既然你了解了如何识别 Alfresco 代码库中的 JavaScript API，让我们看看从 JavaScript 控制器访问 JavaScript API 可用的所有根对象。在你的 Alfresco 安装目录中，你可以在 `tomcat\webapps\alfresco\WEB-INF\classes\alfresco` 下的 `script-services-context.xml` 中找到常用的 JavaScript API 定义。如果你使用的是 Alfresco Community 5，`script-services-context.xml` 可以位于 `alfresco` 包下的 `alfresco-repository-*.jar` 内。在下表中，条目首先列出根对象的名称，然后是包含 API 方法实现的类的完全限定名称，以及根对象的简要描述：

| 根对象 | 类名 | 描述 |
|---|---|---|
| logger | org.alfresco.repo.jscript.ScriptLogger | 可用于记录所需详细信息，这些信息可在调试问题时使用 |
| utils | org.alfresco.repo.jscript.ScriptUtils | 提供一些通用的有用工具方法 |
| test | org.alfresco.repo.jscript.ScriptTestUtils | 此工具类提供一些测试方法 |
| actions | org.alfresco.repo.jscript.Actions | 提供一种在节点上执行操作（actions）的方式 |
| imap | org.alfresco.repo.jscript.Imap | 提供对 imap 方法的访问，例如获取用户的 imap 主页 |
| search | org.alfresco.repo.jscript.Search | 提供一种针对 Alfresco 存储库搜索数据的方式 |
| classification | org.alfresco.repo.jscript.Classification | 提供创建和访问类别的方法 |
| people | org.alfresco.repo.jscript.People | 提供对人员和组的访问，并提供一种操作它们的方式 |
| session | org.alfresco.repo.jscript.Session | 提供当前用户的认证票据 |
| avm | org.alfresco.repo.jscript.AVM | 提供访问 AVM 对象的辅助方法 |
| crossRepoCopy | org.alfresco.repo.jscript.CrossRepositoryCopy | 提供对跨存储库复制的支持 |
| workflow | org.alfresco.repo.workflow.jscript.WorkflowManager | 允许你在 Alfresco 中创建工作流并与工作流交互 |
| presence | org.alfresco.repo.jscript.Presence | 提供一种查询人员当前在线状态的方式 |




