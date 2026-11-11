
![](img/1b8cd151c9f278555f9cf255f20d244e_307_0.png)

## 发生了什么？

我们刚刚演示了如何在创建样式时使用属性替换。根据属性更改要素的点半径是一个相当常见的实践，但正如你所见，我们并不仅限于少数几个属性——我们可以使用属性替换来更改任何属性。这可以非常强大，我们将在本书的其余部分中持续使用它。然而，属性替换只是冰山一角。

## 规则和过滤器

为了获得更精细的控制，我们可以通过使用 **Rule** 和 **Filter** 类来设置对象的样式。这两个类允许我们根据我们选择的任意“规则”对不同的要素应用不同的样式。我们可以使用要素的 `attributes` 属性来应用某些样式，如果一个（或多个）属性匹配我们选择的值。

## 我们如何遵循规则？

**规则** 允许我们在某些条件发生时应用符号化器。这些条件通过 **Filter** 类来处理。我们可以拥有多个规则和过滤器，甚至可以使用逻辑运算符（如 OR 和 AND）与它们配合使用。不过，在谈论这一点之前，有两种方式可以使用规则。

第一种方法是使用 StyleMap 类的 `addUniqueValueRules` 方法。这允许简单的属性匹配，使用起来相当直接。我们将首先介绍这种方法。

第二种方法，使用自定义规则和过滤器对象，有点复杂，但允许我们对应用哪些样式进行雪花级别的控制。在讨论完如何使用 addUniqueValueRules 之后，我们将介绍这一点以及 Rule 和 Filter 类本身。

## 使用 addUniqueValueRules

使用规则最简单的方法是使用 StyleMap 类的 addUniqueValueRules 方法。它允许我们进行某种值查找，并根据属性的值应用不同的样式。更具体地说，假设我们有一堆代表定居点的点要素。每个要素都有一个名为 settlement_type 的属性，其值例如为 'village'、'city' 或 'metropolis'。

现在，假设我们还想根据定居点的类型以不同的方式设置点的样式。我们希望以不同的颜色着色，使点半径更大，并更改描边大小。理论上，我们只需使用属性替换并将这些值嵌入每个要素即可，但更优雅且易于维护的方法是使用 addUniqueValueRules。

使用这种方法，我们可以创建三个不同的符号化器，并将适当的样式应用于不同的定居点类型。例如，如果 settlement_type 属性的值匹配 'village'，它将获得某种样式，如果匹配 'city' 则会收到不同的样式，等等。如果这个解释令人困惑，不用担心，我们将在示例中简要介绍。

## 调用 addUniqueValueRules 函数

首先，让我们看看调用 addUniqueValueRules 函数的一般语法：

```
style_map_object.addUniqueValueRules(intent, property, symbolizer_lookup, context);
```

## intent 参数

intent 参数指定所需意图——通常为 'default'。

## property 参数

第二个参数 property 是你想要检查的要素的属性——例如，我们之前解释中的 'settlement_type'。

## symbolizer_lookup 参数

symbolizer_lookup 参数是一个匿名对象，包含在规则匹配时使用的键值对。键指定传入属性的一个值，而值指定如果属性匹配该值时要应用的符号化器。看看它在代码中出现的样子可能会更清楚一些，例如：

```
var symbolizer_lookup = {
  'village': {pointRadius:5, strokeWidth:2},
  'city': {pointRadius:10, strokeWidth:4}
}
```

这将在要素的 settlement_type 属性为 'village' 时应用 'village' 符号化器，并在属性为 'city' 时应用 'city' 样式符号化器。符号化器将扩展 StyleMap 对象已关联的任何默认符号化器。最后，也是重要的是，如果你搜索的属性包含 symbolizer_object 中不存在的值，该要素将不会收到样式。因此，最好在你知道可以从属性值中期望什么时使用此函数——如果不知道，你可以创建自定义规则，我们将在下一部分介绍。

## context 参数

最后一个参数 context 是一个可选的（通常不包含）对象，指定要检查属性的匿名对象。如果未传入 context，则默认使用 features.attributes（通常这就是你希望发生的情况）。

## 动手实践 – 使用 addUniqueValueRules

现在我们了解了 addUniqueValueRules，让我们看看它在实际中的使用。

- 1. 我们将使用一个包含一些随机生成要素和 settlement_type 属性的基础矢量图层。添加一个 WMS 图层。然后，我们需要一个矢量图层：
  ```
  vector_layer = new OpenLayers.Layer.Vector('Settlement Vector Layer');
  map.addLayer(vector_layer);
  ```
- 2. 现在，让我们创建一个匿名对象，以整数值作为键，以我们期望的 settlement_type 值作为值。我们这样做是因为我们需要一种方法来随机选择要素应为哪种类型的定居点。
  ```
  var settlement_values = {
    0: 'hut',
    1: 'village',
    2: 'city',
    3: 'metropolis',
    4: 'facebook'
  }
  ```
- 3. 好的，现在让我们创建 20 个随机点。使用与之前示例类似的代码，我们将创建二十个随机点，并使用我们刚刚创建的 settlement_values 对象为每个要素分配一个随机定居点类型。
  ```
  for(var i=0; i<20; i++){
      vector_layer.addFeatures([new OpenLayers.Feature.Vector(
          new OpenLayers.Geometry.Point(
              (Math.floor(Math.random() * 360) - 180),
              (Math.floor(Math.random() * 180) - 90)
          ),
          {
              'settlement_type': settlement_values[(Math.floor(Math.random() * 5))]
          }
      )]);
  }
  ```
- 4. 接下来，我们需要创建一个 StyleMap 对象。这次我们不会将任何样式对象应用于任何意图，我们只需将其留空。请记住——如果你只是将一个空的 StyleMap 对象添加到矢量图层，你仍然会看到默认的图层样式，因为我们尚未覆盖意图使用的符号化器。
  ```
  var vector_style_map = new OpenLayers.StyleMap({});
  ```
- 5. 我们几乎准备好调用 addUniqueValueRules 方法了。不过，在这样做之前，我们需要创建一个包含为每个找到的值使用的符号化器的对象。因此，我们需要创建一个包含 settlement_type 中每个值的符号化器的对象。此对象将是我们之前讨论的 symbolizer_lookup 对象。让我们继续为我们之前创建的 settlement_values 的所有值设置符号化器（如果愿意，可以在此处定义自己的样式）：
  ```
  var symbolizers_lookup = {
      'hut': {
          'fillColor': '#ababab', 'fillOpacity':.8, 'pointRadius':4,
          'strokeColor': '#454545', 'strokeWidth':2
      },
      'village': {
          'fillColor': '#FFFA93', 'fillOpacity':.8, 'pointRadius':8,
          'strokeColor': '#AFAB57', 'strokeWidth':4
      },
      'city': {
          'fillColor': '#aaee77', 'fillOpacity':.8, 'pointRadius':12,
          'strokeColor': '#669933', 'strokeWidth':5
      },
      'metropolis' : {
          'fillColor': '#BD1922','fillOpacity':.8,  'pointRadius':16,
          'strokeColor': '#812B30', 'strokeWidth':6
      },
      'facebook': {
          'fillColor': '#336699','fillOpacity':.8,  'pointRadius':26,
          'strokeColor': '#003366', 'strokeWidth':2
      }
  }
  ```
- 6. 呼——现在我们可以调用 `addUniqueValueRules` 函数了。我们将使用 `'default'` 意图，因为我们希望这些样式默认应用。我们想要使用的属性是 `'settlement_type'` 属性。最后，我们将传入上面创建的 `symbolizer_lookup` 对象。

我们在 `symbolizer_lookup` 对象中使用的符号化器也将扩展已应用于 StyleMap 的任何样式；然而，在这种情况下，我们尚未定义要使用的样式对象，因此符号化器将仅扩展默认样式。让我们按如下方式调用函数：
  ```
  vector_style_map.addUniqueValueRules('default', 'settlement_type', symbolizers_lookup);
  ```
- 7. 最后，我们需要将 StyleMap 对象连接到我们的矢量图层。我们只需将矢量图层设置为使用 `vector_style_map` 对象：
  ```
  vector_layer.styleMap = vector_style_map;
  ```
