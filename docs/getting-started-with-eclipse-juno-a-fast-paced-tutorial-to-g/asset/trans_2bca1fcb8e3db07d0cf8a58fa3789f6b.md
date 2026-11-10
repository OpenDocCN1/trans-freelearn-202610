
-   `ToolItem` 事件如下：
  -   `SWT.Selection`：工具项已被选中

在下面的代码片段中，我们展示了一个关于创建工具栏以及工具项的非常简单的示例。也可以使用图像代替文本。

```
...
Display display = new Display();
Shell shell = new Shell(display);
ToolBar toolBar = new ToolBar(shell, SWT.HORIZONTAL);

ToolItem item = new ToolItem(toolBar, SWT.DROP_DOWN);
item.setText("Drop Down Behavior (Combo)");
toolBar.pack();
shell.pack();
shell.open();
while (!shell.isDisposed()) {
    if (!display.readAndDispatch()) display.sleep();
}
display.dispose();
...
```

我们可以看到创建了一个 `ToolBar` 项，之后一个具有下拉行为的工具项被插入到其中。在实际应用中，可能有必要为 `ToolItems` 对象添加监听器，以便处理生成的事件。

## 表格

表格可以被视为由多个单元格组成的二维网格。表格的一个可能用途是存储数据库查询的结果。为了向表格添加数据，有必要使用一个 `TableItem` 对象。每个 `TableItem` 对象的实例负责表格的一整行。值可以通过 `setText` 和 `setImage` 方法设置，这些方法接受一个整数作为参数来标识应修改的列。

与 TableItem 对象类似，TableColumn 对象接收一个表格作为参数以关联这两个对象。每次关联一个 TableColumn 对象时，对应表格中就会创建一个新列。

以下代码片段展示了如何创建 SWT 表格：

```
...
    Display display = new Display();
    Shell shell = new Shell(display);

    shell.setSize(500,400);
    shell.setText("Table Example");
    shell.setLayout(new FillLayout( ));

    Table table = new Table(shell, SWT.BORDER);
    TableColumn tc1 = new TableColumn(table, SWT.CENTER);
    TableColumn tc2 = new TableColumn(table, SWT.CENTER);
    TableColumn tc3 = new TableColumn(table, SWT.CENTER);
    tc1.setText("Name");
    tc2.setText("Age");
    tc3.setText("Country");
    tc1.setWidth(80);
    tc2.setWidth(40);
    tc3.setWidth(80);
    table.setHeaderVisible(true);
    TableItem item1 = new TableItem(table,SWT.NONE);
    item1.setText(new String[] {"Rodrigo","28","France"});
    TableItem item2 = new TableItem(table,SWT.NONE);
    item2.setText(new String[] {"Vinicius","28","USA"});
    TableItem item3 = new TableItem(table,SWT.NONE);
    item3.setText(new String[] {"Rafael","28","Brazil"});
    shell.open( );
    while(!shell.isDisposed()){
        if(!display.readAndDispatch( ))
            display.sleep( );
    }
    display.dispose();
...
```

相应代码的表格 GUI 如下所示：

![](img/d7c3f2018603810fae83e1a6fd98749e_134_0.png)

尽管可以直接在列上设置属性，但最好使用 TableLayout 类。TableLayout 提供了 addColumnData 方法，这使得描述表格中每列的外观变得更容易。

JFace 是一个用于处理常见 UI 编程任务的工具包。JFace 设计为与 SWT 协同工作并且依赖于系统。其最相关的特性是 actions 和 viewers。action 机制允许用户命令独立于其在 UI 中的确切位置来定义。Viewers 是某些 SWT 组件的基于模型的适配器，简化了以列表、表格或树结构组织的应用程序数据的展示，这在我们的示例中使用。TableLayout 是 org.eclipse.jface.viewers 包的一部分。它可以像 SWT 一样导入，其解释在上一章的开头，通过访问以下网站： http://wiki.eclipse.org/JFace

以下代码片段展示了如何使用 TableLayout 对象：

```
...
TableLayout layout = new TableLayout();
layout.addColumnData(new ColumnWeightData(40, 80, true));
layout.addColumnData(new ColumnWeightData(40, 80, true));
layout.addColumnData(new ColumnWeightData(40, 80, true));
Table table = new Table(shell, SWT.SINGLE);
table.setLayout(layout);
TableColumn column1 = new TableColumn(table, SWT.CENTER);
TableColumn column2 = new TableColumn(table, SWT.CENTER);
TableColumn column3 = new TableColumn(table, SWT.CENTER);
TableItem item = new TableItem(table, SWT.NONE);
item.setText( new String[] { "column 1",
    "column 2",
    "column 3" } );
item = new TableItem(table, SWT.NONE);
item.setText( new String[] { "a", "b", "c" } );
...
```

每次创建 addColumnData 时，它都会向表格添加一列。ColumnWeightData 对象以权重、最小宽度以及是否可调整大小作为参数。重要的是要记住，有必要实例化 TableLayout 对象来处理列特征以及 TableColumns 本身。

## 对话框

对话框是与主应用程序分离的窗口。通常将其置于用户面前，以引起其对需要执行的操作的注意。dialog 抽象类为 SWT 中的对话框提供了基础。可以扩展它以创建自定义对话框或使用已有的多个模板，例如确认消息框或文件对话框。

### MessageBox

MessageBox 对话框通常用于向用户显示信息，例如可能的错误，或在需要执行的操作上获取用户的输入。对话框可以显示多个按钮，例如是或否。当用户做出选择时，对应的代码分支将被执行。

使用 MessageBox 对话框时最简单的事情是通过 setMessage 方法设置其消息。尽管如此，还可以指定多种图标样式以及多种类型的按钮。我们按如下方式展示可能的样式组合，然后是可用图标的描述：

样式组合如下：
-   `SWT.OK`
-   `SWT.OK | SWT.CANCEL`
-   `SWT.YES | SWT.NO`
-   `SWT.YES | SWT.NO | SWT.CANCEL`
-   `SWT.RETRY | SWT.CANCEL`
-   `SWT.ABORT | SWT.RETRY | SWT.IGNORE`

可用对话框图标的描述如下：
-   `SWT.ERROR_ICON`：用于告知发生错误
-   `SWT.ICON_INFORMATION`：用于提供非交互式信息
-   `SWT.ICON_QUESTION`：用于为给定问题选择答案
-   `SWT.ICON_WARNING`：用于告知用户可能有害的操作
-   `SWT.ICON_WORKING`：告知用户工作正在进行中

以下代码片段展示了 MessageBox 对话框的使用：

```
...
MessageBox messageBox = new MessageBox(shell, SWT.ICON_QUESTION | SWT.
YES | SWT.NO);
messageBox.setMessage("Do you really want to quit?");
messageBox.setText("Exiting Application");
int response = messageBox.open( );
if (response==SWT.YES) {
    shell.dispose();
    System.exit(0);
}
...
```

以下屏幕截图显示了 MessageBox 示例的结果：

![](img/d7c3f2018603810fae83e1a6fd98749e_136_0.png)

### FileDialog

FileDialog 打开一个带有按钮以及列表框或树视图的对话框，可实现文件系统的导航。程序员只需实例化此类，指定几个参数，而无需进行繁重的编程。

```
FileDialog fd = new FileDialog(shell, SWT.OPEN);
fd.setText("Open");
fd.setFilterPath("/");
String[] filterExt = {"*.txt"};
fd.setFilterExtensions(filterExt);
String selected = fd.open( );
System.out.println(selected);
```

相应的 GUI 对话框在以下屏幕截图中显示：

![](img/d7c3f2018603810fae83e1a6fd98749e_137_0.png)

### ColorDialog

ColorDialog 的目的是允许在组件或文本中选择颜色。可以图形化地选择所需颜色，然后将其传递给组件。也可以手动选择 RGB 值。

```
Text text = new Text(shell, SWT.CENTER | SWT.BORDER);
text.setSize(200,25);
ColorDialog cd = new ColorDialog(shell);
cd.setText("ColorDialog");
cd.setRGB(new RGB(255,255,255));
RGB newColor = cd.open( );
text.setBackground(new Color(display, newColor));
```

相应的组件如下所示。请注意，该组件依赖于操作系统，在不同系统间可能有所不同。

![](img/d7c3f2018603810fae83e1a6fd98749e_138_0.png)
