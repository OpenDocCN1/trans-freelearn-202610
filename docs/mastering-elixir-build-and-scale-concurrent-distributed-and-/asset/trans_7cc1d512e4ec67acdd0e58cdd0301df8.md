
```
</div>
<div class="form-group">
  <%= text_input f, :full_path, value: @path, placeholder: "Full Path",
    class: "form-control" %>
  <%= error_tag f, :full_path %>
</div>
<div class="form-group">
  <label>File</label>
  <%= file_input f, :file, class: "form-control" %>
</div>
<%= submit("Upload", class: "btn btn-primary") %>
<% end %>
```

模板的核心部分是 Phoenix.HTML.Form.form_for/4 函数。该函数的第一个参数可以是 changeset（正如我们在模板中所使用的），也可以是 connection——事实上，它可以是实现 Phoenix.HTML.FormData 协议的任何内容。这些随后被用来填充表单数据，例如默认值或错误信息。请记住，我们在控制器中调用 render 时将 changeset 传递给了模板。第二个参数是表单提交时调用的 action——在我们的例子中，是 file 控制器上的 create action。第三个参数是一个关键字列表，允许我们配置表单的行为。在本例中，我们传递了两个选项：multipart: true，它将表单的 enctype 设置为 multipart/form-data，使我们能够在此表单中上传文件；以及 as: :file，它定义了在服务器端收集表单所有参数的参数。这就是为什么在 file 控制器的 create action 中，我们对 file 参数进行模式匹配的原因。最后，form_for 的最后一个参数是一个接收表单作为参数的 lambda。form_for 函数将第一个参数（在我们的例子中是 changeset）转换为表单，并将其传递给上述 lambda。

现在，我们使用 Phoenix.HTML.Form 模块中的辅助函数来生成我们想要的输入字段。这些函数接收表单 (f) 作为参数。使用表单，这些函数还可以从 changeset 中获取数据并预填充字段。例如，如果我们的 changeset 为 filename 设置了默认值，它将自动显示在表单中。此外，Phoenix 表单与 Ecto change set 之间的集成允许我们直接在表单中渲染 changeset 的错误。这就是为什么我们对两个输入都调用 error_tag 的原因。error_tag 函数由 Phoenix 生成，默认位于应用程序的 ErrorHelpers 模块中（在我们的例子中是 ElixirDripWeb.ErrorHelpers）。

让我们通过有意在表单中生成错误来实际查看其效果：

![](img/c636f4cfb07f25f1db535995e50706ed_370_0.png)

我们在表单中插入了一条无效路径。后端检测到该路径无效，并返回了一个包含错误的 changeset。正如截图中所见，表单能够从 changeset 中获取错误并将其显示在对应字段旁边。如果未对 changeset 应用 action（或 action 为 :ignore），则错误不会显示，即使 changeset 的 :errors 字段中包含错误。通常，表单是使用空的 changeset 创建的，该 changeset 可能携带错误（例如由于缺少必填字段）。然而，我们不希望在用户对表单执行操作之前向其显示错误。

![](img/c636f4cfb07f25f1db535995e50706ed_370_1.png)

为了在应用程序中促进 HTML 安全，Phoenix 模板不会直接将数据类型转换为字符串。相反，Phoenix 为大多数数据类型实现了 `Phoenix.HTML.Safe`，从而保证返回安全的 HTML 表示。

表单的最后一部分是对 `submit` 函数的调用，该函数生成一个用于提交表单的按钮。第一个参数是按钮上显示的文本，第二个参数是一个选项关键字列表，这些选项会转发给底层的按钮标签。最后关于表单的一个说明，我们想指出在 Phoenix 中，表单会自动生成一个 CSRF token。默认情况下，Phoenix 也会检查此 token（通过 `:protect_from_forgery` plug），从而保护应用程序免受 CSRF 攻击。

## 布局

到目前为止，我们已经看到了一些模板示例以及它们如何用于渲染响应。然而，当控制器调用视图来渲染模板时，该视图并不会直接渲染；控制器首先渲染布局视图，然后布局视图再渲染模板本身。这样，我们可以在整个应用程序中保持一致的样式（例如设置页眉和页脚），而无需在各个模板中重复它们。

然而，布局是普通的模板。你对模板学到的所有内容同样适用于布局。Phoenix 生成器会创建一个 `app` 布局，在我们的例子中它位于 `apps/elixir_drip_web/lib/elixir_drip_web/templates/layout/app.html.eex`。该布局将被编译为 `ElixirDripWeb.LayoutView` 上的一个函数（位于 `apps/elixir_drip_web/lib/elixir_drip_web/views/layout_view.ex`）。

为了渲染你在控制器中设置的模板，Phoenix 会将该模板嵌套在 `app` 布局中。负责执行此操作的特定行是：

```
<%= render(@view_module, @view_template, assigns) %>
```

类似于我们对 `file` 模板所做的操作，我们正在调用 `render` 函数在当前布局（它本身也是一个模板）内部渲染另一个模板。`@view_module` 和 `@view_template` 变量由 Phoenix 分配，分别包含正在渲染的视图的模块名称和该视图内部的模板名称。如果出于某种原因你想更改正在渲染的布局，你有几个选项：调用控制器中的 `render` 时传递 `layout:` 选项；在控制器中调用 `put_layout/2` plug；或在路由器管道中调用 `put_layout/2` plug，使此更改对通过该管道的每个请求都生效。

在结束本节之前，我们想谈谈 `ErrorView`。该视图由 Phoenix 生成，是集中处理应用程序中错误渲染逻辑的地方。

Phoenix 会检测任何 400 或 500 级别的错误，并使用该视图中的 render/2 函数来渲染错误信息。以下是 Phoenix 生成的两个子句：

```
$ cat apps/elixir_drip_web/lib/elixir_drip_web/views/error_view.ex

defmodule ElixirDripWeb.ErrorView do
  use ElixirDripWeb, :view

  def render("404.html", _assigns) do
    "Page not found"
  end

  def render("500.html", _assigns) do
    "Internal server error"
  end

  # ...
end
```

这只会向浏览器返回文本，但在实际应用程序中，你可以自定义它以渲染你想要的适当的 404 和 500 页面。如果 Phoenix 找不到与当前错误匹配的 render/2 函数子句，则会执行 template_not_found/2 函数：

```
$ cat apps/elixir_drip_web/lib/elixir_drip_web/views/error_view.ex

defmodule ElixirDripWeb.ErrorView do
  # ...

  def template_not_found(_template, assigns) do
    render("500.html", assigns)
  end
end
```

正如我们所见，默认情况下它只会调用 "500.html" 模板。我们将在本章后面实现应用程序的 JSON API 时再次涉及该模块。

我们现在已经到达了视图部分的结尾。此时，我们已经涵盖了 Phoenix 工作原理的基础，从 endpoint 开始，经过 router，涉及 controller，最后结束于 view。结合我们在这些部分中获得的所有知识，我们现在将把它们应用到为我们的 ElixirDrip 应用程序添加身份验证。

## 用户认证

在上一章中，我们看到了如何在 Accounts 上下文中处理用户密码，使用 comeonin 库对其进行签名和验证。在本章中，我们的重点是扩展我们的 Web 界面，以允许用户注册和登录我们的应用程序，并限制某些页面仅对已登录用户开放。Elixir 生态系统中有一系列库可以在几行代码内实现此功能。然而，实现我们自己的身份验证解决方案有两个好处：它将给我们机会更深入地探索 Phoenix，并且它将给予我们在身份验证方式上更大的自由度，使我们能够将其调整以适应我们的需求。

为了让用户通过身份验证，我们首先需要在应用程序中创建用户的能力。我们将从下而上完成这部分工作，围绕用户身份验证构建逻辑，并在该部分完成后，我们将着手允许用户注册和登录。

正如我们在本章开头所暗示的，我们将使用 plugs 实现身份验证。我们将创建两个 plugs：第一个负责从 session 中提取用户 ID 并将对应的用户加载到连接中，第二个负责验证连接中是否存在当前用户，并在不存在时限制对某些内容的访问。让我们从第一个名为 FetchUser 的 plug 开始：

```
$ cat apps/elixir_drip_web/lib/elixir_drip_web/plugs/fetch_user.ex

defmodule ElixirDripWeb.Plugs.FetchUser do
  import Plug.Conn

  alias ElixirDrip.Accounts

  def init(options), do: options

  def call(conn, _options) do
    user_id = get_session(conn, :user_id)
    find_user(conn, user_id)
  end

  defp find_user(conn, nil), do: conn
  defp find_user(conn, user_id) do
    user = Accounts.find_user(user_id)
    assign_current_user(conn, user)
  end

  defp assign_current_user(conn, nil), do: conn
  defp assign_current_user(conn, user) do
    conn
```
