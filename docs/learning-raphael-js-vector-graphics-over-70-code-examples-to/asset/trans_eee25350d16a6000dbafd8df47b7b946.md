

## 创建社交媒体可视化套件

在本节中，我们将创建一个交互式可视化，类似于直方图/面积图，通过将社交网络用户数量与总世界人口（六十亿）进行比较，来展示这些数字有多么显著。它将使用一种常见的技术，即在选择相关社交网络时，通过高亮显示图标来指示用户相对于所有世界公民的比例。最终结果如下所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_108_0.png)

## 开始使用

要开始，我们首先创建一个基本的 HTML 文件，其中包含社交媒体图标图片的标记，并在 `body` 标签闭合之前包含所有相关的 JavaScript 文件。我们将在 `social-network-usage.js` 文件中编写 JavaScript：

```html
<body>
<h1>Social Network Usage</h1>
<p>The ratio of users to the total world population,
based on a world population of 6,000,000,000.</h1>
<div id="world-population-usage"></div>
<div class="networks">
<img class="facebook" src="icons/facebook_32.png" />
<img class="twitter" src="icons/twitter_32.png" />
<img class="flickr" src="icons/flickr_32.png" />
<img class="linkedin" src="icons/linkedin_32.png" />
<img class="myspace" src="icons/myspace_32.png" />
<img class="googleplus" src="icons/google_plus_32.png" />
<img class="orkut" src="icons/orkut_32.png" />
<img class="lastfm" src="icons/lastfm_32.png" />
</div>
<script type="text/javascript"
src="raphael-2.1.0.min.js"></script>
<script type="text/javascript"
src="jquery-1.9.1.min.js"></script>
<script type="text/javascript"
src="social-network-usage.js"></script>
</body>
```

当点击社交媒体图标时，高亮显示的人数将更新以反映该网络的总用户数。这些图标由 komodomedia.com 提供，并随本书附带的源代码一起分发。

## 使用 jQuery

你会注意到在上一节的标记中，我们包含了 jQuery JavaScript 库。这将用于在本章中方便地进行 DOM 交互。

你可以从 http://jquery.com/ 下载 jQuery。编写时的最新版本是 1.9.1，我们将在本章中使用该版本。

对于不熟悉 jQuery 的人，网上有许多资源可供参考。你的首选应该是 http://docs.jquery.com/。

## 社交网络使用数据

本可视化使用的数据取自 http://en.wikipedia.org/wiki/List_of_social_networking_websites，其中使用量基于注册用户数量进行测量，并且该数字四舍五入到最近的 5000 万用户：

| 社交网络 | 注册用户数量（四舍五入到最近的 50,000,000） |
|---|---|
| Facebook | 1,000,000,000 |
| Twitter | 500,000,000 |
| Flickr | 50,000,000 |
| LinkedIn | 150,000,000 |
| Myspace | 50,000,000 |
| Google+ | 400,000,000 |
| Orkut | 100,000,000 |
| last.fm | 50,000,000 |

我们将此数据定义为一个对象，其中键对应于前面标记中定义的类名，值为注册用户数量：

```javascript
var usage = {
    facebook: 1000000000,
    twitter: 500000000,
    flickr: 50000000,
    linkedin: 150000000,
    myspace: 50000000,
    googleplus: 400000000,
    orkut: 100000000,
    lastfm: 50000000
};
```

## 绘制人形图标

像往常一样，我们定义画布并将其引用存储在 `paper` 变量中。根据我们使用的图标的默认宽度和高度，一个宽 770px、高 220px 的画布非常适合我们的目的：

```javascript
var paper = Raphael('world-population-usage', 770, 220);
```

我们使用的人形图标由 Dmitry Baranovskiy 提供，他创建了不少于 266 个作为路径的图标，可在 http://raphaeljs.com/icons/ 上找到。该页面上的每个图标都带有一个相关的路径字符串，我们可以直接在代码中使用。我们将其中一个路径字符串的引用保存在名为 `personPath` 的变量中，以便重复使用（注意，此处显示的路径为了简洁而被截断）：

```javascript
var personPath = "M20.771,12.364c0,0,0.849-3.51 ..."
```

鉴于此，我们现在绘制 120 个这样的图标，每行 20 个，共六行。每个图标代表 5000 万人，这意味着 120 个图标代表 60 亿的世界人口：

```javascript
var people = paper.set();
for(var j = 0; j < 6; j+=1) {
    for(var i = 0; i < 20; i+=1) {
        var path = paper.path(personPath).attr({
            fill: '#666',
            'stroke-width': 0,
            opacity: 0.3
        });
        var bBox = path.getBBox(); // width/height of icon
        var xTranslation = (bBox.width + 4) * i;
        var yTranslation = (bBox.height + 4) * j;
        path.transform('T', xTranslation, yTranslation);
        people.push(path);
    }
}
```

在给定的代码中，我们遍历 i 列和 j 行。我们使用 path 方法绘制图标，然后获取其边界框，以确定其占据的空间宽度和高度。然后我们将图标按此宽度和高度加上每次迭代的 4px 小间距进行平移。我们希望在 x 方向上平移图标的宽度数量由列索引 i 决定，而在 y 方向上平移图标的高度数量由行索引 j 决定：

```javascript
var xTranslation = (bBox.width + 4) * i;
var yTranslation = (bBox.height + 4) * j;
```

最后，我们将创建的人形路径存储在名为 people 的集合中。这将允许我们根据点击的社交媒体图标对所有人进行操作：
people.push(path);

请注意，每个图标的不透明度为 0.3，这将使我们能够区分社交网络用户和构成世界人口的非用户。

## 响应图标点击

我们在标记中定义的每个社交媒体图标上注册一个点击事件处理程序。当点击图标时，我们将对在上一节中创建的 people 集合进行操作：

```javascript
$(' .networks img').click(function() {
    // Operate on people Set
});
```

点击时，我们结束 people 集合中存储的所有路径上的任何现有动画，并将不透明度重置为 0.3。stop 方法防止动画继续到其目标属性，并且是防止未来动画与正在进行的动画冲突所必需的：

```javascript
people.stop().attr({opacity: 0.3});
```

然后我们获取被点击元素的类名，并使用它来获取该特定社交网络的用户数量：

```javascript
var className = $(this).attr('class');
var numPeople = Math.ceil(
  people.length * (usage[className] / 6000000000)
);
```

由于类名映射到我们 `usage` 对象中存储的使用值，我们将使用量计算为总人口的比例，然后将此数字乘以我们 `people` 集合中的人总数。这将给出一个 0 到 120 之间的值，即基于每个图标 5000 万用户应高亮显示的图标数量。`ceil` 方法将此值向上舍入到最近的一个人。

在确定需要高亮显示的图标数量后，我们使用集合对象的 `forEach` 方法遍历它们：

```javascript
people.forEach(function(o) {
  if(o.id < numPeople) {
    o.animate({opacity: 1}, 500);
    return true;
  }
  return false;
});
```

`forEach` 方法将集合中的对象传递给它作为第一个参数接受的函数。该对象有一个 `id` 属性，对应于其在集合中的索引位置。它是一个基于零的 ID，这意味着我们可以在循环期间比较其值，看它是否小于 `numPeople` 变量（即应高亮显示的数量）。如果是，我们将其动画化为不透明度 1，以区别于所有其他不透明度为 0.3 的定义图标。遍历 `people` 集合中所有路径所需的时间可以忽略不计，这意味着所有动画似乎同时发生。例如，当点击 Facebook 图标时，`people` 集合中的前 20 个图标将动画化为不透明度 1。

## 绘制图例

最后，为了识别一个图标相当于 5000 万人，我们重新绘制人形路径并添加一些伴随文本：

```javascript
var keyPerson = paper.path(personPath).attr({
  fill: '#666',
  'stroke-width': 0
});
var bBox = keyPerson.getBBox();
```

