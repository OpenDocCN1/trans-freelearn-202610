

这里，由于 hasModels 属性设置为 true，列表中的所有元素都会显示一个右箭头（▶）。这表明用户可以点击它来导航到下一组节点。

当用户点击 ListView 中的任何元素时，将调用 attachDataSource 函数。点击元素的 id 将用于获取层次结构中的下一组元素。请注意，当你为 ListView 设置 DataSource 时，查询参数 parentID 会存在；会向服务发送一个 GET 请求，将 parentID 值指定给查询参数 id。远程服务会查找记录并将其返回。请求 URL 的形式如下：

```
/service?id=<parentID>
```

当你点击第一个元素（id = 1）时，服务将查找 parentID 为 1 的记录。假设服务返回以下记录：

```
[
  {"id": 11, "name": "Gallardo", "hasModels": false},
  {"id": 12, "name": "Aventador", "hasModels": false},
  {"id": 13, "name": "Huracan", "hasModels": false}
]
```

请注意，这三个对象中 hasModels 的值均为 false。现在，视图会更新以在 ListView 中显示上述记录，并且不包含右箭头。

![](img/2a66507445090790d34dc5588b56b24c_158_0.png)

用于渲染列表元素的模板会检查 hasModels 属性的值，并且仅在该值设置为 true 时才添加锚元素。在此情况下，该值为 false，因此不显示右箭头。

## 构建无限滚动列表

ListView 小部件可以自定义以显示详尽的项目列表。实现此功能有多种方式，例如，在列表底部添加一个按钮，点击后将下一组元素添加到列表中。另一种方式是随着用户向下滚动而添加更多元素。这称为无限滚动，当用户浏览详尽列表时，它能提供无缝体验。在本示例中，我们将看到这两种方法，即添加按钮以加载更多元素，以及创建在用户向下滚动时加载的无限滚动列表。

### 操作步骤...

在两种情况下，都必须配置 DataSource 对象，使其返回有限数量的记录。在为 ListView 小部件配置 DataSource 时，请指定 pageSize 属性：

```
dataSource: {
 transport: {
 read: '/movies/listService'
 },
 pageSize: 10
}
```

要在列表底部添加按钮，请指定 loadMore 选项并将其值设置为 true：

```
function initListView (e) {
 $('#listViewContainer').kendoMobileListView({
 dataSource: {
 transport: {
 read: '/movies/listService'
 },
 pageSize: 10
 },
 loadMore: true
 });
}
```

要启用无限滚动而不是添加加载更多按钮，请将 endlessScroll 属性设置为 true：

```
function initListView (e) {
    $('#listViewContainer').kendoMobileListView({
        dataSource: {
            transport: {
                read: '/movies/listService'
            },
            pageSize: 10
        },
        endlessScroll: true
    });
}
```

### 工作原理...

渲染页面时，带有 **Press to load more** 文本的按钮会显示在列表底部，如下图所示：

![](img/2a66507445090790d34dc5588b56b24c_160_0.png)

在无限滚动列表的情况下，当用户向下滚动时，会向服务发送请求。在加载列表期间，您将看到加载器显示，如下图所示：

![](img/2a66507445090790d34dc5588b56b24c_161_0.png)

## 显示可使用 ActionSheet 小部件执行的操作列表

Kendo Mobile ActionSheet 小部件用于显示当用户点击屏幕上的元素时可以执行的操作列表。ActionSheet 小部件从底部滑出，并显示可针对屏幕上点击的元素执行的操作列表。

### 操作步骤...

让我们创建一个包含 ListView 小部件和 ActionSheet 小部件的视图：

```html
<div
data-role="view"
data-title="Movies to watch"
data-layout="defaultLayout">
<ul>
```

```
id="listViewContainer">
</ul>
<ul
data-role="actionsheet"
id="optionActionSheet">
<li>
<a data-action="option1">Favourite</a>
</li>
<li>
<a data-action="option2">Watch</a>
</li>
<li>
<a data-action="option3">Remove</a>
</li>
</ul>
</div>
```

ListView 小部件的内容通过 DataSource 对象填充。ActionSheet 小部件（带有 data-role=actionsheet 的 ul）包含当用户点击 ListView 小部件中的任何元素时需要显示的操作列表。请注意，每个列表元素都包含带有 data-action 属性的锚元素。该属性用于指定当用户点击操作按钮时要调用的 JavaScript 函数。

下一步是将列表中的元素与 ActionSheet 小部件关联。要填充列表中的元素，可以使用模板，并且该模板可以指定对 ActionSheet 小部件的引用：

```
<script id="listViewTemplate" type="text/x-kendo-template">
<a
data-rel="actionsheet"
href="\\#optionActionSheet">
#= data #
</a>
</script>
```

这里，锚的 data-rel 属性设置为 actionsheet，href 属性引用 ActionSheet 小部件（id=optionActionSheet）。

### 工作原理...

ListView 小部件中的每个元素都有一个右箭头（>），这表示可以在列表元素上调用操作：

![](img/2a66507445090790d34dc5588b56b24c_163_0.png)

当您点击列表中的任何元素时，将显示 ActionSheet 小部件。小部件从屏幕底部滑出，并列出用户可以执行的选项或操作，如下图所示：

![](img/2a66507445090790d34dc5588b56b24c_163_1.png)

## 第 9 章

ActionSheet 小部件的覆盖层覆盖整个屏幕，并显示包含操作的列表。请注意，ActionSheet 小部件除了列出上述选项外，还包含 **Cancel** 按钮。

当用户点击之前的任何选项时，将调用关联的 JavaScript 函数。点击 **Cancel** 时，ActionSheet 小部件将隐藏并移除覆盖层。

### 更多内容...

在 Android 设备上，ActionSheet 小部件显示为 **Modal window**。如前所述，移动小部件已自定义以提供各种设备的原生外观和感觉。

![](img/2a66507445090790d34dc5588b56b24c_164_0.png)

此处选项也列出了 **Cancel** 选项。

### 使用 ScrollView 小部件浏览图片集合

ScrollView 小部件用于图片画廊或类似轮播的应用程序，您希望滚动到下一页以查看画廊中的下一张图片。该小部件允许您通过从右向左或从左向右滑动来移动到下一页或上一页。

### 操作步骤...

让我们创建一个包含 ScrollView 小部件的 `view`：

```html
<div
data-role="view"
data-title="Gallery"
data-layout="defaultLayout">
<div
data-role="scrollview">
</div>
</div>
```

这里，通过将 data-role 属性设置为 scrollview，在视图内创建了一个 ScrollView 小部件。ScrollView 小部件现在需要包含可以通过从右向左或从左向右滑动来浏览的页面。要创建页面，请在 ScrollView 容器内添加 div 元素，并将 data-role 属性设置为 page。每个页面小部件随后可以包含需要显示的图片：

```
<div
data-role="scrollview">
<div
data-role="page">
<img src="./images/large/her.jpg" />
</div><div
data-role="page">
<img src="./images/large/gravity.jpg" />
</div><div
data-role="page">
<img src="./images/large/12_years_a_slave.jpg" />
</div><div
data-role="page">
<img src="./images/large/american_hustle.jpg" />
</div><div
data-role="page">
<img src="./images/large/wolf_of_wall_street.jpg" />
</div>
.
.
.
</div>
```

### 工作原理...

渲染页面时，ScrollView 小部件会显示包含图片的第一页。

