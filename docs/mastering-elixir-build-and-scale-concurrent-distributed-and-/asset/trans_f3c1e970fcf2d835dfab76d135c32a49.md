
首先要指出的是 `<%= %>` 的使用。这些标签内的代码由 EEx 引擎执行，结果会被注入到模板中。我们也可以在 `<% %>` 内放置代码，它同样会被 EEx 引擎执行，但在这种情况下结果不会被注入回模板。如你在本书其余部分会看到的，我们大多会使用 `<%= %>` 形式。

然后，在这些标签内部，我们调用了 `link` 函数，它来自 `Phoenix.HTML.Link` 模块。这个函数以及我们在后续模板中会使用的其他 HTML 助手，在我们调用 `use ElixirDripWeb, :view` 时会被导入。这些助手非常有用，可以让我们在模板中创建常见的 HTML 结构。link 函数会创建一个锚元素。第一个参数是锚上显示的文本，而第二个参数是一个关键字列表。`to:` 键指定了链接的目标。我们使用之前见过的路径助手来创建一个指向文件资源 `:new` 动作的链接。注意我们如何将当前路径传递给这个函数。EEx 允许我们通过在名称前加上 `@` 来引用传递给模板的 assigns。这种语法会在 `@current_path` 未传递给模板时生成异常。为了避免在这种情况下抛出异常，我们可以使用 `assigns[:current_path]`。最后，我们传递了 `class:` 键，它允许我们定义正在创建的锚的类——在这个例子中，我们用它来将链接样式化为按钮。这些类之所以可用，是因为 Phoenix 生成器导入了 Bootstrap CSS 库（默认情况下放置在 `apps/elixir_drip_web/assets/css/phoenix.css`）。

现在让我们继续看下一部分：

```
<%= if Enum.empty?(@folders) && Enum.empty?(@files) do %>
  <p class="lead">This folder is empty!</p>
  <% else %>
  # ...
<% end %>
```

现在，我们使用条件表达式来检查 assigns 中提供的 folders 和 files 是否都为空。如果是，我们插入一段说明此事的段落。否则，我们执行 `else` 块中的内容，我们接下来会看到。在此之前，重要的是要注意，即使使用 `if`，我们仍在使用 `<%= %>` 记法。如第一章所述，Elixir 中的一切都是表达式，因此如果我们希望将 `if` 的结果注入到模板中，就应该使用适当的记法。现在让我们看看 else 块中的内容：

```
<table class="table table-striped">
  <%= if @current_path != "/" do %>
    <tr>
      <td><img src="<%= static_path(@conn, "/images/folder.svg") %>"></td>
      <td colspan="3"><%= link("..", to: file_path(@conn, :index, "path":
        parent_directory(@current_path))) %></td>
        </tr>
        <% end %>
```

这里是我们开始渲染文件夹和文件的地方。我们首先打开一个 table 元素，在其中插入每个文件夹和文件。在遍历之前，我们检查当前路径是否不是根路径。如果不是，我们希望渲染一个指向当前目录父目录的虚拟文件夹。在链接之前，我们渲染一个图像，这展示了如何指向静态文件夹内的文件：使用 static_path 路径助手，将文件的相对路径作为参数传递。图像之后，我们渲染一个文本为 ".." 的链接，指向当前目录的父级。注意我们如何传递路径查询参数：通过调用在 FileView 模块上定义的 parent_directory 函数。该函数以当前路径作为参数并返回父目录的路径。请注意，我们不需要在函数名前加上其模块。猜猜为什么？因为此模板将被编译为该模块内部的一个函数，这意味着我们可以像在其中编写常规 Elixir 函数一样调用该模块中的函数。让我们继续看下一部分：

```
<%= for folder <- @folders do %>
    <tr>
        <td><img src="<%= static_path(@conn, "/images/folder.svg") %>"></td>
        <td><%= link(folder.name, to: file_path(@conn, :index, "path": @current_path <> "/" <> folder.name)) %></td>
        <td><%= folder.size %> bytes</td>
        <td><%= folder.files %> files</td>
    </tr>
<% end %>
```

这里，我们看到了模板中循环的第一个示例。也就是说，我们使用 comprehension 来遍历作为参数传递的 folders 列表中的每个文件夹。除此之外，逻辑与我们已经看到的示例非常相似。首先，我们渲染一个图像，然后是一个链接，这个链接允许用户进入当前文件夹。最后，我们不是注入两个空列，而是用当前文件夹的大小（其下文件和文件夹大小的总和）和当前文件夹拥有的文件数量来填充它们。让我们继续看此模板的最后一部分：

```
<%= for file <- @files do %>
    <tr>
        <td><img src="<%= static_path(@conn, "/images/file.svg") %>"></td>
        <td><%= link(file.name, to: file_path(@conn, :show, file.id)) %></td>
        <td colspan="2"><%= file.size %> bytes</td>
    </tr>
<% end %>
</table>
```

这个示例与上一个非常相似。不同之处在于，我们现在遍历的是 files 而不是 folders。我们渲染了一个不同的图像，并且我们创建的链接指向特定的文件（使用其 ID）。

如前所述，模板会被编译为其各自视图模块内部的函数。这意味着在运行时，模板已经在内存中，不需要进行磁盘读取或模板引擎计算。此外，Phoenix 使用链表构建模板，而不是像许多其他框架那样拼接巨大的字符串。这样，Phoenix 在渲染模板时就不会在字符串之间来回复制。由于 Elixir 中的数据类型是不可变的，Phoenix 可以共享响应的静态部分，因为它们永远不会被修改。模板渲染时的这种效率是 Phoenix 的一个非常重要的部分，使其能够实现我们在本章开头描述的惊人性能。

在继续下一个模板之前，让我们看一下此模板运行时的截图：

![](img/c636f4cfb07f25f1db535995e50706ed_367_0.png)

我们现在将探索 show 动作的模板，它位于 `apps/elixir_drip_web/lib/elixir_drip_web/templates/file/show.html.eex`。在 show 动作的模板中，我们可能会复制 index 模板中渲染文件的内容，但这会造成逻辑重复。在本示例中，我们将看到如何嵌套模板，从而消除重复。首先，我们将创建一个新模板：

```
$ cat apps/elixir_drip_web/lib/elixir_drip_web/templates/file/
file.html.eex

<tr>
    <td><img src="<%= static_path(@conn, "/images/file.svg") %>"></td>
    <td><%= link(@file.name, to: file_path(@conn, :show, @file.id)) %></td>
    <td><%= @file.size %> bytes</td>
</tr>
```

```
<td/>
</tr>
```

这与我们在 index 模板最后一部分中展示的内容是一份拷贝，只是我们读取 file 的地方现在看到的是 @file，因为文件现在来自 assigns 而不是循环。现在，让我们在 show 模板中使用它：

```
$ cat apps/elixir_drip_web/lib/elixir_drip_web/templates/file/show.html.eex
```

```
<div>
  <table class="table table-striped">
    <%= render("file.html", conn: @conn, file: @file) %>
    <tr>
      <td> Full path: </td>
      <td colspan="3"><%= @file.full_path %></td>
    </tr>
    <tr>
      <td> Uploaded at: </td>
      <td colspan="3"><%= @file.uploaded_at %></td>
    </tr>
  </table>
  <%= link("Back to folder", to: file_path(@conn, :index, "path": @file.full_path), class: "btn btn-default") %>
</div>
```

为了使用 file 模板，我们只需调用 render，传递模板名称作为属性（连同必要的 assigns）。这样，我们在整个应用程序中就有了一种统一的方式来渲染文件，这不仅消除了重复，也促进了视觉一致性。我们还更新了 index 动作以使用 file 模板。由于这是 show 动作，除了渲染文件的一般信息外，我们还显示文件的完整路径以及上传时间。最后，我们渲染一个链接，允许用户返回此文件所属的文件夹。我们使用在 index 模板中看到的相同技术来实现这一点。

我们现在将探索最后一个模板，在其中学习如何在 Phoenix 中生成表单。这是 new 动作的模板：

```
$ cat apps/elixir_drip_web/lib/elixir_drip_web/templates/file/
new.html.eex
```

```
<h1>Upload a File</h1>
<%= form_for @changeset, file_path(@conn, :create), [multipart: true, as: :file], fn f -> %>
  <div class="form-group">
    <%= text_input f, :file_name, placeholder: "File Name", class: "form-control" %>
    <%= error_tag f, :file_name %>
```
