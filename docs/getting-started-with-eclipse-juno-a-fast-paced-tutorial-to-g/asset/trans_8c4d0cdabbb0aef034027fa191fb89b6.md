
## 文本控件

文本控件是用于字符串的可编辑文本字段。这些字符串可以包含一行或多行，可用于密码字段。用户也可以对其进行选择。

创建文本控件时可用的样式如下：

- SEARCH：搜索行为，可与图标组合
- ICON_CANCEL：显示 **取消** 图标
- ICON_SEARCH：显示 **搜索** 图标
- MULTI：允许多行
- SINGLE：允许单行
- PASSWORD：显示密码字段行为
- CENTER：将文本内容对齐到中心
- RIGHT：将文本内容右对齐
- LEFT：将文本内容左对齐
- READ_ONLY：使字段只读
- WRAP：换行文本而不是滚动行为

文本控件仅支持纯文本，这意味着字符必须具有相同的字体和大小。如果需要更灵活的文本控件，可以使用一个 `StyledText` 控件，它不是 SWT 的原生控件。

在 SWT 程序中使用文本字段时，控件在应用程序中具有某种功能的情况相当常见。程序员通常需要检测文本何时发生变化或过滤正在输入的字符串。为此，可能需要监听器处理以下文本控件事件：

- `DefaultSelection`：按下 *回车*
- `Modify`：文本被修改
- `Verify`：文本待验证
- `OrientationChange`：方向已更改

在下面的示例中，我们展示如何处理上述提到的其中一个事件，SWT.Modify。为此，我们创建一个文本控件，并为 SWT.Modify 事件添加一个监听器。此事件在用户输入任何字符时发送。此类监听器中代码的目的是计算已输入的字符数。消息显示在文本控件下方的标签中。

```
Display display = new Display();
Shell shell = new Shell(display);
shell.setSize(200,200);
final Text text = new Text(shell, SWT.CENTER | SWT.BORDER);
text.setSize(200,25);
final Label label = new Label(shell, SWT.CENTER);
label.setLocation(0,100);
label.setSize(200,25);
text.addListener(SWT.Modify, new Listener() {
  public void handleEvent(Event event) {
    label.setText(Integer.toString(text.getText().length()));
  }
});
shell.pack();
shell.open();
while (!shell.isDisposed()) {
  if (!display.readAndDispatch()) display.sleep();
}
display.dispose();
```

以下截图显示了与此代码片段相关的 GUI 输出：

![](img/d7c3f2018603810fae83e1a6fd98749e_109_0.png)

需要说明的是，在此示例中我们还使用了 setSize 和 setLocation 方法，它们分别负责设置控件的大小及其位置，以像素为单位的 x 和 y 坐标。pack() 方法也用于将控件自动调整为其首选大小。

## 按钮控件

按钮在桌面应用程序中经常使用，因为这些应用程序通常使用鼠标设备。当用户选择一个选项时，可以处理选择事件。按钮可以填充文本或图像，但不能同时使用，并且屏幕上没有获取多行文本的选项。

创建按钮控件时可用的样式如下：

- `ARROW`：以箭头外观绘制按钮
- `CHECK`：复选按钮行为
- `PUSH`：按下按钮行为
- `RADIO`：显示单选按钮行为
- `TOGGLE`：切换按钮行为
- `FLAT`：以扁平外观绘制按钮
- `UP`：绘制上箭头
- `DOWN`：绘制下箭头
- `LEFT`：绘制左箭头或左对齐
- `RIGHT`：绘制右箭头或右对齐
- `CENTER`：绘制中心箭头或居中对齐

唯一可用的事件是 SWT.Selection 事件，当按钮被点击时触发。

我们展示了如何使用按钮控件的示例，随后是应用程序输出如下：

```java
Display display = new Display ();
Shell shell = new Shell (display);
Label label = new Label (shell, SWT.NONE);
label.setText ("Enter your name:");
Text text = new Text (shell, SWT.BORDER);
text.setLayoutData (new RowData (100, SWT.DEFAULT));
final Label resultLabel = new Label(shell, SWT.BORDER);
resultLabel.setText("Nothing pressed");
Button ok = new Button (shell, SWT.PUSH);
ok.setText ("OK");
ok.addListener(SWT.Selection, new Listener() {
    public void handleEvent(Event event) {
        resultLabel.setText("OK pressed");
    }
});
Button cancel = new Button (shell, SWT.PUSH);
cancel.setText ("Cancel");
cancel.addListener(SWT.Selection, new Listener() {
    public void handleEvent(Event event) {
        resultLabel.setText("Cancel pressed");
    }
});
shell.setLayout (new RowLayout ());
shell.pack ();
shell.open ();
while (!shell.isDisposed ()) {
    if (!display.readAndDispatch ()) display.sleep ();
}
display.dispose ();
```

![](img/d7c3f2018603810fae83e1a6fd98749e_111_0.png)

在此示例中，我们创建了两个按下按钮，即 OK 和 Cancel，并添加了两个监听器，当每个按钮被按下时触发，显示消息 OK pressed，或 Cancel pressed。在实际应用中，此类应用程序的可能用途之一是在按下 OK 按钮时读取文本控件中输入的名称以进行记录。

## 列表控件

列表可用于显示用户可选择的一组字符串。它们以单列呈现，并且有用于从中选择、添加和删除项目的方法。列表的内容可以通过 setItems(String[] items) 方法设置。当需要设置或替换特定项目时，可以使用 setItem(int index, String item)。

类似于 setItem 方法，该控件通过使用 getItem 或 getItems 方法来获取列表中的一个或多个项目。

项目也可以通过 add 方法添加，并使用 remove 方法删除。

setSelection 和 getSelection 方法通常在需要执行与项目选择相关的操作时使用。

列表控件可用的样式如下：

- SINGLE：只能选择一个项目
- MULTI：可选择多个项目

每个样式的描述相当直接。我们按如下方式列出此控件可用的事件：

- SWT.Selection：选择了一个项目
- SWT.DefaultSelection：双击了一个项目

使用 SWT.Selection 方法的代码示例如下所述，其后是 GUI 结果如下：

```
Display display = new Display();
Shell shell = new Shell (display);
final List list = new List (shell, SWT.BORDER | SWT.MULTI | SWT.V_SCROLL);
for (int i=0; i<10; i++) list.add ("Item " + i);
list.setSize(200, 200);
list.addListener (SWT.Selection, new Listener () {
    public void handleEvent (Event e) {
        String string = "";
        int [] selection = list.getSelectionIndices();
        for (int i=0; i<selection.length; i++)
            string += selection[i] + " ";
        System.out.println ("Selected: " + string);
    }
});
shell.pack ();
shell.open ();
while (!shell.isDisposed ()) {
    if (!display.readAndDispatch ()) display.sleep ();
}
display.dispose ();
```

![](img/d7c3f2018603810fae83e1a6fd98749e_113_0.png)

在此示例中，我们使用 add 方法向列表添加了 10 个项目，并处理了事件 `SWT.Selection`。当选择我们示例中的这三个项目时，应用程序将以下字符串输出到控制台：**Selected: 2 4 5.**

## 组合框控件

一个 `Combo` 控件， commonly known as combo box， 是 `Text` 控件和 `List` 控件的组合。`Combo` 控件通常在用户点击 `Combo` 控件中的小图标时显示为下拉列表。

用户可以输入文本字段或从列表中选择项目，但如果需要，组合框可以设置为只读，此时只能选择预定义的项目值。

`Combo` 控件的方法是 `Text` 控件和 `List` 控件方法的组合。

`Combo` 控件可用的样式如下：

- `SIMPLE`：组合框始终具有可见列表
- `DROP_DOWN`：组合框是下拉列表
- `READ_ONLY`：组合框不可编辑

`Combo` 控件可用的事件如下：

- `DefaultSelection`：项目被双击或按下 *回车*
- `Modify`：文本已被修改
- `Selection`：项目被选中

