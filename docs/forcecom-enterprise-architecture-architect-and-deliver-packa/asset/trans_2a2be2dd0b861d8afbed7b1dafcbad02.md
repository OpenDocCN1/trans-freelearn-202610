

执行以下步骤以创建新仓库并将其克隆，从而在计算机上创建本地仓库并用初始内容填充它：

- 1. 从 Web 浏览器登录到您的 GitHub 帐户，并从用户旁边的下拉菜单中选择创建新仓库的选项。同时选择创建默认 README 文件的选项。

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_465_0.png)

> 如果您计划在本章后面尝试持续集成步骤，那么就当前配置而言，最简单的选项是创建公共仓库。关于如何配置 Jenkins 与 GitHub 身份验证的深入演示超出了本章的范围。

- 2. 从 Web 浏览器中新仓库的项目页面，找到页面右下侧边栏底部的“克隆或下载”按钮。点击其上的“在桌面打开”按钮以打开 GitHub 桌面客户端并开始克隆仓库，或者手动打开 GitHub 桌面客户端并选择“克隆到计算机”选项。
- 3. 前述克隆过程应已提示您在硬盘上选择存储本地仓库的位置。请记下此位置，因为我们将在后续步骤中填充此位置。
- 4. 在仓库文件夹的根目录下创建 /lib 文件夹。
- 5. 将本章示例代码文件夹 /lib 中的以下文件复制到前一步创建的新 /lib 文件夹中：
  - 复制 ant-salesforce.jar 文件
  - 复制 ant-salesforce.xml 文件
  - 复制 ant-contrib-1.0b3.jar 文件
- 6. 在仓库文件夹的根目录下创建 /src 文件夹。
- 7. 将您在之前章节中使用的 build.xml 文件复制到仓库文件夹的根目录中。

在本章中，我们将更新 Ant build.xml 脚本文件，以便在需要时将应用程序源代码部署到新的开发者版 org。在此之前，我们将使用 salesforce-ant.jar 文件提供的 Ant sf:retrieve 任务，以组件源文件的形式检索迄今您一直在开发的 FormulaForce 包的最新内容，我们将把这些内容迁移到源代码控制中。

> 请注意，我们执行此步骤是由于本书所采用的方法，即从打包 org 开始进行开发。如果您不是从这一点开始，则可以按手动描述创建 /src 文件夹和 package.xml 文件。话虽如此，您可能希望在打包 org 内进行一些初步探索，以建立带有某些初始对象的包和命名空间。

以下 build.xml 目标检索打包 org 中定义的包的内容并填充 /src 文件夹。请确保您的包名称为 FormulaForce。如果不是，请按如下方式更新 packageNames 属性：

```
<target name="retrieve.package">
    <sf:retrieve
        username="${sf.username}"
        password="${sf.password}"
        retrieveTarget="${basedir}/src"
        packageNames="FormulaForce"/>
</target>
```

> 您可以在登录打包 org 时，看到 Salesforce 页面标头右上角黑色横幅中显示的包名称。

使用您自己的用户名、密码和令牌执行以下命令（全部在一行命令行上）以执行检索目标。首先确保已将位置更改为本地仓库的根文件夹：

```
ant retrieve.package
  -Dsf.username=yourusername
  -Dsf.password=yourpasswordyourtoken
```

您应该在本地仓库文件夹中看到类似下面的内容：

| 文件夹 | 文件夹 | 文档 |
|---|---|---|
| src | applications | Application.cls |
| bin | classes | Application.cls-meta.xml |
| .git | labels | Cars.cls |
| | layouts | Cars.cls-meta.xml |
| | objects | CarsSelector.cls |
| | pages | CarsSelector.cls-meta.xml |
| | permissionsets | ComplianceController.cls |
| | quickActions | Compliance...-ls-meta.xml |
| | tabs | ComplianceService.cls |
| | triggers | Compliance...-ls-meta.xml |
| Documents | Documents | ComplianceServiceTest.cls |
| build.xml | package.xml | ContestantController.cls |
| README.md | | ContestantC...-ls-meta.xml |

> 您可能会在检索的代码和元数据文件中找到对包命名空间的引用。应将其删除，否则文件可能无法在非托管开发者 org 中部署或执行。您可以使用文件搜索实用程序查找此类引用。例如，打开 /layouts/Contestant__c-Contestant Layout.layout 文件并搜索 QuickActionName，然后删除命名空间引用。

在准备将此内容提交到本地仓库（在您可以上传或推送到 GitHub 远程仓库之前必需）之前，打开由检索操作创建的 package.xml 文件并将其转换为更通用的形式。前一过程创建的文件包含包中每个组件的显式列表。

这意味着，如果开发人员在 /classes、/pages 和 /objects 等子文件夹中添加新组件文件，则除非先更新 package.xml 文件，否则这些文件不会随后自动部署。

为避免开发人员需要记住执行此操作，可以将 package.xml 文件更新为使用通配符。例如，打开 package.xml 文件并用通配符字符替换显式引用，如以下示例所示：

```
<types>
    <members>*</members>
    <name>ApexClass</name>
</types>
<types>
    <members>*</members>
    <name>ApexPage</name>
</types>
```

```
<types>
<members>*</members>
<name>ApexTrigger</name>
</types>
```

还要从 package.xml 文件中删除 fullName、apiAccessLevel 和 namespacePrefix 标记。对 package.xml 文件中的大多数组件类型重复上述方法，而不仅仅是前面示例中所示的那些，但 **子组件** 除外。

如果将前面的示例与之前创建的 package.xml 文件进行比较，您会注意到原始 package.xml 文件中的某些组件是 CustomObject 组件的子组件，例如 FieldSet、WebLink、CustomField 和 ListView。此外，不需要多个 CustomLabel 条目。因此，它们根本不需要通配符条目，因为它们将通过其父文件部署，例如，通过包含字段的 CustomObject 组件的 .object 文件。

请注意，FormulaForce 包中实际包含的组件列表仍位于打包 org 中。package.xml 文件在源代码控制中的作用和目的不同，并且更针对开发者 org 部署进行了优化。

最后，使用 GitHub UI 工具将前面的更改提交并同步到本地仓库，并上传或推送（使用正确的 Git 术语）。如果您运行该工具，应该会看到它已经在报告硬盘上的仓库文件夹中检测到新文件，如以下 Mac 版工具屏幕截图所示。只需输入摘要和说明，然后单击 **提交并同步**。

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_468_0.png)

请注意，这将在一个操作中同时提交到本地仓库并推送到远程仓库。通常，开发人员会处理一组更改，并在决定将所有更改作为一个操作推送到远程仓库之前，本地增量式提交。

> 如果您不熟悉 Git，这可能有点令人困惑，因为您已经在仓库文件夹中实际复制或更新了文件。但是，为了将更改推送到存储在 GitHub 服务器上的远程 Git 仓库，您必须先更新 .git 文件夹中保存的仓库状态的本地副本，.git 文件夹是仓库文件夹根目录下的隐藏文件夹。您绝不应编辑或查看此文件夹，因为它完全由 Git 管理。

## 用于清理和构建应用程序的 Ant 构建脚本

现在，我们已为应用程序创建了初始源代码控制内容，让我们将其部署到开发者 org。在此之前，我们将更新 build.xml 文件以清理 org（以防它已用于开发应用程序的早期版本）。

Salesforce 不提供用于完全清理开发者 org 中先前部署或创建的任何组件的 Ant 任务。但是，它确实通过其 Force.com 迁移工具包提供了一种方式来部署所谓的 destructivePackage.xml 文件，该文件包含要删除的组件列表。不幸的是，当 org 中存在组件依赖项时，这需要一些帮助。FinancialForce.com 已通过 undeploy.xml Ant 脚本提供了一个开源解决方案，该脚本封装了此过程并解决了依赖项不足的问题。

从 FinancialForce.com 存储库（https://github.com/financialforcedev/df12-deployment-tools）的 /lib 文件夹下载以下文件，并将它们与 ant-salesforce.jar 文件一起放入之前创建的 /lib 文件夹中：

- exec_anon.xml
- undeploy.xml
- xmltask.jar
- ml-ant-http-1.1.3.jar
- ant-contrib-1.0b3.jar

