
本质上，我们已将错误处理逻辑复制到此控制器中，并对控制器 action 返回的元组进行模式匹配，以便适当处理每种错误。这非常有益，因为错误处理逻辑通常分散在我们的应用程序各处。例如，匹配全部子句已在 index 和 show action 中重复出现。请注意，在 Elixir 中处理意外错误并不常见，因为我们通常更关注 happy path。

然而，你可能希望确保客户端始终获得有效的响应，在我们的例子中这涉及重定向到根路径。在真实的生产部署中，我们很可能会将此错误标记到错误跟踪系统中，以便日后调查其原因。

使用 fallback controller 的另一个巨大优势是控制器可读性的提高。让我们查看此更改后 show action 的代码：

```elixir
def show(conn, %{"id" => id}) do
  with {:ok, file} <- Storage.get_file_info("hardcoded_user_id", id) do
    render(conn, file: file)
  end
end
```

现在，我们的代码完全专注于 happy path，从而得到一个更容易理解的函数。这个函数如此简单，以至于 show action 中无需再添加任何内容。让我们通过最后两个 action new 和 create 来结束本节。我们将一起讨论它们，因为这两个 action 是耦合的：new 用于渲染表单以允许用户上传新文件，而 create action 用于处理表单提交。在下一节讨论视图和模板时，我们将看到此表单是如何创建和提交的。现在，让我们先看看 new action 的代码：

```elixir
def new(conn, %{"path" => path}) do
  render(conn, changeset: Storage.Media.create_changeset(%Storage.Media{}), path: path)
end
```

```elixir
def new(conn, _params) do
  render(conn, changeset: Storage.Media.create_changeset(%Storage.Media{}), path: "$")
end
```

正如我们所见，此 action 的代码非常简单。当调用此 action 时，我们只需渲染 "new.html" 模板。正如我们在下一节将看到的，此模板需要一个 changeset。因此，我们生成一个空的 changeset 并将其传递给模板。我们有两个函数子句，就像 index action 一样：一个用于 "path" 参数提供的情况，另一个用于未提供的情况。此 action 的代码只是将来自路由器的请求连接到正确的视图和模板的一种方式。现在让我们分析 create action 的代码：

```elixir
def create(conn, %{"file" => file_params}) do
  file_content =
    file_params
    |> Map.get("file")
    |> Map.get(:path)
    |> File.read!()

  with {:ok, :upload_enqueued, _changeset} <- Storage.store(
    "hardcoded_user_id", file_params["file_name"],
    file_params["full_path"], file_content) do
    conn
    |> put_flash(:info, "Your file upload is enqueued. A confirmation will appear shortly.")
    |> redirect(to: file_path(conn, :index, "path": file_params["full_path"]))
  else
    {:error, changeset} ->
      conn
      |> put_flash(:error, "Unable to upload file. Please check the errors below.")
      |> render(:new, changeset: changeset, path: file_params["full_path"])
  end
end
```

此 action 稍微复杂一些，让我们逐段分析。我们将 file_params 绑定到通过表单提交的数据。在这种情况下，它将是一个包含有关正在上传的文件的信息以及文件本身的 map。我们首先获取文件的二进制内容。我们使用 File.read!/1 是因为 Phoenix 为通过表单提交的文件创建了一个临时文件。我们将文件的二进制内容绑定到 file_content，然后将其传递给 Storage.store/4。除了内容本身之外，此函数还接收文件所有者的用户 ID，以及文件的名称和路径。

当文件正确存储时，我们会弹出一条 flash 消息以向用户提供反馈，并将其重定向到刚刚上传的文件的路径。请注意，此操作仅在管道上排队上传。当我们在本章末尾讨论 Phoenix Channels 时，我们将着手提供确认文件已成功上传的通知。

当 store 函数返回错误时，我们会弹出一条 flash 消息说明发生了某些错误，并使用包含错误的 changeset 渲染 "new.html" 模板。此细节将允许我们直接从 changeset 渲染错误。我们将在下一节更详细地了解其工作原理。现在，我们只需要记住将失败的 changeset 传递给模板很重要。还值得指出的是，在这种情况下，我们需要将模板名称传递给 render 函数，因为如果省略它，Phoenix 将尝试渲染名为 "create.html" 的模板，而该模板不存在。在这种情况下，我们只传递一个原子 (:new)，而不是字符串 "new.html"。我们之所以可以这样做，是因为当向 render 传递原子时，Phoenix 将使用当前请求（在我们的例子中为 HTML）的格式来构建模板的完整名称。

我们现在已经完成了控制器部分！接下来，我们将看看控制器完成所有处理并开始渲染视图后会发生什么。

## 渲染视图

在探索了 Phoenix 中控制器的工作方式后，我们现在将看看下一步：渲染视图。视图是一个包含渲染函数的模块，其目的是将数据转换为最终用户可以消费的格式。延续我们在前两节中给出的示例，我们将查看文件以及它们在我们的应用程序中如何被渲染。我们将创建一个文件视图模块，命名为 ElixirDripWeb.FileView。以下是它的代码：

```
$ cat apps/elixir_drip_web/lib/elixir_drip_web/views/file_view.ex

defmodule ElixirDripWeb.FileView do
  use ElixirDripWeb, :view

  def parent_directory(path) do
    Path.dirname(path)
  end
end
```

首先，我们给这个模块起的名字很重要。正如我们在上一节中看到的，文件控制器在渲染响应时从未提及视图模块。我们之所以能够做到这一点，是因为 Phoenix 会根据控制器模块的名称推断出视图模块的名称。为避免在此问题上产生混淆，Phoenix 在模块名称中始终使用单数形式。如果由于某种原因你无法遵循这些规则，并且想要指定要调用的视图的名称，你只需要在控制器中调用 render 时传递视图模块的名称即可。这又是 Phoenix 让你保持高效并从应用程序中移除样板代码的另一个例子，同时也给你根据需要调整框架的选项。

在这个视图模块内部，我们只看到一个函数 parent_directory，它将在模板中使用，正如我们稍后将看到的那样。首先我们需要澄清 Phoenix 中的视图和模板是什么。在许多其他框架中，这两个词可以互换使用，但在该框架中它们是两个不同的概念。视图，正如我们之前描述的那样，是一个包含渲染函数的模块。这些渲染函数可以是模块本身中的 render 函数（就像其他任何 Elixir 函数一样），也可以从模板定义。模板是从包含标记语言与 EEx 代码混合的文件编译而来的，这使我们能够在模板中替换变量并对其进行循环。此文件的编译会在相应的视图模块中生成一个函数。视图与模板之间的关注点分离使我们能够以任何所需的形式渲染数据，无论是从常规的 Elixir 函数、模板引擎（如 EEx）还是其他任何形式。现在，我们将只使用模板来渲染视图，但在本章后面构建 JSON API 时，我们将看到直接在视图模块中使用 render 函数的示例。

我们已经看到控制器如何将要渲染的模板名称传递给视图（或如何根据 action 名称推断它）。但视图是如何找到模板的呢？答案在于 Phoenix 使用的一种约定。它会在一个目录中查找模板，默认情况下是 lib/elixir_drip_web/templates。下一个目录的名称是从视图模块的名称推断出来的；在我们的例子中，视图模块名为 FileView，因此它会在 file 目录中查找模板。该目录包含以当前 action 命名的文件（或控制器传递的参数）。例如，文件索引 action 的模板位于 apps/elixir_drip_web/lib/elixir_drip_web/templates/file/index.html.eex。我们现在就来看看它。由于其中代码相当多，我们将逐段进行。你随时可以在我们提到的路径中查看完整文件。当为用户索引文件时，我们做的第一件事是渲染一个用于上传新文件的链接：

```
<%= link("Upload File", to: file_path(@conn, :new, "path": @current_path), class: "btn btn-primary") %>
```
