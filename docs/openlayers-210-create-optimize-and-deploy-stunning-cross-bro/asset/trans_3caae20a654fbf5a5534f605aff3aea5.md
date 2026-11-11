
8. 你现在应该在地图上看到类似以下的内容。如果某些点类型缺失，不用担心！由于点是随机生成的，有时你不会看到某些定居点类型——刷新地图会生成新的点：

![](img/1b8cd151c9f278555f9cf255b20d244e_312_0.png)

## 刚才发生了什么？

我们刚刚使用了一个创建规则的“快捷方式”。这并不意味着你应该避免这样做。该方法相当快，工作效果良好，而且易于阅读和理解。只要记住，如果你没有为找到的值定义 symbolizer，包含该值的要素可能不会有样式。如果你需要对已知值进行这种属性值检查，`addUniqueValueRules` 是一个很好的解决方案。

然而，有一种更强大的自定义样式的方法——使用自定义 Rules 和 Filters。

## 规则和过滤器

到目前为止，我们基本上只是对属性进行一对一匹配来指定样式。使用属性替换，我们直接替换一个值。使用 `addUniqueValueRules`，我们检查属性是否等于某个值。到目前为止，我们的灵活性并不大。

通过使用 Rules 和 Filters，我们可以拥有想要的灵活性。我们可以对属性进行比较，例如，如果要素的属性小于或大于某个值，则以某种方式对其进行样式设置。我们可以创建过滤器来检查多种不同类型的比较（大于、小于、等于等），并且可以让过滤器相互作用。

## 它们如何工作？

filters 对象属于 rule 对象。rule 对象属于 style 对象。每个 style 对象可以有多个规则，并且规则可以根据其过滤器的定义方式相互作用。

## 我们如何使用它们？

要使用自定义规则和过滤器，我们只需要将它们与 style 对象关联起来。定义规则并将其添加到 style 对象的通用语法是：

```
var my_rule = new OpenLayers.Rule({
  filter: new OpenLayers.Filter({}),
  symbolizer: { key:values }
});
style_object.addRules([my_rule]);
```

filter 对象决定规则如何应用（例如检查属性是否大于某个值）。我们将在示例之后更详细地介绍 Filter 类，因为我们可以用过滤器做很多复杂的事情。这可能听起来有点抽象，所以让我们先通过一个示例看看它是如何工作的，然后再详细介绍这两个类。

## 动手实践 – 使用规则和过滤器

让我们使用规则和过滤器根据要素对象的属性为其分配不同的样式。

- 1. 让我们开始使用规则和过滤器。向地图添加一个 WMS 层。我们还需要一个矢量层，所以添加一个矢量层：
- 2. 接下来，我们需要一些要素。我们将再次生成一些随机要素点，并为每个对象创建一个名为 population 的属性，其中包含 0 到 2000 之间的随机数：

```
vector_layer = new OpenLayers.Layer.Vector('Basic Vector Layer');
map.addLayer(vector_layer);
```

```
for(var i=0; i<20; i++){
  vector_layer.addFeatures([new OpenLayers.Feature.Vector(
new OpenLayers.Geometry.Point(
    (Math.floor(Math.random() * 360) - 180),
    (Math.floor(Math.random() * 180) - 90)
),
{
    'population': Math.floor(Math.random() * 2000)
}
]);
}
```

3. 现在我们需要创建一个 style 对象——规则直接应用于 style 对象，过滤器直接应用于规则。所以，让我们按顺序创建对象，从 style 对象开始：

```
var vector_style = new OpenLayers.Style();
```

4. 现在让我们创建三个规则。对于每个规则，我们将传入一个包含两个键的对象：filter（将是一个 Filter 类对象）和 symbolizer（是熟悉的 symbolizer 对象）。filter 对象需要一个过滤器类型，我们将在此示例之后介绍，以及规则将匹配的属性（在本例中为 population）。我们还需要指定要匹配的每个规则的值（或值范围）。value 参数取决于过滤器类型，我们将使用 value 或 lowerBound 和 upperBound。让我们创建第一个规则，该规则将处理小于 500 的人口值：

```
var rule_pop_low = new OpenLayers.Rule({
    filter: new OpenLayers.Filter.Comparison({
        type: OpenLayers.Filter.Comparison.LESS_THAN,
        property: 'population',
        value: 500
    }),
    symbolizer: {
        fillColor: '#ababab', fillOpacity:.8,
        pointRadius:8, strokeColor: '#454545',
        strokeWidth:2
    }
});
```

5. 现在我们来处理第二个规则，该规则处理下边界为 500、上边界为 1500 之间的人口值。边界是包含的；也就是说，500 和 1500 的值将被匹配。请注意，我们没有像前一个规则那样的 value 属性——这里的值由两个边界属性决定：

```
var rule_pop_mid = new OpenLayers.Rule({
    filter: new OpenLayers.Filter.Comparison({
        type: OpenLayers.Filter.Comparison.BETWEEN,
        property: 'population',
        lowerBoundary:500,
        upperBoundary:1500
    }),
    symbolizer: {
        fillColor: '#aaee77', fillOpacity:.8,
        pointRadius:14, strokeColor: '#669933',
        strokeWidth:5
    }
});
```

6. 我们的最后一个规则将匹配大于 1500 的人口值：

```
var rule_pop_high = new OpenLayers.Rule({
    filter: new OpenLayers.Filter.Comparison({
        type: OpenLayers.Filter.Comparison.GREATER_THAN,
        property: 'population',
        value: 1500
    }),
    symbolizer: {
        fillColor: '#BD1922', fillOpacity:.8,
        pointRadius:20, strokeColor: '#812B30',
        strokeWidth:5
    }
});
```

7. 几乎完成了——现在我们需要将这三个规则添加到 style 对象。我们将规则对象的数组传递给 addRules 方法：

```
vector_style.addRules([rule_pop_low, rule_pop_mid, rule_pop_high]);
```

8. 现在创建一个 styleMap 对象并将 style 对象应用到它，并设置矢量层使用它：

```
var vector_style_map = new OpenLayers.StyleMap({
  'default': vector_style
});
vector_layer.styleMap = vector_style_map;
```

9. 现在查看地图。你应该会看到类似这样的内容，但点的位置和颜色不同，因为值是随机化的：

![](img/1b8cd151c9f278555f9cf255b20d244e_316_0.png)

## 刚才发生了什么？

我们刚刚展示了规则和过滤器的使用方法。我们使用了三个 rule 对象，每个对象都有不同的 filter 对象。filter 对象的 type 属性决定了将应用的比较类型。因此，在前面的示例中，如果一个要素的 'population' 属性值小于 500，则 rule_pop_low 规则将被应用于它。现在我们对它的使用有了基本了解，让我们更仔细地看看这两个类。

## OpenLayers.Rule 类

当你想要非常具体地指定应用于要素对象的样式类型时，最好的方法是使用 Rule 类。当你创建一个 rule 对象时，你指定一个 Filter 对象、如果规则匹配则应用于要素的 symbolizer，以及一个可选的 name 属性，该属性指定规则的任意名称。

Rule 对象应用于 style 对象，你可以在一个 style 对象上拥有几乎任意数量的规则。创建 rule 对象的语法，如前所述，是：

```
var my_rule = new OpenLayers.Rule({
    filter: new OpenLayers.Filter({}),
    symbolizer: { key:values }
});
style_object.addRules([my_rule]);
```

正如我们在前面的示例中看到的，为 style 对象创建和应用多个规则并不困难。在前面的示例中，我们为每个 rule 对象只使用了一个过滤器，但 OpenLayers 使指定多个过滤器变得很容易，从而让你对要素样式有更多控制。

规则是根据过滤器使用的——过滤器负责确定规则是否应用于要素。所以，让我们看看 Filter 类，以及如何使用它来进一步自定义要素样式。

## OpenLayers.Filter 类

Filter 类控制匹配要素属性的逻辑，以查看是否应应用规则。我们可以使用单个过滤器，如前面的示例中所示，也可以将过滤器组合在一起（例如，如果 property1 大于 500 且另一个 property2 小于 200，则应用样式）。

过滤器也可以是空间性的——我们可以检查要素是否与几何对象相交、是否在某一点的某个距离范围内等等。为了完成所有这些操作，我们使用基 Filter 类的子类，类似于我们使用 Layer 类和其他类的子类的方式。

## Filter 子类

这里我们将介绍四个子类——Comparison、FeatureId、Logical 和 Spatial。

## Filter.Comparison

Comparison 类是我们到目前为止一直使用的过滤器类。顾名思义，该类将根据过滤器对象的指定参数比较属性。应用的比较类型由 type 属性指定。我们还必须指定要检查的值——大多数过滤器使用 value 属性（但有些使用不同的属性，例如 lowerBoundary 和 upperBoundary）。最后，我们还必须指定要素中要检查的属性。
