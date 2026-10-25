

## Gallery

在这里，ScrollView 小部件还在屏幕底部包含一个分页器，用于高亮显示用户当前所在的页面。当你从右向左滑动时，下一页显示如下：

![](img/2a66507445090790d34dc5588b56b24c_166_1.png)

## 更多内容

当你渲染页面时，ScrollView 小部件默认显示页面集中的第一页。如果你想显示不同的页面，则为 ScrollView 小部件元素设置 `data-page` 属性：

```html
<div
    data-role="scrollview"
    data-page="4" >
</div>
```

页码从零开始索引，也就是说，当你将 `data-page` 属性指定为 4 时，列表中的第五页会被显示。

如果你想隐藏 ScrollView 小部件底部显示的分页器，则可以将 `data-enable-pager` 属性设置为 `false`：

![](img/2a66507445090790d34dc5588b56b24c_167_0.png)

在前面的截图中，`pager` 已被移除；但是，用户仍然可以通过滑动来浏览页面集。当你有一个需要浏览的详尽页面列表，并且在屏幕底部显示分页器不是你想要提供的体验时，此选项很有用。

## 创建 SplitView 小部件以在平板电脑中显示多个窗格，并在窗格之间构建交互

Kendo UI 库提供了一个 SplitView 小部件，这是一个专用于平板电脑的小部件。SplitView 小部件由两个或多个窗格组成，每个窗格由一个或多个视图小部件组成。通常，SplitView 小部件用于在一个窗格的列表中选择一个项目，并更新显示所选项目更多数据的另一个窗格。

## 操作方法

让我们创建两个窗格，左窗格和右窗格，然后将这些窗格包含在 SplitView 小部件中。以下是左窗格的标记：

```html
<div data-role="pane" id="nav-pane" data-layout="left-navbar-layout">
  <div data-role="view" id="test">
    <ul id="moviesList" data-role="listview" data-template="navbar-template"></ul>
  </div>
  <script type="text/x-kendo-template" id="navbar-template">
    <a href="#movieDescription?movieID=#= data.id #" data-target="content-pane">#= data.name #</a>
  </script>
  <div data-role="layout" data-id="left-navbar-layout">
    <header data-role="header">
      <div data-role="navbar">Left Pane</div>
    </header>
  </div>
</div>
```

通过将 data-role 属性设置为 pane 来创建窗格小部件。窗格包含一个视图小部件，该视图列出使用 ListView 小部件的项目。视图与布局相关联，ListView 小部件使用以下 DataSource 对象填充：

```javascript
var listElements = [
  {id: 1, name: 'Her', image: 'her.jpg'},
  {id: 2, name: 'Gravity', image: 'gravity.jpg'},
  {id: 3, name: '12 Years A Slave', image: '12_years_a_slave.jpg'},
  {id: 4, name: 'Captain Phillips', image: 'captain_phillips.jpg'},
  {id: 5, name: 'American Hustle', image: 'american_hustle.jpg'},
  {id: 6, name: 'The Wolf Of Wall Street', image: 'wolf_of_wall_street.jpg'},
  {id: 7, name: 'Nebraska', image: 'nebraska.jpg'},
  {id: 8, name: 'Dallas Buyers Club', image: 'dallas_buyers_club.jpg'},
  {id: 9, name: 'Philomena', image: 'philomena.jpg'}
];
```

现在让我们考虑 **右窗格** 小部件，它将用于显示 **左窗格** 中所选项目的详细信息：

```html
<div data-role="pane" id="content-pane" data-layout="right-content-layout">
  <div data-role="view" data-show="displayOrders" id="movieDescription">
    <div id="movieDetails"></div>
  </div>
  <div data-role="layout" data-id="right-content-layout">
    <header data-role="header">
      <div data-role="navbar">Right Pane</div>
    </header>
  </div>
</div>
```

**右窗格** 小部件也包含视图；当用户在 **左窗格** 中选择一个项目时，会生成该视图的内容。请注意，左窗格模板中指定的锚元素将 href 属性指定为 `#movieDescription?movieID=#= data.id #`，它引用 **右窗格** 中的视图。此外，它将 data-target 属性指定为 content-pane，它引用 **右窗格** 本身。

下一步是将两个窗格包含在 SplitView 小部件中：

```html
<div data-role="splitview">
  <div data-role="pane" id="nav-pane" data-layout="left-navbar-layout">
    .
  </div>
  <div data-role="pane" id="content-pane" data-layout="right-content-layout">
    .
    .
  </div>
</div>
```

**右窗格** 中的视图小部件将 data-show 属性设置为 displayOrders；data-show 属性用于指定在视图用于显示内容时应调用的 JavaScript 函数。当用户单击 **左窗格** 中的任何项目时，**右窗格** 中的视图用于显示详细信息。让我们使用 **右窗格** 小部件显示 **左窗格** 中所选元素的图像。displayOrder 函数实现如下：

```javascript
function displayOrders(e) {
  var movieID = parseInt(e.view.params.movieID, 10);
  for (var i = 0; i < listElements.length; i++) {
    if (listElements[i].id == movieID) {
      $('#movieDetails').html('<img src="images/large/' + listElements[i].image + '"/>');
      break;
    }
  }
}
```

在这里，函数首先获取所选元素的 id，然后获取相应元素的图像。然后，此图像在 **右窗格** 中包含的视图中显示。

同样，每当用户选择 **左窗格** 中 ListView 的某个项目时，相应的图像就会在 **右窗格** 中显示。

## 工作原理

当你首次渲染页面时，**左窗格** 小部件会使用 DataSource 中的元素进行填充。

| 左窗格 | 右窗格 |
|-----------|------------|
| Her       |            |
| Gravity   |            |
| 12 Years A Slave |    |
| Captain Phillips |    |
| American Hustle |    |
| The Wolf Of Wall Street | |
| Nebraska   |            |
| Dallas Buyers Club |    |
| Philomena  |            |

最初，**右窗格** 不显示任何内容，因为 **左窗格** 中没有选择任何项目。当你从 **左窗格** 的 ListView 小部件中选择任何项目时，**右窗格** 小部件会更新以显示图像：

| 左窗格 | 右窗格 |
|-----------|------------|
| Her       | ![American Hustle](image) |
| Gravity   |            |
| 12 Years A Slave |    |
| Captain Phillips |    |
| American Hustle |    |
| The Wolf Of Wall Street | |
| Nebraska   |            |
| Dallas Buyers Club |    |
| Philomena  |            |

## 更多内容

SplitView 小部件已将窗格水平堆叠；也可以通过将 `data-style` 属性指定为 `vertical` 来垂直堆叠这些窗格：

```html
<div data-role="splitview" data-style="vertical">
  .
  .
</div>
```

以下截图显示了窗格垂直堆叠时的外观：

![](img/2a66507445090790d34dc5588b56b24c_173_0.png)

## 使用 TabStrip 小部件在应用程序视图之间选项卡切换

TabStrip 小部件用于在应用程序的各种视图之间导航。TabStrip 通常放置在布局的页脚中，单击 TabStrip 中的按钮时，会显示相应的视图。

### 操作方法

让我们创建一个在页脚中包含 TabStrip 小部件的布局：

```html
<div data-role="layout" data-id="defaultLayout">
  <footer data-role="footer">
    <div data-role="tabstrip">
      <a href="#contactsView" data-icon="about">Contacts</a>
      <a href="#favoritesView" data-icon="favorites">Favorites</a>
      <a href="#settingsView" data-icon="settings">Settings</a>
    </div>
  </footer>
</div>
```

在前面的代码片段中，TabStrip 小部件 (data-role='tabstrip') 包含三个按钮：**Contacts**、**Favorites** 和 **Settings**。每个按钮都指定了 href 属性，其值与页面中定义的视图的 id 属性所赋的值相同。

