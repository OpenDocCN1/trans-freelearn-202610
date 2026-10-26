

## 有用的占位符控件

最后，我们在创作容器中添加一个 Label 控件。该 Label 将用于显示过程中可能出现的错误消息。与 TextBox 一样，我们会将其设置为类级变量，以便在其他方法中使用它。

```
private TextBox txtAuthoring;
private Label lblMessage;

protected override void CreateAuthoringChildControls
        (BaseModeContainer authoringContainer)
{
    - - - code continues - - -

    // Add a label for displaying messages
    lblMessage = new Label();
    authoringContainer.Controls.Add(lblMessage);
}
```

## 检索已保存的内容

当帖子打开进行编辑时，日期时间选择器控件的文本框会显示之前保存的日期和时间值。对于新帖子，不会有之前的选项；我们将只使用当前日期和时间。重写 LoadPlaceholderContentForAuthoring()：

```
protected override void LoadPlaceholderContentForAuthoring
        (PlaceholderControlEventArgs e)
{
    EnsureChildControls();
    try
    {
        if (WebAuthorContext.Current.Mode == WebAuthorContextMode.AuthoringNew)
        {
            // The default value of the control is the current date and time
            txtAuthoring.Text = DateTime.Now.ToString("dd MMM yyyy HH:mm");
        }
        else
        {
            txtAuthoring.Text = ((HtmlPlaceholder)this.BoundPlaceholder).Text;
        }
    }
    catch(Exception ex)
    {
        lblMessage.Text = "Failed to load placeholder content: " + ex.Message;
    }
}
```

## 保存日期和时间

当帖子保存时，我们读取存储在 TextBox 中的值，并将其写入底层的 HtmlPlaceholder。任何异常都会通过 try-catch 块捕获，并在屏幕上显示详细的错误消息。将重写的 SavePlaceholderContent() 方法添加到代码中：

```
protected override void SavePlaceholderContent(PlaceholderControlSaveEventArgs e)
{
    EnsureChildControls();
    try
    {
        ((HtmlPlaceholder)this.BoundPlaceholder).Html = txtAuthoring.Text;
    }
    catch(Exception ex)
    {
```

## 验证日期和时间

为了确保只保存有效的日期，我们将重写 OnSavingContent() 方法。我们通过尝试将 TextBox 中存储的字符串转换为 DateTime 对象来检查要保存的值是否为有效日期。如果是有效日期，就不会引发异常。否则，我们捕获异常，保存操作可以被取消。

```
protected override void OnSavingContent(PlaceholderControlSavingEventArgs e)
{
    try
    {
        DateTime.Parse(txtAuthoring.Text.Trim());
        base.OnSavingContent(e);

        // Clear any messages on the screen
        lblMessage.Text = "";
    }
    catch(Exception ex)
    {
        e.Cancel = true;
        lblMessage.Text = ex.Message;
    }
}
```

## 显示选中的日期和时间

在展示模式下，选中的日期和时间显示在屏幕上的一个 Label 控件中。让我们重写 CreatePresentationChildControls() 方法，并向展示容器中添加一个 Label 控件。

```
private Label lblPresentation;

protected override void CreatePresentationChildControls(
        BaseModeContainer presentationContainer)
{
    lblPresentation = new Label();
    presentationContainer.Controls.Add(lblPresentation);
}
```

存储在底层 HtmlPlaceholder 中的先前保存的日期和时间值会从内容仓库中检索出来，并显示在之前加载的 Label 控件中。添加重写的 LoadPlaceholderContentForPresentation() 方法，如下所示：

```
protected override void LoadPlaceholderContentForPresentation
        (PlaceholderControlEventArgs e)
{
    EnsureChildControls();
    try
    {
        lblPresentation.Text = ((HtmlPlaceholder)this.BoundPlaceholder).Text;
    }
    catch(Exception ex)
    {
        lblPresentation.Text = ex.Message;
    }
}
```

日期时间选择器占位符控件已完成。接下来，让我们构建用于选择日期和时间的对话框。

## 日期和时间选择器对话框

日期和时间选择器对话框包含：

- 一个用于选择日期的 ASP.NET Calendar 控件
- 两个 DropDownList 控件，一个用于选择小时，另一个用于选择分钟
- 一个用于设置日期和时间值的 Button 控件

向 MCMS Web 项目中添加一个名为 DateTimePicker.aspx 的 Web 窗体（在本示例中，我们将其添加到 Tropical Green 项目的 Dialogs 文件夹中）。确保将 Page Layout 属性更改为 FlowLayout。现在切换到 HTML 视图，并向表单中添加以下标题：

```
- - - code continues - - -
<form id="DateTimePicker" method="post" runat="server">
<b>Pick Date:</b>
<hr>
<b>Pick Time</b>
<hr>
</form>
- - - code continues - - -
```

切换回设计视图。将以下控件拖放到 Web 窗体上，并如下所示进行排列。

| 控件 | 属性 | 属性值 |
|---|---|---|
| Calendar | ID | Calendar1 |
| DropDownList | ID | ddlHour |
| DropDownList | ID | ddlMinute |
| Button | ID | btnOK |
|  | Text | OK |
|  | Width | 100px |

![](img/442e26bf28f2df81fbf8dff3478f88e3_253_0.png)

双击窗体进入代码后置文件。在 Page_Load() 事件处理程序中，我们从 InitialValue 查询字符串参数中检索之前选定的日期和时间。如果由于某种原因（可能是 TextBox 为空）该参数不包含有效的日期和时间，我们将使用当前日期和时间。

```
private void Page_Load(object sender, System.EventArgs e)
{
    if(!Page.IsPostBack)
    {
        // Retrieve the previously saved date and time
        DateTime initialDate;

        try
        {
            initialDate = DateTime.Parse(Request.QueryString["InitialValue"]
                .ToString());
        }
        catch
        {
            // No previously saved date and time found.
            // Use the current date and time.
            initialDate = DateTime.Now;
        }
    }
```

先前选定的日期和时间用于初始化 Calendar 控件以及构成选中时间小时和分钟的两个下拉列表。在 Page_Load() 事件处理程序中，我们还填充小时和分钟下拉框。

```
private void Page_Load(object sender, System.EventArgs e)
{
    if (!Page.IsPostBack)
    {
        . . . code continues . . .
        Calendar1.SelectedDate = initialDate;
```

虽然对话框运行良好，但我需要一个更便携的控件，可以在项目之间使用，而无需每次使用该控件时都重新创建 Web 窗体。我有哪些选择？

要在多个项目中部署此控件，请考虑以下选项之一：

1. 在 `/IIS_CMS/` 文件夹内创建一个文件夹，并将对话框存储在那里。由于所有 MCMS Web 项目都有一个名为 CMS 的虚拟目录映射到 `IIS_CMS` 文件夹，因此对话框的路径将是 `/CMS/SubFolder/datetimepicker.aspx`。
2. 您可以效仿 MCMS 在将 `/nr/` 文件夹部署到同一台计算机上的所有网站时使用的概念，即把包含 DateTimePicker 控件的文件夹添加到每个使用该控件的虚拟网站。
3. 或者，使用 MCMS 在将 `/CMS/` 文件夹部署到新的 MCMS 模板项目时使用的技术。将包含 DateTimePicker 控件的文件夹添加到需要它的每个模板项目。在代码中获取控件路径时，使用 `Page.Request.ApplicationPath` 返回的值。要为新项目实现自动化，您可以为 Visual Studio .NET 创建新的企业模板。有关创建和编辑企业模板的更多信息，请参阅 `http://msdn.microsoft.com/Library/en-us/vsent7/html/vxoriEnterpriseTemplates.asp`。
4. 重写日期时间选择器控件，使日历、小时和分钟下拉菜单在控件本身上生成。您可以将它们嵌入 `<div>` 标记中，并使用客户端脚本在点击“选择日期”按钮时显示它们。

## 将占位符控件添加到模板文件

要将该控件添加到 MCMS 项目中，您必须将该组件添加到工具箱中：

- 右键单击要将控件添加到的选项卡。
- 选择“添加/移除项”，然后选择添加“.NET Framework 组件”。
- 选择“浏览…”，然后选择 `UsefulPlaceholderControls.dll`。单击“打开”。
- 单击“确定”关闭对话框，并将控件添加到工具箱。

将控件添加到工具箱后，您可以像使用常规占位符控件一样，将其拖放到模板文件上并设置其属性。

