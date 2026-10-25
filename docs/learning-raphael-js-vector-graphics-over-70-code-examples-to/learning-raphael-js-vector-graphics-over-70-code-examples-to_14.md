## 矢量动画

到目前为止，我们已经研究了绘制静态图形以及即时与之交互和转换它们。动画使我们能够随时间改变图形，也就是说，我们可以在非零时间间隔内修改矢量图形的属性。

在 2000 年代初期 JavaScript 框架出现之前，动画主要由 Flash 实现，以及在较小程度上由 Java 小程序实现。随着这些框架的出现，DOM 的动画带来了更流畅、更直观的用户界面和更具语义的网站。在近年来，HTML5、CSS3、WebGL 和 SVG 使开发人员能够创建更复杂、功能齐全的动画和交互式用户界面。

Raphaël 使矢量图形动画变得简单，在本章中，我们将探讨使用该库为图形制作动画的不同方式。我们将特别涵盖以下主题：

- Animating 方法和基本属性动画
- 动画路径
- 使用三次贝塞尔语法进行缓动
- 动画变换
- 使用自定义属性进行动画以及沿路径动画

## 基本动画

动画是使用元素的 `animate` 方法完成的。该方法按定义顺序接受以下参数：

| 参数 | 类型 | 描述 |
|---|---|---|
| attributes | Object | 按照 `Element` 的 `attr()` 方法的键值对属性 |
| duration | Number | 动画持续的毫秒数 |
| easing（可选） | String | 描述属性随时间变化率的字符串 |
| callback（可选） | Function | 在动画结束时执行的函数 |

为了演示动画，请考虑一个圆，其中心点在 (100, 100)，半径为 50px：

```
var circle = paper.circle(100, 100, 50).attr({fill: 'green'});
```

我们可以将圆动画到点 (500, 100) 并将半径减小到 15px，如下所示：

```
circle.animate({
 cx: 500,
 r: 15
 }, 1000);
```

圆在一秒钟内动画，其初始状态和最终状态如下所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_76_0.png)

> 要真正欣赏本章中给出的示例，您应该参考本书附带的源代码演示或网站。





同样，位于 (100, 50) 的矩形在 x 和 y 方向上被动画化到点 (400, 100)，同时宽度增加，不透明度降低，描边宽度增大，如下所示：

```javascript
var rectangle = paper.rect(100, 50, 20, 50).attr({
    fill: '#345'
});
rectangle.animate({
    opacity: 0.6,
    x: 400,
    y: 100,
    width: 100,
    'stroke-width': 10
}, 1000);
```

结果如下所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_77_0.png)

## 动画路径

元素的 `path` 属性允许我们像在上一节中动画化属性一样动画化路径。虽然基本形状具有内在的 `x` 和 `y` 或 `cx` 和 `cy` 属性以及宽度和高度，但路径没有，因此我们将从一个路径动画化到另一个自定义路径。

考虑绘制一个五角星，如下所示：

```javascript
var star = paper.path(
    'M 100,40 40,210 190,90 10,90 160,210 z'
);
```

## 矢量动画

五角星是一个线条相互重叠的多边形。当将五角星动画化为阿贝洛斯形状时，这会产生一个有趣的效果。为了演示这一点，我们定义一个正方形，当点击时初始化一个路径动画，如下所示：

```javascript
var square = paper.rect(500, 100, 50, 50).attr({
    'fill': '#09c', cursor: 'pointer'
});
square.click(function() {
    star.animate({
        path: [
            'M', 100, 180,
            'a', 140, 140, 0, 0, 1, 280, 0,
            90, 90, 0, 0, 0, -180, 0,
            50, 50, 0, 0, 0, -100, 0
        ]
    }, 1000);
});
```

运行此代码时，您将看到三角形被平滑地转换为阿贝洛斯形状，如下所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_78_0.png)

Raphaël 网站上有一个非常好的路径动画示例，网址为 http://raphaeljs.com/chart.html。该示例演示了在切换月份时的三次贝塞尔曲线动画。我们可以使用在第 3 章“绘制路径”中遇到的 Catmull-Rom 曲线来实现类似的效果。我们首先定义初始和结束的 Catmull-Rom 路径，并将第一个绘制到画布上：

```javascript
var pathStart = [
    'M', 50, 250,
    'R', 100, 100, 150, 150, 200, 140, 250, 200, 300, 80,
    350, 130, 400, 250,
    'H', 50,
    'z'
];
var pathEnd = [
    'M', 50, 250,
    'R', 100, 130, 150, 220, 200, 200, 250, 100, 300, 200,
    350, 180, 400, 250,
    'H', 50,
    'z'
];
var curve = paper.path(pathStart).attr({
    'stroke-width': 0,
    'stroke-linejoin': 'round',
    'fill': 'green'
});
```

您会注意到我们在每个路径的起点处连接了一条直线。这是因为简单地闭合 Catmull-Rom 曲线将继续沿着路径的曲率，而不会给我们一条直线。点击正方形时，我们将曲线动画化到结束路径，如下所示：

```javascript
square.click(function() {
    curve.animate({
        path: pathEnd
    }, 500);
});
```

结果如下所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_79_0.png)

## 动画缓动

**缓动**描述了属性值随时间变化的方式。默认情况下，属性值在动画过程中均匀变化——即 _线性_ 变化——但通过指定特定的缓动类型，我们可以改变属性的动画方式。请参考下图，该图演示了三种可用的缓动类型：

![](img/d8ec653db4cab13fec8b3f5718174af6_80_0.png)

对于每种缓动类型，属性值的变化率在时间轴上的图表中有所不同。每种缓动类型可以描述如下：

- **线性**：值在动画过程中从起始值一致地变化到结束值
- **缓出**：值快速增加到其终点，然后在动画结束前减速
- **缓入缓出**：值起初缓慢减少，然后快速增加，最后在动画结束前减速到其终点

## 内置缓动公式

Raphaël 拥有多种内置缓动类型，包括上一节中详细介绍的那些，以及诸如 'elastic' 或 'bounce' 等其他类型。每种效果在 http://raphaeljs.com/easing.html 上都有很好的演示，其中圆形的 cx 和不透明度使用不同的缓动类型进行动画化。例如，考虑将 'bounce' 缓动类型应用于我们的 Catmull-Rom 路径动画曲线：

```javascript
square.click(function() {
    curve.animate({
        path: pathEnd
    }, 500, 'bounce');
});
```

在此示例中，属性值为 `path`，它快速动画化到其终点，然后再返回，直到最终停止。路径随时间的变化如下所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_81_0.png)

## 使用三次贝塞尔格式自定义缓动

CSS3 过渡中描述动画缓动的格式可以用于在 Raphaël 中定义自定义缓动路径。语法为 `cubic-bezier(point1_x, point1_y, point2_x, point2_y)`，其中每个参数定义描述从 (0, 0) 到 (1,1) 的三次贝塞尔曲线的锚点的 _x_ 和 _y_ 位置，用于属性值和时间。请考虑描述一个属性值非常急剧增加，然后几乎停止，最后缓入到其终点的缓动路径：

![](img/d8ec653db4cab13fec8b3f5718174af6_81_1.png)

描述此路径的 CSS3 语法为 `cubic-bezier(0.1, 0.97, 0.08, 0.03)`。这是使用 `http://www.roblaplaca.com/examples/bezierBuilder/` 上的一个有用工具生成的，该工具允许您直观地构建自定义缓动路径并获得生成的缓动的 `cubic-bezier` 值。

例如，考虑两个圆形，其中一个使用此缓动在 `cx` 中动画化，而另一个具有线性缓动：

```javascript
circle.animate({
    cx: 450
}, 1000, "cubic-bezier(0.10,0.97,0.08,0.03)");
circle2.animate({
    cx: 450
}, 1000, 'linear');
```

运行此代码时，您将看到第一个圆形启动速度比第二个快得多，但到动画结束时，两个圆形的 `cx` 值均大致呈线性变化。

## 动画变换

与路径一样，元素的变换是元素上的一个属性，因此我们可以以同样的方式进行动画化。例如，考虑一个我们可以在点击时旋转的正方形：

```javascript
square.click(function() {
    this.animate({
        transform: '...r 45'
    }, 300);
});
```

省略号确保每次连续点击都会使正方形从当前位置顺时针旋转 45 度。

同样，我们可以动画化更复杂的变换。例如，考虑依次对正方形进行 x 方向 5 度的剪切：

```javascript
var anim = Raphael.animation({
    transform: [
        '...', 'M', 1, 0, Math.tan(Raphael.rad(5)), 1, 0, 0
    ]
}, 300);
square.click(function() {
    this.animate(anim);
});
```

连续点击正方形会产生相对于 y 轴额外五度的剪切。您会注意到，在此示例中，我们定义了一个 `animation` 对象，然后在点击正方形时将其传递给正方形的 `animate` 方法。这是定义动画的另一种方法，很有用，因为我们可以重复使用它们。

## 使用自定义属性进行动画

Raphaël 通过 **自定义属性** 促进高级动画。自定义属性可用于基于自定义逻辑对特定元素应用或动画化多个属性。

## 自定义属性

自定义属性是一个自定义函数，返回一组要应用于元素的属性。它可以被视为一个辅助函数，其中现有属性基于计算得出。自定义属性定义为 `customAttributes` 命名空间的一个属性，如下所示：

```javascript
Paper.customAttributes.yourAttribute = function(a1, a2, ...){ };
```

这里，`a1`、`a2` 等参数是数字，`yourAttribute` 是自定义属性的名称。然后，自定义属性可以像我们迄今使用的其他元素属性一样使用。

例如，假设我们需要使用以下方式表示人口数据：

- 圆的半径表示相对人口规模
- 圆的颜色表示国家的人口密度

以下数据表示五个国家的相对人口和人口密度：

| 国家 | 人口（0 到 1 之间归一化） | 人口密度（0 到 1 之间归一化） |
|---------|-----------------------------------------|-------------------------------------------------|
| 台湾 | 0.10 | 1.00 |
| 韩国 | 0.32 | 0.50 |
| 荷兰 | 0.05 | 0.22 |
| 比利时 | 0.00 | 0.07 |
| 日本 | 1.00 | 0.00 |





## 矢量动画

我们定义了一个自定义属性 `popDensity`，用于根据上述规则对数据进行可视化表示：

```javascript
paper.customAttributes.popDensity = function(population, density) {
    var radius = 25 + (25 * population),
        fillColor = 'hsb(0,0,' + (1 - density) + ')'; 
    return {
        fill: fillColor,
        r: radius
    };
};
```

此方法定义了一个半径，其值为 25px 加上 25 乘以国家的相对人口数，这意味着人口最多的国家半径为 50px，人口最少的国家半径为 25px，而所有其他国家的半径介于这两个值之间。填充颜色由亮度比例决定，其中色相和饱和度固定。公式 (1 – density) 对于人口密度最低的国家将返回值 1（饱和度呈现白色），对于人口密度最高的国家将返回值 0（饱和度呈现黑色）。0 到 1 之间的所有饱和度值都会产生灰色调。

鉴于我们将数据集定义为对象数组，如下所示：

```javascript
var data = [
    {
        name: 'Taiwan',
        population: 0.1,
        density: 1
    },
    {
        name: 'South Korea',
        population: 0.32,
        density: 0.5
    },
    // ...
];
```

我们可以遍历每个国家，并绘制一个带有 `popDensity` 属性的圆，如下所示：

```javascript
var currentX = 70;
for(var i = 0, ii = data.length; i < ii; i+=1) {
    var country = data[i];
    paper.circle(currentX, 150, 50).attr({
        popDensity: [country.population, country.density],
        'stroke-width': 4
    });
    paper.text(currentX, 225, country.name).attr({
        'font-size': 14
    });

    currentX += 100;
}
```

在循环的每次迭代中，人口密度属性从我们的数据对象中确定并绘制到画布上。我们已将每个圆标记为国家名称文本，并递增 x 坐标，以便后续圆在前一个圆的右侧绘制。结果如下所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_85_0.png)

如您所见，人口最多的国家不一定具有最大的人口密度。虽然台湾和荷兰的人口大致相等，但台湾每平方公里的人口数量远多于荷兰。

如前所述，我们可以将自定义属性从一组值动画化到另一组值。考虑一下，如果我们在循环中初始化元素时将人口和密度值设置为零，然后动画化到人口和密度的实际值，会发生什么：

```javascript
var currentX = 70;
for(var i = 0, ii = data.length; i < ii; i+=1) {
    var country = data[i];
    var circle = paper.circle(currentX, 150, 50).attr({
        popDensity: [0, 0],
        'stroke-width': 4
    });
    circle.animate({
        popDensity: [country.population, country.density]
    }, 1000, 'easeOut');
    paper.text(currentX, 225, country.name).attr({
        'font-size': 14
    });
    currentX += 100;
}
```

运行此代码时，您会注意到半径和填充属性都会动画化到其最终状态。

## 路径动画

在动画过程中，属性值根据指定的缓动和已过的时间而变化。自定义属性的工作方式相同。也就是说，传递给自定义属性函数的值取决于缓动和已过的时间。我们可以利用此功能将元素沿给定路径动画化。

首先，我们绘制一条三次贝塞尔曲线，圆将沿其动画化：

```javascript
var path = paper.path([
    'M', 100, 100,
    'C', 100, 0, 400, 200, 400, 100,
    'S', 100, 200, 100, 100,
    'z'
]).attr({'stroke-width': 2});
var circle = paper.circle(0, 0, 13).attr({
    fill: '#09c', cursor: 'pointer'
});
```

我们的圆初始化为 (0, 0) 点，因为我们将应用一个自定义属性来确定其初始位置。

我们的自定义属性，我们将其命名为 `pathFactor`，它确定圆应该位于路径上的哪个点，具体取决于动画已进行了多少。如果我们将 `distance = 0` 定义为动画的起点，将 `distance = 1` 定义为终点，我们可以将 distance 乘以路径的总长度，以确定圆应该遍历路径的多少长度：

```javascript
paper.customAttributes.pathFactor = function(distance) {
    var point = path.getPointAtLength(distance * pathLength);
    var dx = point.x,
        dy = point.y;
    return {
        transform: ['T', dx, dy]
    };
};
```

我们始终返回一个绝对变换，它使用路径上的 (_x_, _y_) 点来确定圆位于路径上的哪个位置。

为了动画化我们的圆，我们首先将其 `pathFactor` 属性初始化为零。这会将其放置在路径的零长度点，即我们开始绘制路径的点，也就是 (100, 100)：

![](img/d8ec653db4cab13fec8b3f5718174af6_87_0.png)

为了沿路径动画化我们的圆，我们定义并执行一个 `runloop` 函数，该函数将我们的 `pathFactor` 属性动画化到值 1。动画结束的回调函数将 `pathFactor` 重置为零，然后递归调用 `runloop`。您会注意到，我们的递归函数调用是作为 `setTimeout` 的回调执行的；这是为了确保在动画重复之前，浏览器进入循环下一次迭代所需的最短时间已过去：

```javascript
function runloop() {
    circle.animate({pathFactor: 1}, 4000, function() {
        this.attr({pathFactor: 0}); // Reset
        setTimeout(runloop);
    });
}
runloop();
```

执行此代码时，您会看到我们的圆沿着路径永久动画化，花费四秒钟从起点遍历路径并返回。

## 暂停和恢复动画

我们尚未研究暂停和恢复动画的功能。考虑一下，在沿路径动画化的示例中，如果我们在运行循环中为圆定义单击和双击事件处理程序，如下所示：

```javascript
function runloop() {
    circle.animate({pathFactor: 1}, 4000, function() {
        this.attr({pathFactor: 0}); // Reset
        setTimeout(runloop);
    });
    circle.unclick().click(function() {
        this.pause();
    });
    circle.undblclick().dblclick(function() {
        this.resume();
    });
}
runloop();
```

由于每次都会启动新动画，我们在每次动画运行时都会注销并重新注册单击和双击事件处理程序。运行此代码时，您会看到，单击和双击时，`pause` 和 `resume` 方法分别会暂停和恢复圆上当前运行的动画。在动画周期的四分之一处（t = 0.25）或四分之三处（t = 0.75）暂停动画的结果如下所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_88_0.png)

## 摘要

在本章中，我们探讨了驱动动画的概念以及将元素归属于属性的不同方式。您现在应该能够舒适地使用 Raphaël 库创建、转换和动画化矢量图形，并且应该能够毫无困难地利用 Raphaël 文档来基于我们所涵盖的内容进行扩展，并按照自己的意图操作图形。

在下一章中，我们将探讨如何在 Raphaël 中使用现有的 SVG。
