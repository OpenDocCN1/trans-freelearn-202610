
## 创建 Java 类

要创建新的 Java 类，请在 Package Explorer 视图中右键单击项目并导航到 **New** | **Class**。您将看到 **New Java Class** 窗口，在其中输入有关类的信息。您可以更改类的超类，并添加其实现的接口，以及为从接口和抽象超类继承的抽象方法添加存根、从超类添加构造函数，并添加 main 方法。

要在包内创建类，只需在相应字段中输入其名称，或单击旁边的 **Browse** 按钮并选择包。如果您输入的包名不存在，Eclipse 会为您创建它。新的包也可以通过在 Package Explorer 中右键单击项目并导航到 **New** | **Package** 来创建。在 Project Explorer 中右键单击包而不是项目并导航到 **New** | **Class** 将导致类在该包内创建。

## 创建工作集

工作集提供了一种将工作区中的项目组织成子集的方式。当工作区中的项目太多时，很难在 **Package Explorer** 视图中找到所需的项目。例如，您当前未处理的项目可以保存在单独的工作集中。它们不会妨碍您当前的工作，但在需要时仍可使用。

要创建新的工作集，请打开 Package Explorer 的视图菜单（视图右上角的白色三角形），并选择 **Select Working Set**。单击 **New** 并选择工作集将包含的项目类型（本例中为 **Java**）。在下一页中，输入工作集的名称，并选择其将包含的项目。工作集创建后，选择 **Selected Working Sets** 选项，并标记您的工作集。单击 **OK**，Package Explorer 将仅显示您刚刚创建的工作集中的项目。

工作集创建后，它们会列在 Package Explorer 的视图菜单中。选择其中一个将使其成为 Package Explorer 中唯一可见的工作集。要同时查看多个工作集，请选择 **Select Working Set** 选项并标记要显示的项目。要再次查看整个工作区，请在视图菜单中选择 **Deselect Workspace**。您还可以通过将工作集选为 **Package Explorer** 视图的顶级元素来查看所有工作集及其嵌套项目。为此，请在视图菜单中导航到 **Top Level Elements** | **Working Sets**。

虽然在选择工作集时看不到属于其他工作集的项目，但它们仍加载在工作区中，因此会占用机器资源。为避免浪费这些资源，您可以通过右键单击它们并选择 Close Project 来关闭不相关的项目。您可以使用 Ctrl + A 键盘快捷键选择工作集中的所有项目。

如果您有大量项目，但从不同时处理所有项目（个人/业务项目、不同客户的项目等），您还可以为每个项目集创建特定的工作区。要创建新的工作区，请在菜单中导航到 File | Switch Workspace | Other，输入新工作区的[content]
## 创建 Java 类

要创建新的 Java 类，请在 Package Explorer 视图中右键单击项目并导航到 **New** | **Class**。您将看到 **New Java Class** 窗口，在其中输入有关类的信息。您可以更改类的超类，并添加其实现的接口，以及为从接口和抽象超类继承的抽象方法添加存根、从超类添加构造函数，并添加 main 方法。

要在包内创建类，只需在相应字段中输入其名称，或单击旁边的 **Browse** 按钮并选择包。如果您输入的包名不存在，Eclipse 会为您创建它。新的包也可以通过在 Package Explorer 中右键单击项目并导航到 **New** | **Package** 来创建。在 Project Explorer 中右键单击包而不是项目并导航到 **New** | **Class** 将导致类在该包内创建。

## 创建工作集

工作集提供了一种将工作区中的项目组织成子集的方式。当工作区中的项目太多时，很难在 **Package Explorer** 视图中找到所需的项目。例如，您当前未处理的项目可以保存在单独的工作集中。它们不会妨碍您当前的工作，但在需要时仍可使用。

要创建新的工作集，请打开 Package Explorer 的视图菜单（视图右上角的白色三角形），并选择 **Select Working Set**。单击 **New** 并选择工作集将包含的项目类型（本例中为 **Java**）。在下一页中，输入工作集的名称，并选择其将包含的项目。工作集创建后，选择 **Selected Working Sets** 选项，并标记您的工作集。单击 **OK**，Package Explorer 将仅显示您刚刚创建的工作集中的项目。

工作集创建后，它们会列在 Package Explorer 的视图菜单中。选择其中一个将使其成为 Package Explorer 中唯一可见的工作集。要同时查看多个工作集，请选择 **Select Working Set** 选项并标记要显示的项目。要再次查看整个工作区，请在视图菜单中选择 **Deselect Workspace**。您还可以通过将工作集选为 **Package Explorer** 视图的顶级元素来查看所有工作集及其嵌套项目。为此，请在视图菜单中导航到 **Top Level Elements** | **Working Sets**。

虽然在选择工作集时看不到属于其他工作集的项目，但它们仍加载在工作区中，因此会占用机器资源。为避免浪费这些资源，您可以通过右键单击它们并选择 Close Project 来关闭不相关的项目。您可以使用 Ctrl + A 键盘快捷键选择工作集中的所有项目。

如果您有大量项目，但从不同时处理所有项目（个人/业务项目、不同客户的项目等），您还可以为每个项目集创建特定的工作区。要创建新的工作区，请在菜单中导航到 File | Switch Workspace | Other，输入新工作区的文件夹名称，然后单击 OK。您可以在 Copy Settings 部分选择是否复制当前工作区的布局和工作集。

## 导入 Java 项目

如果您要处理现有项目，则可以根据项目源代码的获取方式以多种不同的方式将其导入 Eclipse。要打开导入向导，请导航到 File | Import。让我们介绍一下 General 类别下的选项：

- Archive file: 如果您要处理的项目已存在于工作区中，并且只想导入包含新资源的归档文件，请选择此选项。导入向导将列出归档文件中的所有资源，以便您可以选择要导入的资源。要选择资源将导入到哪个项目，请单击 Browse 按钮。您还可以选择资源将被包含到的文件夹。完成后单击 Finish。导入的资源将被解压缩并复制到项目的[content]
## 创建 Java 类

要创建新的 Java 类，请在 Package Explorer 视图中右键单击项目并导航到 **New** | **Class**。您将看到 **New Java Class** 窗口，在其中输入有关类的信息。您可以更改类的超类，并添加其实现的接口，以及为从接口和抽象超类继承的抽象方法添加存根、从超类添加构造函数，并添加 main 方法。

要在包内创建类，只需在相应字段中输入其名称，或单击旁边的 **Browse** 按钮并选择包。如果您输入的包名不存在，Eclipse 会为您创建它。新的包也可以通过在 Package Explorer 中右键单击项目并导航到 **New** | **Package** 来创建。在 Project Explorer 中右键单击包而不是项目并导航到 **New** | **Class** 将导致类在该包内创建。

## 创建工作集

工作集提供了一种将工作区中的项目组织成子集的方式。当工作区中的项目太多时，很难在 **Package Explorer** 视图中找到所需的项目。例如，您当前未处理的项目可以保存在单独的工作集中。它们不会妨碍您当前的工作，但在需要时仍可使用。

要创建新的工作集，请打开 Package Explorer 的视图菜单（视图右上角的白色三角形），并选择 **Select Working Set**。单击 **New** 并选择工作集将包含的项目类型（本例中为 **Java**）。在下一页中，输入工作集的名称，并选择其将包含的项目。工作集创建后，选择 **Selected Working Sets** 选项，并标记您的工作集。单击 **OK**，Package Explorer 将仅显示您刚刚创建的工作集中的项目。

工作集创建后，它们会列在 Package Explorer 的视图菜单中。选择其中一个将使其成为 Package Explorer 中唯一可见的工作集。要同时查看多个工作集，请选择 **Select Working Set** 选项并标记要显示的项目。要再次查看整个工作区，请在视图菜单中选择 **Deselect Workspace**。您还可以通过将工作集选为 **Package Explorer** 视图的顶级元素来查看所有工作集及其嵌套项目。为此，请在视图菜单中导航到 **Top Level Elements** | **Working Sets**。

虽然在选择工作集时看不到属于其他工作集的项目，但它们仍加载在工作区中，因此会占用机器资源。为避免浪费这些资源，您可以通过右键单击它们并选择 Close Project 来关闭不相关的项目。您可以使用 Ctrl + A 键盘快捷键选择工作集中的所有项目。

如果您有大量项目，但从不同时处理所有项目（个人/业务项目、不同客户的项目等），您还可以为每个项目集创建特定的工作区。要创建新的工作区，请在菜单中导航到 File | Switch Workspace | Other，输入新工作区的文件夹名称，然后单击 OK。您可以在 Copy Settings 部分选择是否复制当前工作区的布局和工作集。

## 导入 Java 项目

如果您要处理现有项目，则可以根据项目源代码的获取方式以多种不同的方式将其导入 Eclipse。要打开导入向导，请导航到 File | Import。让我们介绍一下 General 类别下的选项：

- Archive file: 如果您要处理的项目已存在于工作区中，并且只想向其导入包含新资源的归档文件，请选择此选项。导入向导将列出归档文件中的所有资源，以便您可以选择要导入的资源。要选择资源将导入到哪个项目，请单击 Browse 按钮。您还可以选择资源将被包含到的文件夹。完成后单击 Finish。导入的资源将被解压缩并复制到项目的[content]
## 创建 Java 类

要创建新的 Java 类，请在 Package Explorer 视图中右键单击项目并导航到 **New** | **Class**。您将看到 **New Java Class** 窗口，在其中输入有关类的信息。您可以更改类的超类，并添加其实现的接口，以及为从接口和抽象超类继承的抽象方法添加存根、从超类添加构造函数，并添加 main 方法。

要在包内创建类，只需在相应字段中输入其名称，或单击旁边的 **Browse** 按钮并选择包。如果您输入的包名不存在，Eclipse 会为您创建它。新的包也可以通过在 Package Explorer 中右键单击项目并导航到 **New** | **Package** 来创建。在 Project Explorer 中右键单击包而不是项目并导航到 **New** | **Class** 将导致类在该包内创建。

## 创建工作集

工作集提供了一种将工作区中的项目组织成子集的方式。当工作区中的项目太多时，很难在 **Package Explorer** 视图中找到所需的项目。例如，您当前未处理的项目可以保存在单独的工作集中。它们不会妨碍您当前的工作，但在需要时仍可使用。

要创建新的工作集，请打开 Package Explorer 的视图菜单（视图右上角的白色三角形），并选择 **Select Working Set**。单击 **New** 并选择工作集将包含的项目类型（本例中为 **Java**）。在下一页中，输入工作集的名称，并选择其将包含的项目。工作集创建后，选择 **Selected Working Sets** 选项，并标记您的工作集。单击 **OK**，Package Explorer 将仅显示您刚刚创建的工作集中的项目。

工作集创建后，它们会列在 Package Explorer 的视图菜单中。选择其中一个将使其成为 Package Explorer 中唯一可见的工作集。要同时查看多个工作集，请选择 **Select Working Set** 选项并标记要显示的项目。要再次查看整个工作区，请在视图菜单中选择 **Deselect Workspace**。您还可以通过将工作集选为 **Package Explorer** 视图的顶级元素来查看所有工作集及其嵌套项目。为此，请在视图菜单中导航到 **Top Level Elements** | **Working Sets**。

虽然在选择工作集时看不到属于其他工作集的项目，但它们仍加载在工作区中，因此会占用机器资源。为避免浪费这些资源，您可以通过右键单击它们并选择 Close Project 来关闭不相关的项目。您可以使用 Ctrl + A 键盘快捷键选择工作集中的所有项目。

如果您有大量项目，但从不同时处理所有项目（个人/业务项目、不同客户的项目等），您还可以为每个项目集创建特定的工作区。要创建新的工作区，请在菜单中导航到 File | Switch Workspace | Other，输入新工作区的文件夹名称，然后单击 OK。您可以在 Copy Settings 部分选择是否复制当前工作区的布局和工作集。

## 导入 Java 项目

如果您要处理现有项目，则可以根据项目源代码的获取方式以多种不同的方式将其导入 Eclipse。要打开导入向导，请导航到 File | Import。让我们介绍一下 General 类别下的选项：

- Archive file: 如果您要处理的项目已存在于工作区中，并且只想向其导入包含新资源的归档文件，请选择此选项。导入向导将列出归档文件中的所有资源，以便您可以选择要导入的资源。要选择资源将导入到哪个项目，请单击 Browse 按钮。您还可以选择资源将被包含到的文件夹。完成后单击 Finish。导入的资源将被解压缩并复制到项目的[content]
## 创建 Java 类

要创建新的 Java 类，请在 Package Explorer 视图中右键单击项目并导航到 **New** | **Class**。您将看到 **New Java Class** 窗口，在其中输入有关类的信息。您可以更改类的超类，并添加其实现的接口，以及为从接口和抽象超类继承的抽象方法添加存根、从超类添加构造函数，并添加 main 方法。

要在包内创建类，只需在相应字段中输入其名称，或单击旁边的 **Browse** 按钮并选择包。如果您输入的包名不存在，Eclipse 会为您创建它。新的包也可以通过在 Package Explorer 中右键单击项目并导航到 **New** | **Package** 来创建。在 Project Explorer 中右键单击包而不是项目并导航到 **New** | **Class** 将导致类在该包内创建。

## 创建工作集

工作集提供了一种将工作区中的项目组织成子集的方式。当工作区中的项目太多时，很难在 **Package Explorer** 视图中找到所需的项目。例如，您当前未处理的项目可以保存在单独的工作集中。它们不会妨碍您当前的工作，但在需要时仍可使用。

要创建新的工作集，请打开 Package Explorer 的视图菜单（视图右上角的白色三角形），并选择 **Select Working Set**。单击 **New** 并选择工作集将包含的项目类型（本例中为 **Java**）。在下一页中，输入工作集的名称，并选择其将包含的项目。工作集创建后，选择 **Selected Working Sets** 选项，并标记您的工作集。单击 **OK**，Package Explorer 将仅显示您刚刚创建的工作集中的项目。

工作集创建后，它们会列在 Package Explorer 的视图菜单中。选择其中一个将使其成为 Package Explorer 中唯一可见的工作集。要同时查看多个工作集，请选择 **Select Working Set** 选项并标记要显示的项目。要再次查看整个工作区，请在视图菜单中选择 **Deselect Workspace**。您还可以通过将工作集选为 **Package Explorer** 视图的顶级元素来查看所有工作集及其嵌套项目。为此，请在视图菜单中导航到 **Top Level Elements** | **Working Sets**。

虽然在选择工作集时看不到属于其他工作集的项目，但它们仍加载在工作区中，因此会占用机器资源。为避免浪费这些资源，您可以通过右键单击它们并选择 Close Project 来关闭不相关的项目。您可以使用 Ctrl + A 键盘快捷键选择工作集中的所有项目。

如果您有大量项目，但从不同时处理所有项目（个人/业务项目、不同客户的项目等），您还可以为每个项目集创建特定的工作区。要创建新的工作区，请在菜单中导航到 File | Switch Workspace | Other，输入新工作区的文件夹名称，然后单击 OK。您可以在 Copy Settings 部分选择是否复制当前工作区的布局和工作集。

## 导入 Java 项目

如果您要处理现有项目，则可以根据项目源代码的获取方式以多种不同的方式将其导入 Eclipse。要打开导入向导，请导航到 File | Import。让我们介绍一下 General 类别下的选项：

- Archive file: 如果您要处理的项目已存在于工作区中，并且只想向其导入包含新资源的归档文件，请选择此选项。导入向导将列出归档文件中的所有资源，以便您可以选择要导入的资源。要选择资源将导入到哪个项目，请单击 Browse 按钮。您还可以选择资源将被包含到的文件夹。完成后单击 Finish。导入的资源将被解压缩并复制到项目的[content]
## 创建 Java 类

要创建新的 Java 类，请在 Package Explorer 视图中右键单击项目并导航到 **New** | **Class**。您将看到 **New Java Class** 窗口，在其中输入有关类的信息。您可以更改类的超类，并添加其实现的接口，以及为从接口和抽象超类继承的抽象方法添加存根、从超类添加构造函数，并添加 main 方法。

要在包内创建类，只需在相应字段中输入其名称，或单击旁边的 **Browse** 按钮并选择包。如果您输入的包名不存在，Eclipse 会为您创建它。新的包也可以通过在 Package Explorer 中右键单击项目并导航到 **New** | **Package** 来创建。在 Project Explorer 中右键单击包而不是项目并导航到 **New** | **Class** 将导致类在该包内创建。

## 创建工作集

工作集提供了一种将工作区中的项目组织成子集的方式。当工作区中的项目太多时，很难在 **Package Explorer** 视图中找到所需的项目。例如，您当前未处理的项目可以保存在单独的工作集中。它们不会妨碍您当前的工作，但在需要时仍可使用。

要创建新的工作集，请打开 Package Explorer 的视图菜单（视图右上角的白色三角形），并选择 **Select Working Set**。单击 **New** 并选择工作集将包含的项目类型（本例中为 **Java**）。在下一页中，输入工作集的名称，并选择其将包含的项目。工作集创建后，选择 **Selected Working Sets** 选项，并标记您的工作集。单击 **OK**，Package Explorer 将仅显示您刚刚创建的工作集中的项目。

工作集创建后，它们会列在 Package Explorer 的视图菜单中。选择其中一个将使其成为 Package Explorer 中唯一可见的工作集。要同时查看多个工作集，请选择 **Select Working Set** 选项并标记要显示的项目。要再次查看整个工作区，请在视图菜单中选择 **Deselect Workspace**。您还可以通过将工作集选为 **Package Explorer** 视图的顶级元素来查看所有工作集及其嵌套项目。为此，请在视图菜单中导航到 **Top Level Elements** | **Working Sets**。

虽然在选择工作集时看不到属于其他工作集的项目，但它们仍加载在工作区中，因此会占用机器资源。为避免浪费这些资源，您可以通过右键单击它们并选择 Close Project 来关闭不相关的项目。您可以使用 Ctrl + A 键盘快捷键选择工作集中的所有项目。

如果您有大量项目，但从不同时处理所有项目（个人/业务项目、不同客户的项目等），您还可以为每个项目集创建特定的工作区。要创建新的工作区，请在菜单中导航到 File | Switch Workspace | Other，输入新工作区的文件夹名称，然后单击 OK。您可以在 Copy Settings 部分选择是否复制当前工作区的布局和工作集。

## 导入 Java 项目

如果您要处理现有项目，则可以根据项目源代码的获取方式以多种不同的方式将其导入 Eclipse。要打开导入向导，请导航到 File | Import。让我们介绍一下 General 类别下的选项：

- Archive file: 如果您要处理的项目已存在于工作区中，并且只想向其导入包含新资源的归档文件，请选择此选项。导入向导将列出归档文件中的所有资源，以便您可以选择要导入的资源。要选择资源将导入到哪个项目，请单击 Browse 按钮。您还可以选择资源将被包含到的文件夹。完成后单击 Finish。导入的资源将被解压缩并复制到项目的[content]
## 创建 Java 类

要创建新的 Java 类，请在 Package Explorer 视图中右键单击项目并导航到 **New** | **Class**。您将看到 **New Java Class** 窗口，在其中输入有关类的信息。您可以更改类的超类，并添加其实现的接口，以及为从接口和抽象超类继承的抽象方法添加存根、从超类添加构造函数，并添加 main 方法。

要在包内创建类，只需在相应字段中输入其名称，或单击旁边的 **Browse** 按钮并选择包。如果您输入的包名不存在，Eclipse 会为您创建它。新的包也可以通过在 Package Explorer 中右键单击项目并导航到 **New** | **Package** 来创建。在 Project Explorer 中右键单击包而不是项目并导航到 **New** | **Class** 将导致类在该包内创建。

## 创建工作集

工作集提供了一种将工作区中的项目组织成子集的方式。当工作区中的项目太多时，很难在 **Package Explorer** 视图中找到所需的项目。例如，您当前未处理的项目可以保存在单独的工作集中。它们不会妨碍您当前的工作，但在需要时仍可使用。

要创建新的工作集，请打开 Package Explorer 的视图菜单（视图右上角的白色三角形），并选择 **Select Working Set**。单击 **New** 并选择工作集将包含的项目类型（本例中为 **Java**）。在下一页中，输入工作集的名称，并选择其将包含的项目。工作集创建后，选择 **Selected Working Sets** 选项，并标记您的工作集。单击 **OK**，Package Explorer 将仅显示您刚刚创建的工作集中的项目。

工作集创建后，它们会列在 Package Explorer 的视图菜单中。选择其中一个将使其成为 Package Explorer 中唯一可见的工作集。要同时查看多个工作集，请选择 **Select Working Set** 选项并标记要显示的项目。要再次查看整个工作区，请在视图菜单中选择 **Deselect Workspace**。您还可以通过将工作集选为 **Package Explorer** 视图的顶级元素来查看所有工作集及其嵌套项目。为此，请在视图菜单中导航到 **Top Level Elements** | **Working Sets**。

虽然在选择工作集时看不到属于其他工作集的项目，但它们仍加载在工作区中，因此会占用机器资源。为避免浪费这些资源，您可以通过右键单击它们并选择 Close Project 来关闭不相关的项目。您可以使用 Ctrl + A 键盘快捷键选择工作集中的所有项目。

如果您有大量项目，但从不同时处理所有项目（个人/业务项目、不同客户的项目等），您还可以为每个项目集创建特定的工作区。要创建新的工作区，请在菜单中导航到 File | Switch Workspace | Other，输入新工作区的文件夹名称，然后单击 OK。您可以在 Copy Settings 部分选择是否复制当前工作区的布局和工作集。

## 导入 Java 项目

如果您要处理现有项目，则可以根据项目源代码的获取方式以多种不同的方式将其导入 Eclipse。要打开导入向导，请导航到 File | Import。让我们介绍一下 General 类别下的选项：

- Archive file: 如果您要处理的项目已存在于工作区中，并且只想向其导入包含新资源的归档文件，请选择此选项。导入向导将列出归档文件中的所有资源，以便您可以选择要导入的资源。要选择资源将导入到哪个项目，请单击 Browse 按钮。您还可以选择资源将被包含到的文件夹。完成后单击 Finish。导入的资源将被解压缩并复制到项目的[content]
## 创建 Java 类

要创建新的 Java 类，请在 Package Explorer 视图中右键单击项目并导航到 **New** | **Class**。您将看到 **New Java Class** 窗口，在其中输入有关类的信息。您可以更改类的超类，并添加其实现的接口，以及为从接口和抽象超类继承的抽象方法添加存根、从超类添加构造函数，并添加 main 方法。

要在包内创建类，只需在相应字段中输入其名称，或单击旁边的 **Browse** 按钮并选择包。如果您输入的包名不存在，Eclipse 会为您创建它。新的包也可以通过在 Package Explorer 中右键单击项目并导航到 **New** | **Package** 来创建。在 Project Explorer 中右键单击包而不是项目并导航到 **New** | **Class** 将导致类在该包内创建。

## 创建工作集

工作集提供了一种将工作区中的项目组织成子集的方式。当工作区中的项目太多时，很难在 **Package Explorer** 视图中找到所需的项目。例如，您当前未处理的项目可以保存在单独的工作集中。它们不会妨碍您当前的工作，但在需要时仍可使用。

要创建新的工作集，请打开 Package Explorer 的视图菜单（视图右上角的白色三角形），并选择 **Select Working Set**。单击 **New** 并选择工作集将包含的项目类型（本例中为 **Java**）。在下一页中，输入工作集的名称，并选择其将包含的项目。工作集创建后，选择 **Selected Working Sets** 选项，并标记您的工作集。单击 **OK**，Package Explorer 将仅显示您刚刚创建的工作集中的项目。

工作集创建后，它们会列在 Package Explorer 的视图菜单中。选择其中一个将使其成为 Package Explorer 中唯一可见的工作集。要同时查看多个工作集，请选择 **Select Working Set** 选项并标记要显示的项目。要再次查看整个工作区，请在视图菜单中选择 **Deselect Workspace**。您还可以通过将工作集选为 **Package Explorer** 视图的顶级元素来查看所有工作集及其嵌套项目。为此，请在视图菜单中导航到 **Top Level Elements** | **Working Sets**。

虽然在选择工作集时看不到属于其他工作集的项目，但它们仍加载在工作区中，因此会占用机器资源。为避免浪费这些资源，您可以通过右键单击它们并选择 Close Project 来关闭不相关的项目。您可以使用 Ctrl + A 键盘快捷键选择工作集中的所有项目。

如果您有大量项目，但从不同时处理所有项目（个人/业务项目、不同客户的项目等），您还可以为每个项目集创建特定的工作区。要创建新的工作区，请在菜单中导航到 File | Switch Workspace | Other，输入新工作区的文件夹名称，然后单击 OK。您可以在 Copy Settings 部分选择是否复制当前工作区的布局和工作集。

## 导入 Java 项目

如果您要处理现有项目，则可以根据项目源代码的获取方式以多种不同的方式将其导入 Eclipse。要打开导入向导，请导航到 File | Import。让我们介绍一下 General 类别下的选项：

- Archive file: 如果您要处理的项目已存在于工作区中，并且只想向其导入包含新资源的归档文件，请选择此选项。导入向导将列出归档文件中的所有资源，以便您可以选择要导入的资源。要选择资源将导入到哪个项目，请单击 Browse 按钮。您还可以选择资源将被包含到的文件夹。完成后单击 Finish。导入的资源将被解压缩并复制到项目的[content]
## 创建 Java 类

要创建新的 Java 类，请在 Package Explorer 视图中右键单击项目并导航到 **New** | **Class**。您将看到 **New Java Class** 窗口，在其中输入类的信息。您可以更改超类，添加实现的接口，为接口和抽象超类继承的抽象方法添加存根，从超类添加构造函数，并添加 main 方法。

要在包内创建类，只需在相应字段输入名称，或单击旁边的 **Browse** 按钮选择包。若输入的包名不存在，Eclipse 会为您创建。新的包也可通过在 Package Explorer 中右键单击项目并导航到 **New** | **Package** 创建。在 Project Explorer 中右键单击包而非项目并导航到 **New** | **Class**，类将在该包内创建。

## 创建工作集

工作集用于将工作区项目组织成子集。项目过多时，在 **Package Explorer** 视图中查找项目会困难。例如，未处理的项目可放入单独工作集，不会干扰当前工作但仍可使用。

要创建新工作集，请打开 Package Explorer 视图菜单（视图右上角白色三角形），选择 **Select Working Set**。单击 **New** 并选择工作集包含的项目类型（本例为 **Java**）。在下一页输入工作集名称并选择包含的项目。创建后选择 **Selected Working Sets** 选项并标记工作集。单击 **OK**，Package Explorer 仅显示该工作集中的项目。

工作集创建后会列在 Package Explorer 视图菜单中。选择其一则该工作集为唯一可见工作集。要同时查看多个工作集，请选择 **Select Working Set** 并标记要显示的项目。要恢复整个工作区，请在视图菜单选择 **Deselect Workspace**。也可将工作集设为 **Package Explorer** 视图的顶级元素以查看所有工作集及其嵌套项目。为此在视图菜单导航到 **Top Level Elements** | **Working Sets**。

选择工作集时虽看不到其他工作集的项目，但它们仍加载在工作区中会占用资源。为避免浪费，可右键单击并选择 Close Project 关闭无关项目。可使用 Ctrl + A 快捷键选择工作集中的所有项目。

若项目很多但从不同时使用全部，可为每个项目集创建特定工作区。要创建新工作区，请在菜单导航到 File | Switch Workspace | Other，输入新工作区文件夹名称并单击 OK。可在 Copy Settings 部分选择是否复制当前工作区布局和工作集。

## 导入 Java 项目

处理现有项目时，可根据源代码获取方式以多种方式导入 Eclipse。打开导入向导请导航到 File | Import。以下介绍 General 类别下的选项：

- Archive file: 若项目已存在于工作区且只想导入包含新资源的归档文件，请选择此选项。导入向导会列出归档文件中的所有资源供选择。要选择导入目标项目请单击 Browse 按钮。也可选择资源包含的文件夹。完成后单击 Finish。导入资源将被解压缩并复制到项目文件夹。
- Existing Projects into Workspace: 若要导入新项目，请在导入向导中选择此选项。若项目源文件已压缩为归档文件（.zip、.tar、.jar 或 .tgz 格式），无需解压，只需在向导下一页标记 Select archive file 选项并指向归档文件。若已解压代码，请标记 Select root directory 并指向项目。向导会列出文件夹或归档文件中找到的 Eclipse 项目。选择要导入的项目并单击 Finish。可将导入项目添加到特定工作集并选择是否复制到工作区文件夹。强烈建议这样做以提高简洁性和可移植性；您知道所有 Eclipse 项目所在位置，易于备份或迁移到其他机器。
- **File System**: 若工作区已有项目且想添加文件系统中的现有资源，请使用此向导。下一页中通过勾选选择要导入的资源。单击 **Browse** 按钮选择项目和资源导入的文件夹。单击 **Finish** 按钮后，资源将被复制到工作区中项目的文件夹。
- **Preferences**: 通过选择此选项可将 Eclipse 首选项文件导入工作区。首选项文件包含代码风格和编译器首选项、已安装 JRE 列表以及 **Problems** 视图配置。可从所选配置文件中选择要导入的首选项。

## 从版本控制服务器导入项目

存储在版本控制服务器中的项目可直接导入 Eclipse。有多种版本控制软件，各有优缺点，大多数均可通过插件被 Eclipse 支持。GIT 是最常用的版本控制软件之一；将在 *Chapter 4, Managing a Project* 中详细介绍其在 Eclipse 中的高效使用方法。**CVS** 是默认唯一支持的版本控制系统。要导入由其管理的项目，请在导入向导中导航到 **CVS | Projects**。在下一页填写服务器信息，然后单击 **Finish**。

## 介绍 Java 视图

如 *Chapter 1, Introduction* 中所述，Eclipse 用户界面由称为视图的元素组成。以下部分将介绍与 Java 开发相关的[content]
## 创建 Java 类

要创建新的 Java 类，请在 Package Explorer 视图中右键单击项目并导航到 **New** | **Class**。会打开 **New Java Class** 窗口，输入类信息。可更改超类，添加实现的接口，为接口和抽象超类继承的抽象方法添加存根，从超类添加构造函数，并添加 main 方法。

要在包内创建类，可在相应字段输入名称，或单击旁边的 **Browse** 按钮选择包。若包名不存在，Eclipse 会自动创建。新的包也可通过在 Package Explorer 中右键单击项目并导航到 **New** | **Package** 创建。在 Project Explorer 中右键单击包而非项目并导航到 **New** | **Class**，类将在该包内创建。

## 创建工作集

工作集用于将工作区项目组织成子集。项目过多时，在 **Package Explorer** 视图中查找项目会困难。例如未处理的项目可放入单独工作集，不干扰当前工作但仍可使用。

要创建新工作集，请打开 Package Explorer 视图菜单（视图右上角白色三角形），选择 **Select Working Set**。单击 **New** 并选择工作集包含的项目类型，本例为 **Java**。在下一页输入工作集名称并选择包含的项目。创建后选择 **Selected Working Sets** 选项并标记工作集。单击 **OK**，Package Explorer 仅显示该工作集项目。

工作集创建后会列在视图菜单中。选择其一则该工作集为唯一可见工作集。要同时查看多个工作集，请选择 **Select Working Set** 并标记要显示的项目。要恢复整个工作区，请在视图菜单选择 **Deselect Workspace**。也可将工作集设为 **Package Explorer** 视图的顶级元素以查看所有工作集及其嵌套项目。为此在视图菜单导航到 **Top Level Elements** | **Working Sets**。

选择工作集时虽看不到其他工作集的项目，但它们仍加载会占用资源。为避免浪费，可右键单击并选择 Close Project 关闭无关项目。可使用 Ctrl + A 快捷键选择工作集中的所有项目。

若项目很多但从不同时使用全部，可为每个项目集创建特定工作区。要创建新工作区，请在菜单导航到 File | Switch Workspace | Other，输入新工作区文件夹名称并单击 OK。可在 Copy Settings 部分选择是否复制当前工作区布局和工作集。

## 导入 Java 项目

处理现有项目时，可根据源代码获取方式以多种方式导入 Eclipse。打开导入向导请导航到 File | Import。以下介绍 General 类别选项：

- Archive file: 若项目已存在于工作区且只想导入包含新资源的归档文件，请选择此选项。导入向导会列出归档文件中的所有资源供选择。要选择导入目标项目请单击 Browse 按钮。也可选择资源包含的文件夹。完成后单击 Finish。导入资源将被解压缩并复制到项目文件夹。
- Existing Projects into Workspace: 若要导入新项目，请在导入向导中选择此选项。若项目源文件已压缩为归档文件（.zip、.tar、.jar 或 .tgz 格式），无需解压，只需在向导下一页标记 Select archive file 选项并指向归档文件。若已解压代码，请标记 Select root directory 并指向项目。向导会列出文件夹或归档文件中找到的 Eclipse 项目。选择要导入的项目并单击 Finish。可将导入项目添加到特定工作集并选择是否复制到工作区文件夹。强烈建议这样做以提高简洁性和可移植性。
- **File System**: 若工作区已有项目且想添加文件系统中的现有资源，请使用此向导。下一页通过勾选选择要导入的资源。单击 **Browse** 按钮选择项目和资源导入的文件夹。单击 **Finish** 按钮后，资源将被复制到工作区中项目的文件夹。
- **Preferences**: 通过选择此选项可将 Eclipse 首选项文件导入工作区。首选项文件包含代码风格和编译器首选项、已安装 JRE 列表以及 **Problems** 视图配置。可从所选配置文件中选择要导入的首选项。

## 从版本控制服务器导入项目

存储在版本控制服务器中的项目可直接导入 Eclipse。有多种版本控制软件，各有优缺点，大多数均可通过插件被 Eclipse 支持。GIT 是最常用的版本控制软件之一；将在 *Chapter 4, Managing a Project* 中详细介绍其在 Eclipse 中的高效使用方法。**CVS** 是默认唯一支持的版本控制系统。要导入由其管理的项目，请在导入向导中导航到 **CVS | Projects**。在下一页填写服务器信息，然后单击 **Finish**。

## 介绍 Java 视图

如 *Chapter 1, Introduction* 中所述，Eclipse 用户界面由称为视图的元素组成。以下部分将介绍与 Java 开发相关的[content]
## 创建 Java 类

要创建新 Java 类，在 Package Explorer 视图中右键单击项目并导航到 **New** | **Class**。打开 **New Java Class** 窗口输入类信息。可更改超类，添加实现的接口，为抽象方法添加存根，从超类添加构造函数，并添加 main 方法。

要在包内创建类，可在相应字段输入名称，或单击 **Browse** 按钮选择包。若包名不存在，Eclipse 会自动创建。新的包也可通过右键项目并导航到 **New** | **Package** 创建。右键包而非项目并导航到 **New** | **Class**，类将在该包内创建。

## 创建工作集

工作集用于将工作区项目组织成子集。项目过多时在 **Package Explorer** 视图中查找困难。未处理的项目可放入单独工作集，不干扰当前工作但仍可使用。

创建新工作集：打开 Package Explorer 视图菜单，选择 **Select Working Set**，单击 **New** 选择项目类型 **Java**，输入名称并选择项目。创建后选择 **Selected Working Sets** 并标记工作集，单击 **OK**，Package Explorer 仅显示该工作集项目。

工作集会列在视图菜单中。选择其一则唯一可见。要同时查看多个工作集，选择 **Select Working Set** 并标记项目。要恢复整个工作区，选择 **Deselect Workspace**。也可将工作集设为 **Package Explorer** 顶级元素查看所有工作集及其嵌套项目，导航到 **Top Level Elements** | **Working Sets**。

选择工作集时其他工作集项目仍加载会占用资源。可右键选择 Close Project 关闭无关项目。可使用 Ctrl + A 选择工作集中所有项目。

项目很多但不同时使用全部时，可为每个项目集创建特定工作区。创建新工作区：菜单 File | Switch Workspace | Other，输入文件夹名称并单击 OK。可在 Copy Settings 部分选择是否复制当前布局和工作集。

## 导入 Java 项目

处理现有项目时可根据源代码获取方式导入 Eclipse。打开导入向导：File | Import。General 类别选项如下：

- Archive file: 项目已存在于工作区且只想导入归档文件中的新资源时选择。向导会列出归档文件资源供选择。单击 Browse 按钮选择导入目标项目，也可选择资源包含的文件夹。完成后单击 Finish，资源将被解压缩并复制到项目文件夹。
- Existing Projects into Workspace: 导入新项目时选择。若源文件已压缩为 .zip、.tar、.jar 或 .tgz，无需解压，标记 Select archive file 并指向归档文件。若已解压，标记 Select root directory 并指向项目。向导会列出找到的 Eclipse 项目，选择后单击 Finish。可将项目添加到特定工作集并选择是否复制到工作区文件夹，强烈建议这样做。
- **File System**: 工作区已有项目且想添加文件系统中现有资源时使用。下一页勾选选择资源，单击 **Browse** 选择项目和导入文件夹。单击 **Finish** 后资源复制到工作区项目文件夹。
- **Preferences**: 选择此选项可导入 Eclipse 首选项文件。文件包含代码风格、编译器首选项、已安装 JRE 列表及 **Problems** 视图配置。可选择要导入的首选项。

## 从版本控制服务器导入项目

版本控制服务器中的项目可直接导入 Eclipse。多种版本控制软件各有优缺点，大多数可通过插件支持。GIT 是最常用的之一，将在 *Chapter 4, Managing a Project* 中详细介绍。**CVS** 是默认唯一支持的系统。导入其管理的项目：导入向导中导航到 **CVS | Projects**，填写服务器信息后单击 **Finish**。

## 介绍 Java 视图

如 *Chapter 1, Introduction* 所述，Eclipse UI 由视图组成。以下介绍与 Java 开发相关的[content]
## 创建 Java 类

在 Package Explorer 视图中右键项目，导航到 **New** | **Class**。打开 **New Java Class** 窗口输入信息。可改超类，添加接口，为抽象方法加存根，从超类加构造函数，加 main 方法。

要在包内创建类，输入名称或点 **Browse** 选择包。包不存在时 Eclipse 会创建。新建包：右键项目，**New** | **Package**。右键包再 **New** | **Class**，类将在该包内创建。

## 创建工作集

工作集用于将工作区项目分组。项目多时在 **Package Explorer** 难找。可把未用项目放单独工作集，不干扰当前工作。

创建工作集：打开 Package Explorer 视图菜单，选 **Select Working Set**，点 **New** 选类型 **Java**，输入名称并选项目。创建后选 **Selected Working Sets** 标记工作集，点 **OK**，仅显示该工作集项目。

工作集列在视图菜单。选其一则唯一可见。要看多个，选 **Select Working Set** 标记项目。要看全部工作区，选 **Deselect Workspace**。也可将工作集设为 **Package Explorer** 顶级元素，导航 **Top Level Elements** | **Working Sets**。

工作集选中时其他项目仍加载占用资源。可右键选 Close Project 关闭无关项目。Ctrl + A 可全选工作集项目。

项目多但不同时用，可为每组建工作区。菜单 File | Switch Workspace | Other，输入文件夹名点 OK。可在 Copy Settings 复制布局和工作集。

## 导入 Java 项目

处理现有项目可按获取方式导入 Eclipse。打开向导：File | Import。General 选项：

- Archive file: 项目已在工作区，只想导入归档新资源时选。向导列出资源，点 Browse 选目标项目，可选文件夹。点 Finish，资源解压复制到项目文件夹。
- Existing Projects into Workspace: 导入新项目时选。若已压缩为 .zip/.tar/.jar/.tgz，无需解压，选 Select archive file 并指向文件。若已解压，选 Select root directory 并指向项目。向导列出 Eclipse 项目，选后点 Finish。可加到工作集并选是否复制到工作区，强烈建议。
- **File System**: 工作区已有项目，想加文件系统资源时用。勾选资源，点 **Browse** 选项目和文件夹。点 **Finish** 后资源复制到项目文件夹。
- **Preferences**: 可导入 Eclipse 首选项文件。含代码风格、编译器首选项、JRE 列表及 **Problems** 视图配置。可选要导入项。

## 从版本控制服务器导入项目

版本控制服务器项目可直接导入 Eclipse。多种软件各有优缺点，多可通过插件支持。GIT 最常用，将在 *Chapter 4, Managing a Project* 详细介绍。**CVS** 为默认唯一支持系统。导入其项目：向导中 **CVS | Projects**，填服务器信息后点 **Finish**。

## 介绍 Java 视图

如 *Chapter 1, Introduction* 所述，Eclipse UI 由视图组成。以下介绍 Java 开发相关视图。

## Package Explorer 视图

**Package Explorer** 视图为显示项目内容的默认视图。按包层次显示类，不论实际文件层次。也显示项目构建路径。

以下为 Package Explorer 视图截图：

![](img/d7c3f2018603810fae83e1a6fd98749e_38_0.png)

## Java Editor 视图

Java Editor 用于编辑 Java 源文件，为 Java 透视图中主视图，位于屏幕中间。

以下为 Java Editor 视图截图：

![](img/d7c3f2018603810fae83e1a6fd98749e_38_1.png)

Java Editor 不止普通文本编辑器，含多项功能，便于新手写代码并提升资深程序员[content]
## 创建 Java 类

右键 Package Explorer 中的项目，选 **New** | **Class**。打开 **New Java Class** 窗口输入信息。可改超类，加接口，为抽象方法加存根，加构造函数，加 main 方法。

要在包内创建，输入名称或点 **Browse** 选包。包不存在 Eclipse 会创建。新建包：右键项目 **New** | **Package**。右键包选 **New** | **Class**，类在该包内创建。

## 创建工作集

工作集用于分组工作区项目。项目多时在 **Package Explorer** 难找。未用项目可放单独工作集。

创建：视图菜单选 **Select Working Set**，点 **New** 选 **Java**，输入名称选项目。创建后选 **Selected Working Sets** 标记，点 **OK**，仅显示该工作集。

工作集列在视图菜单。选其一则唯一可见。选 **Select Working Set** 可多选。选 **Deselect Workspace** 看全部。导航 **Top Level Elements** | **Working Sets** 可以工作集为顶级元素查看。

工作集选中时其他项目仍加载。可右键 Close Project 关闭无关项目。Ctrl + A 全选工作集项目。

项目多但不同时用，可建多个工作区。File | Switch Workspace | Other，输入文件夹名点 OK。可在 Copy Settings 复制布局和工作集。

## 导入 Java 项目

File | Import 打开向导。General 选项：

- Archive file: 项目已在工作区，只导入归档新资源。向导列出资源，点 Browse 选目标项目和文件夹。点 Finish 解压复制。
- Existing Projects into Workspace: 导入新项目。已压缩则选 Select archive file，未解压则选 Select root directory。向导列出项目，点 Finish。可加工作集并选是否复制到工作区。
- **File System**: 向工作区项目添加文件系统资源。勾选资源，点 **Browse** 选项目和文件夹。点 **Finish** 复制。
- **Preferences**: 导入 Eclipse 首选项。含代码风格、编译器、JRE 列表、**Problems** 视图配置。可选导入项。

## 从版本控制服务器导入项目

版本控制服务器项目可直接导入。多种软件可通过插件支持。GIT 最常用，详见 *Chapter 4, Managing a Project*。**CVS** 为默认支持。导入：向导 **CVS | Projects**，填服务器信息点 **Finish**。

## 介绍 Java 视图

如 *Chapter 1, Introduction* 所述，Eclipse UI 由视图组成。以下介绍 Java 相关视图。

## Package Explorer 视图

**Package Explorer** 为默认显示项目内容的视图。按包层次显示类，也显示构建路径。

截图：

![](img/d7c3f2018603810fae83e1a6fd98749e_38_0.png)

## Java Editor 视图

Java Editor 用于编辑 Java 源文件，为 Java 透视图主视图，位于屏幕中间。

截图：

![](img/d7c3f2018603810fae83e1a6fd98749e_38_1.png)

Java Editor 不止文本编辑器，含多项功能，便于新手写代码并提升资深程序员生产力。以下介绍部分功能。
