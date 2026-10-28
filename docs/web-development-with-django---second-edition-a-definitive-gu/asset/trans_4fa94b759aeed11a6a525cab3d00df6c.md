

14. 使用 PyCharm 中的 New HTML File 功能，在 reviews 应用的 templates 目录下创建一个 form-example.html 文件。由于这是一个独立的模板（它不扩展任何其他模板），我们需要在其中渲染消息。在打开的 <body> 标签后添加以下代码以遍历所有消息并显示它们：

```
{% for message in messages %}
<p><em>{{ message.level_tag|title }}:</em>
{{ message }}</p>
{% endfor %}
```

这将遍历我们添加的消息并显示标签（在我们的例子中为 Success），然后是消息。

15. 然后，添加正常的表单渲染和提交代码：

```
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <p>
        <input type="submit" value="Submit">
    </p>
</form>
```

保存并关闭此文件。

你可以在 https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/blob/main/Chapter07/Exercise7.03/bookr/reviews/templates/form-example.html 处查看此文件的完整版本。

16. 启动 Django 开发服务器，然后导航到 http://127.0.0.1:8000/publishers/new/。你应该会看到一个空白的 PublisherForm 被显示：

![](img/4840a43efd335abd10b93708225692d6_372_0.png)

Figure 7.20 – Blank publisher form

![](img/4840a43efd335abd10b93708225692d6_374_0.png)

Figure 7.22 – Existing Publisher 1 information

尝试对现有的 **Publisher** 实例进行更改。注意保存后，消息会不同——它会告诉用户 *Publisher* 实例已被 *updated* 而不是 created：

![](img/4840a43efd335abd10b93708225692d6_374_1.png)

Figure 7.23 – Publisher after updating instead of creating

在本练习中，我们从模型（`PublisherForm` 由 `Publisher` 创建）实现了一个 `ModelForm`，并看到了 Django 如何自动生成具有正确验证规则的表单字段。然后我们使用了表单内置的 `save` 方法，在 `publisher_edit` 视图中保存对 `Publisher` 实例的更改（或自动创建它）。我们将两个 URL 映射到该视图。第一个用于编辑现有 `Publisher` 并向视图传递 pk。另一个不向视图传递此 pk，表示应创建 `Publisher` 实例。最后，我们使用浏览器实验了创建新的 `Publisher` 实例然后编辑现有实例。

## Activity 7.01 – styling and integrating the publisher form

在 *Exercise 7.03 – creating and editing a publisher* 中，你添加了 `PublisherForm` 来创建和编辑 `Publisher` 实例。你使用一个未扩展任何其他模板的独立模板构建了它，因此它缺少全局样式。在本次活动中，你将构建一个通用的表单详情页，该页面将显示一个 Django 表单，类似于 `form-example.html`，但扩展自基础模板。

模板将接受一个变量来显示正在编辑的模型类型。你还将更新主 base.html 模板以使用 Bootstrap 样式渲染 Django 消息。

以下步骤将帮助你完成此活动：

- 1. 首先编辑项目的 base.html 文件。用一个 div 容器包裹 content 块，以获得更美观的布局和一些间距。用 class="container-fluid" 的 <div> 元素包围现有的 content 块。
- 2. 在 messages 中渲染每条消息（类似于 Exercise 7.03 – creating and editing a publisher 的步骤 14）。在你刚刚创建的 <div> 容器之后但在 content 块之前添加 {% for %} 块。你应该使用 Bootstrap 框架类——以下片段会帮助你：
- 3. 在 reviews 应用的命名空间 templates 目录下创建一个名为 instance-form.html 的新模板。
- 4. instance-form.html 应扩展自 reviews 应用的 base.html。
- 5. 传递给此模板的上下文将包含一个名为 instance 的变量。这将是正在编辑的 Publisher 实例，或在创建新的 Publisher 实例时为 None。上下文还将包含一个 model_type 变量，这是一个字符串，表示模型类型（在本例中为 Publisher）。使用这两个变量填充 title 块模板标签：
- 6. instance-form.html 应包含一个 content 块模板标签，以覆盖 base.html 的 content 块。
- 7. 在 content 块内添加一个 <h2> 元素并使用与 title 相同的逻辑填充它。为了获得更好的样式，将出版商名称包裹在 <em> 元素中。
- 8. 在模板中添加一个 <form> 元素，method 为 post。由于我们发布回同一 URL，因此不需要指定 action。
- 9. 在 <form> 主体中包含 CSRF token 模板标签。

```
<div class="alert alert-{% if message.level_tag == 'error' %}danger{% else %}{{ message.level_tag }}{% endif %}" role="alert">
    {{ message }}
</div>
```

Bootstrap 类和 Django 消息标签在大部分情况下名称对应（例如 success 和 alert-success）。例外是 Django 的 error 标签。对应的 Bootstrap 类是 alert-danger。有关 Bootstrap 警告的更多信息，请参阅 https://getbootstrap.com/docs/4.0/components/alerts/。这就是为什么你需要在此片段中使用 if 模板标签。

- - 如果 instance 为 None，则标题应为 New Publisher
- - 否则，标题应为 Editing Publisher <Publisher Name>

- 10. 在 `<form>` 内使用 `as_p` 方法渲染 Django 表单（其上下文变量为 `form`）。
- 11. 向表单添加一个 `submit <button>`。其文本应取决于你是在编辑还是创建。使用编辑时的 `Save` 文本或创建时的 `Create`。你可以在此处使用 Bootstrap 类进行按钮样式设置。它应具有 `class="btn btn-primary"` 属性。
- 12. 在 `reviews/views.py` 中，`publisher_edit` 视图不需要太多更改。更新 `render` 调用以渲染 `instance-form.html` 而不是 `form-example.html`。
- 13. 更新传递给 `render` 调用的上下文字典。它应包括 `Publisher` 实例（已定义的 `publisher` 变量）和 `model_type` 字符串。上下文字典已包含 `form`（一个 `PublisherForm` 实例）。你可以删除 `method` 键。
- 14. 由于我们已完成 `form-example.html` 模板的使用，它可以被删除。

完成后，`Publisher` 创建页面（位于 http://127.0.0.1:8000/publishers/new/）应如下图所示 Figure 7.24：

![](img/4840a43efd335abd10b93708225692d6_376_0.png)

Figure 7.24 – The Publisher creation page

编辑 Publisher 时（例如 http://127.0.0.1:8000/publishers/1/），你的页面应如下图所示 Figure 7.25：

![](img/4840a43efd335abd10b93708225692d6_377_0.png)

Figure 7.25 – The Editing Publisher page

保存 Publisher 实例后，无论是创建还是编辑，你都应该在页面顶部看到一条成功消息（Figure 7.26）：

![](img/4840a43efd335abd10b93708225692d6_377_1.png)

Figure 7.26 – Success message rendered as a Bootstrap alert

## Activity 7.02 – Review Creation UI

> **Note**
> 
> The solution for this activity can be found on `https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/ActivitySolutions`.

*Activity 7.01 – styling and integrating the Publisher form* 非常广泛；然而，通过完成它，你创建了一个基础，使添加其他 *edit* 和 *create* 视图变得更容易。你将在本次活动中亲身体验这一点，当你将构建用于创建和编辑评论的表单时。因为 `instance-form.html` 模板是通用制作的，你可以在其他视图中重用它。

在本活动中，你将创建一个 review `ModelForm`，然后添加一个 `review_edit` 视图来创建或编辑 Review 实例。你可以重用 *Activity 7.01 – styling and integrating the Publisher form* 中的 `instance-form.html`，并传入不同的上下文变量使其与 Review 模型一起工作。在处理评论时，你将在一本书的上下文中操作——也就是说，`review_edit` 视图必须接受 Book PK 作为参数。你将单独获取这本书并将其分配给你创建的 Review 实例。

以下步骤将帮助你完成此活动：

- 1. 在 `forms.py` 中，添加一个 `ReviewForm`，它继承自 `ModelForm`；其模型应为 Review（确保导入 Review 模型）。
  
  `ReviewForm` 应排除 `date_edited` 和 `book` 字段，因为用户不应在表单中设置这些字段。数据库允许任何评分，但我们可以覆盖 `rating` 字段，使用需要最小值为 0 且最大值为 5 的 `IntegerField`。
  
  创建一个名为 `review_edit` 的新视图。它应接受两个参数在 request 之后：`book_pk`，为必需，以及 `review_pk`，为可选（默认为 None）。使用 `get_object_or_404` 快捷方式获取 Book 实例和 Review 实例（对每种类型调用一次）。获取评论时，确保评论属于该书。如果 `review_pk` 为 None，则 Review 实例也应为 None。

- 2. 如果 `request` 方法为 `POST`，则使用 `request.POST` 和 review 实例实例化一个 `ReviewForm`。确保导入此 `ReviewForm`。
  
  如果表单有效，则保存表单但将 save 的 `commit` 参数设为 False。然后，将返回的 Review 实例上的 book 属性设置为在 *步骤 2* 中获取的 book。
  
  如果 Review 实例正在更新而不是创建，则还应将 `date_edited` 属性设置为当前日期和时间。使用 `from django.utils.timezone.now()` 函数。然后，保存 Review 实例。

