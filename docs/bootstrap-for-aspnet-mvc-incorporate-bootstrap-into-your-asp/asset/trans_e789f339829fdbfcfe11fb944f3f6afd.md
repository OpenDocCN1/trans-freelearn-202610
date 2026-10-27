
> 请注意，我们将菜单中锚元素<a>的tabindex项设置为-1。这样做是为了防止该元素成为Tab键顺序的一部分。

- 接下来，向你的项目中添加一个名为Territories.cshtml的新分部视图。我们将使用此分部视图在用户每次更改区域时加载地区。为了加载该分部视图，我们需要向CustomerController添加一个返回PartialViewResult的新方法。此方法的代码如下：

```
public PartialViewResult Territories(int regionId)
{
    ViewBag.TerritoryId = new SelectList(_context.Territories.Where(t => t.RegionId == regionId), "TerritoryId", "TerritoryDescription");
    return PartialView();
}
```

此方法接收一个区域ID作为参数，然后将该区域关联的地区加载到ViewBag对象中。最后，该方法返回我们之前创建的Territories分部视图。

- 最后，为了在用户选择不同区域时加载关联的地区，我们需要向页面添加一点JavaScript。为此，在视图中创建一个新的section，并向其中添加以下JavaScript代码：

```
@section scripts{
<script type="text/javascript">
    $('#regiondropdown li a').click(function () {
        var $this = $(this);
        var region = $this.data('value');
        var regionName = $this.text();
        $('#RegionId').val(region);
        $.get("/Customers/Territories", { regionId: region })
            .done(function (data) {
                $('#regionbutton').text(regionName);
                $('#territorybutton').text('Select Territory');
                $('#territories').html(data);
            });
    });

    $('body').on("click", '#territories li a', function () {
        var $this = $(this);
        var territory = $this.data('value');
        var territoryName = $this.text();
        $('#TerritoryId').val(territory);
        $('#territorybutton').text(territoryName);
    });
</script>
}
```

上述JavaScript为区域下拉元素创建了一个点击处理程序，并将所选区域ID保存到一个变量中。然后，该变量被传递给jQuery的get方法，该方法进而向CustomerController中的Territories方法发起请求。Territories方法返回一个分部视图，其中包含区域ID与接收参数匹配的地区列表。

该代码还使用jQuery的on方法，为ID为territories的元素的所有锚点子元素附加事件处理程序。然后，事件处理程序将所选地区ID保存到我们之前创建的表单内的隐藏字段中。

## 模态对话框

模态框用于提供一种弹出式对话框样式的元素，可用于向用户提供信息，甚至允许用户在模态框内完成表单。Bootstrap模态框基本上由三部分组成：头部、主体和底部。你可以将任何标准HTML标记放在模态框的body元素内，包括标准文本，甚至嵌入的YouTube视频。

作为一般规则，始终将模态框的标记放在视图内的顶层位置——视图的顶部或底部是放置模态框标记的最佳位置。

使用以下HTML和Razor标记，我们将创建一个模态框，提示用户是否要继续从数据库中删除某个客户记录。我们将在模态框的body内声明一个<form>元素，并使用在模态框的footer元素内创建的按钮来提交表单。我们还将创建一个用户可以点击以关闭模态框的按钮。这是通过向按钮添加data-dismiss="modal"属性来实现的，如下所示：

```
<div class="modal fade" id="deleteConfirmationModal" tabindex="-1" role="dialog" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
                <h4 class="modal-title">Deletion confirmation</h4>
```

为了显示模态框，我们不需要编写任何JavaScript。只需给锚点或按钮元素添加data-toggle="modal"属性，当用户点击它时就会显示模态框，如下代码所示：

```
<a href="#" data-toggle="modal" data-target="#deleteConfirmationModal">Delete</a>
```

为了指示要显示哪个模态框，我们在data-target属性中指定模态框的ID。

上述代码的结果将显示一个模态框，类似于以下截图所示：

![](img/371b9a67300a03b7b335eb8a6fe55d94_94_0.png)

你也可以选择不使用data- API来显示模态框，而是使用jQuery来显示和隐藏模态框。以下JavaScript在页面加载时显示ID为deleteConfirmationModal的模态框：

```
$(document).ready(function () {
  $('#deleteConfirmationModal').modal('show');
});
```

你也可以使用同样的函数来隐藏模态框：

```
$('#deleteConfirmationModal').modal('hide');
```

## 选项卡

选项卡提供了一种将内容拆分为独立页面的选项。当你有一个特别大的表单并希望将其拆分为逻辑分组时，这是一个理想的组件。例如，在编辑客户记录时，你可能希望将联系详情和地址详情分开，如下面的截图所示：

![](img/371b9a67300a03b7b335eb8a6fe55d94_95_0.png)

Bootstrap选项卡分为两部分。首先，你需要指定选项卡名称，以及当用户点击选项卡时要显示的对应<div>元素的ID。这是通过创建一个标准的无序列表<ul>元素来实现的，其中选项卡名称作为子列表项<li>。<ul>元素的class必须设置为`nav nav-tabs`或`nav nav-pills`，如下脚本所示：

```html
<ul class="nav nav-tabs">
    <li class="active"><a href="#info" data-toggle="tab">Customer Info</a></li>
    <li><a href="#address" data-toggle="tab">Address</a></li>
</ul>
```

你可以通过将<li>元素的data-toggle属性设置为tab或pill，并将<ul>元素的class设置为nav-pills，来使用选项卡或胶囊式导航。结果在浏览器中将如下面的截图所示：

![](img/371b9a67300a03b7b335eb8a6fe55d94_96_0.png)

要指定选项卡的内容，请创建一个新的<div>元素并将其class设置为tab-content。在父级<div>元素内为每个选项卡创建一个<div>元素，并将每个选项卡的<div>元素的class设置为tab-pane，如下所示：

```
<div class="tab-content well">
    <div class="tab-pane active" id="info">
        Info Tab Content goes here
    </div>
    <div class="tab-pane" id="address">
        Address Tab Content goes here
    </div>
</div>
```

在上述标记中，我们创建了两个选项卡，并通过将info选项卡的class设置为active，将其设为活动选项卡。

你还可以使用jQuery激活特定页面。要在页面加载时立即激活address选项卡，请使用以下代码：

```
$(document).ready(function () {
  $('.nav-tabs a[href="#address"]').tab('show');
});
```

## 工具提示

Bootstrap的工具提示插件是Jason Frame的jQuery.tipsy插件的更新版本。工具提示可用于向用户提供有关页面特定内容的附加信息标签，或提供对表单元素中预期输入的说明。

工具提示可用于多种元素；例如，以下标记在用户悬停在锚点<a>元素上时显示工具提示：

```
<a data-toggle="tooltip" data-placement="top" data-original-title="Welcome to the wonderful World of Grammar!" href="#">World of Grammar</a>
```

要在任何元素上使用工具提示，请向其添加data-toggle="tooltip"属性。你可以通过将data-placement属性设置为以下值之一来指定工具提示的位置：

- top
- bottom
- left
- right

最后，设置data-original-title属性的值，以指定工具提示内应显示的文本。

工具提示的一个注意事项是，出于性能考虑，data- API是选择加入的，这意味着你必须手动初始化插件。为此，请向页面添加以下JavaScript：

```
<script type="text/javascript">
  $(document).ready(function () {
    $('[data-toggle="tooltip"]').tooltip();
  });
</script>
```

