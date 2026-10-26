

## 有用的占位符控件

DataGrid 创建完成后，我们就可以开始插入列了。我们要添加的第一列是编辑/更新列。在 DataGrid 的术语中，这就是 EditCommandColumn。它包含一个 Edit 链接，供作者选择他们想要修改的网格行。点击后，该行会转变为一系列文本框。作者修改完行内容后，点击 Update 链接即可将工作保存回网格。

当然，如果不编写必要的逻辑，Edit 和 Update 链接不会执行编辑和更新的工作。我们将在稍后编写 OnEditCommand() 方法用于编辑，以及 OnUpdateCommand() 方法用于更新。与此同时，我们将这些方法分别注册到 DataGrid 的 EditCommand 和 UpdateCommand 事件。

```
// - - - code continues - - -
protected override void CreateAuthoringChildControls(
                                  BaseModeContainer authoringContainer)
{
    // Add a DataGrid control to the authoring container
    dg = new DataGrid();

    // Add an Edit/Update column to the DataGrid
    EditCommandColumn c0 = new EditCommandColumn();
    c0.EditText = "Edit";
    c0.UpdateText = "Update";
    dg.Columns.Add(c0);
    dg.EditCommand += new DataGridCommandEventHandler(this.OnEditCommand);
    dg.UpdateCommand += new DataGridCommandEventHandler(this.OnUpdateCommand);

    authoringContainer.Controls.Add(dg);
}
```

我们还将为作者提供一个 Delete 按钮，以便从 DataGrid 中删除行。自动在 DataGrid 中创建按钮的列类型是 ButtonColumn。与之前添加的编辑和更新链接一样，我们需要为删除项目编写逻辑。我们在 OnDeleteCommand() 事件处理程序中执行此操作，将在稍后处理。现在，我们将该方法注册为另一个 DataGridCommandEventHandler 到 DataGrid 的 DeleteCommand 事件。

```
// - - - code continues - - -
protected override void CreateAuthoringChildControls(
                                  BaseModeContainer authoringContainer)
{
    // - - - code continues - - -

    // Add a Delete button to the DataGrid
    ButtonColumn c1 = new ButtonColumn();
    c1.CommandName = "Delete";
    c1.Text = "Delete";
    dg.Columns.Add(c1);
    dg.DeleteCommand += new DataGridCommandEventHandler(this.OnDeleteCommand);

    authoringContainer.Controls.Add(dg);
}
```

接下来，我们添加一个 Label 用于在从 DataGrid 加载和保存内容时显示错误信息（如果有）。请记住将 Label 定义为类级变量，以便类中的其他方法可以访问它。

```
// - - - code continues - - -
private Label lblMessage;
protected override void CreateAuthoringChildControls(
                                  BaseModeContainer authoringContainer)
```

## 我正在使用 Telerik MCMS 占位符控件。如何使用客户端脚本访问其内容？

使用客户端脚本访问 Telerik MCMS 占位符控件中的内容的技术是类似的。首先，我们需要获取包含 HTML 的对象的 ID。为此，在 Telerik MCMS 占位符控件中输入一些文本并保存页面。返回到编辑模式并使用 Notepad 查看页面的 HTML 源代码。按 Ctrl+F 并搜索刚刚输入的文本。注意包含内容的 `<div>` 标签的 ID，在控件 v2.12 中类似于这样：

```
`radEditorContainer_(ID of control)_AuthoringModeControlsContainer_Editor_(ID of Control)_AuthoringModeControlsContainer`
```

对于 ID 为 RadEditorPlaceholderControl1 的占位符控件，检索其内容的 JavaScript 如下：

```html
<script language="javascript">

var content =
document.all.radEditorContainerRadEditorPlaceholderControl1_AuthoringModeControlsContainer_EditorRadEditorPlaceholderControl1_AuthoringModeControlsContainer.innerHTML;

</script>
```

请注意，在控件的不同版本中，包围内容的 `<div>` 标签的 ID 会有所不同。你必须检查生成的源代码以获取你正在使用的特定版本的 `<div>` 标签的 ID。

## 检查 HtmlPlaceholderControl 是否为空

现在我们知道了如何检索 HtmlPlaceholderControl 的当前内容，就可以继续验证它了。当满足以下任一条件时，HtmlPlaceholderControl 被认为为空：

- 它包含一个空字符串。
- 它仅包含不可见标签和空格，例如 `<p>&nbsp;</p>`，或 `<p><div>&nbsp;</div></p>` 等。

根据你的需求，你可能还需要检查占位符内容是否包含默认文本。

用于检查这些条件的 JavaScript 在本节中介绍。要验证的控件的名称将被添加到例程的名称中，以确保如果在同一模板文件中放置了多个验证控件；它们各自将拥有自己的客户端例程。代码使用了客户端的 `isEmpty()` 方法来检查占位符是否为空。

```
else
{
    // content is valid
    return true;
}
}
function isEmpty(content)
{
    // Add more tags to ignore in this list if you need to.
    // Here, we ignore <hr> and <img> tags
    // Additional tags should be added and separated with a "|" character.
    var tagsToKeep = "img|hr";

    // This regular expression matches all <img> and <hr> tags
    var regExpTagsToKeep = "<[\\s]*(\" + tagsToKeep + ")\\b[^>]*>";
    var reTagsToKeep = new RegExp(regExpTagsToKeep, "gim");

    // exit if one of the tags to keep is included.
    if (content.match(reTagsToKeep))
    {
        // Placeholder is not empty.
        return false;
    }

    // This regular expression gets all tags in the content
    var regExpForAllTags = "<[ ^ ]*>";
    var reAllTags = new RegExp(regExpForAllTags,"gim");

    // Remove all tags by replacing with an empty string
    content = content.replace(reAllTags, "");

    // Remove all spaces and non-breaking spaces (&nbsp;)
    content = content.replace(" ","");
    content = content.replace("&nbsp;","");

    if (content == "")
    {
        // if after removing all tags, we are left with an empty string
        // Placeholder is empty.
        return true;
    }
    else
    {
        // if after removing all tags, we still have content.
        // Placeholder is not empty.
        return false;
    }
}
</script>
```

## 使用正则表达式匹配标签

我们刚刚看到的 isEmpty() JavaScript 函数使用了两个正则表达式。第一个检查 HTML 中是否存在 <img> 和 <hr> 标签。 recall 只要 HTML 包含这些单标签项，就不为空。让我们更仔细地看看这个表达式：

<[\s|/]*(img|hr)\b[^>]*>

第一个和最后一个字符 < 和 > 匹配所有 HTML 标签的开始和结束角括号。

在开始的角括号之后是空格或正斜杠的转义序列，后跟一个星号（Kleene 星），\s*。这确保了在开始角括号后带有空格的标签（例如 < img>）和闭合标签都包含在匹配中。

