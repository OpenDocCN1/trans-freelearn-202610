

```
ShapeType[ShapeType["Rectangle"] = 3] = "Rectangle";
ShapeType[ShapeType["Circle"] = 4] = "Circle";
ShapeType[ShapeType["Line"] = 5] = "Line";
ShapeType[ShapeType["Freehand"] = 6] = "Freehand";
ShapeType[ShapeType["Random"] = GetEnumValue()] = "Random";
})(ShapeType || (ShapeType = {}));
```

如你所见，提供给计算成员的表达式被直接构建到生成的枚举声明中。该表达式将在运行时计算，但仅在 ShapeType 初始化时执行。一旦表达式运行完毕，其结果将保留为 ShapeType 对象的成员。因此，在前面的示例中，只要 ShapeType 枚举驻留在内存中，ShapeType.Random 就等价于 ShapeType["Random"]。现在你可能会想，能够改变常量成员的数值序列有什么用处呢？

到目前为止，关于枚举我们还没有涉及的一点是，枚举定义可以被合并。如果将两个枚举声明放在代码的不同片段中并使用相同的类型名称，它们将作用于同一个对象。你可以在生成的 JavaScript 中枚举声明的最后一行看到这一点。如果该对象已存在，则每个新属性都会被添加到已有对象上；否则，会创建一个新对象并应用这些属性。

## 模块

模块模式在 JavaScript 开发中变得越来越流行。它提供了一种简单的方法来封装私有成员（这是 JavaScript 本身不具备的能力），并将对象隔离在全局命名空间之外。在 TypeScript 中有两种类型的模块：**内部模块**和**外部模块**。我们稍后会详细讨论它们，但暂时先只谈内部模块。

现在我们有一个内部模块，它包含两个可访问的命名类型：ShapeType 枚举和 Shape 类，以及一个不可访问的变量 origin，该变量属于模块的隔离状态。尽管我们未必需要访问 ShapeType 枚举，但它仍然必须被导出，否则代码无法编译。任何在另一个成员定义中使用的命名类型，都必须具有与该成员相同的可访问性。如果不能满足这一要求，编译将失败。访问这些类型的方式是先提供模块名称，然后是你要访问的类型。在下面的示例中，你可以看到我们创建一个新的 Shape 对象并设置其类型：

```
var shape = new Shapes.Shape();
shape.Type = Shapes.ShapeType.Circle;
```

为这个模块生成的 JavaScript 看起来与为枚举类型生成的代码非常相似。模块的所有成员都被包裹在一个闭包中，该闭包在模块加载时运行。每个导出的类型都被放置到传递给闭包的实例变量上，以便任何访问模块的人都能使用。模块也拥有与枚举相同的合并能力。这意味着你可以将代码拆分为多个文件，并在运行时将它们合并为一个对象。这种模块实现的一个缺点是无法为模块创建接口。然而，能够以你选择的任何方式组织代码，比接口能为模块带来的任何功能都更有价值。下面你可以看到 TypeScript 为定义模块而生成的 JavaScript：

```
var Shapes;
(function (Shapes) {
    var origin = new Point(0, 0);
    (function (ShapeType) {
        ShapeType[ShapeType["Rectangle"] = 3] = "Rectangle";
        ShapeType[ShapeType["Circle"] = 4] = "Circle";
        ShapeType[ShapeType["Line"] = 5] = "Line";
        ShapeType[ShapeType["Freehand"] = 6] = "Freehand";
    })(Shapes.ShapeType || (Shapes.ShapeType = {}));
    var ShapeType = Shapes.ShapeType;
    var Shape = (function () {
        function Shape() {
            this.Type = 3 /* Rectangle */;
            this.Bounds = new Bounds();
            this.Bounds.Location = origin;
        }
        return Shape;
    })();
    Shapes.Shape = Shape;
})(Shapes || (Shapes = {}));
```

到目前为止，我们只看到了一个模块的小示例，但不妨想象一下构建一个包含大量对象类型的大型应用程序。你会希望创建命名空间来组织代码，并在互不相关的代码段之间建立一层隔离。在 TypeScript 中有两种提供模块命名空间的方式：可以将模块相互嵌套，也可以将它们作为模块声明的一部分来定义。

让我们来看看定义命名空间和创建模块定义的一些不同方式。

```typescript
module Animals {
    export module Reptiles {
        export var snake = "snake";
    }
}
module Animals.Mammals {
    export var monkey = "monkey";
}
```

在本例中，我们为 Animals 模块创建了一个声明，然后在其中嵌套了另一个被导出的模块声明。此外，还有一个模块声明使用了以 . 字符分隔的多部分类型名称。这里展示的两种模块声明都是有效的。两种声明生成的 JavaScript 输出完全相同，因此选择哪种风格实际上取决于个人偏好。编译器会像开发环境一样将它们视为等价。正如下面的截图所示，访问 Reptiles 和 Mammals 命名空间的方式完全一致：

![](img/83a7f7e1723158e56008cf70304a8da4_93_0.png)

这些模块定义各自生成的 JavaScript 在结构上是相同的。然而，如果你打算在单个 TypeScript 文件中声明多个嵌套模块，最好使用展开式的嵌套声明，而不是使用快捷方式。编译器在生成 JavaScript 输出时不会尝试优化和合并模块定义。正如下面的 JavaScript 代码所示，你将会不必要地创建多个闭包来向类型对象添加对象。如果你打算按文件来划分命名空间，那么这种情况是不可避免的。

```javascript
var Animals;
(function (Animals) {
    (function (Reptiles) {
        Reptiles.snake = "snake";
    })(Animals.Reptiles || (Animals.Reptiles = {}));
    var Reptiles = Animals.Reptiles;
})(Animals || (Animals = {}));
var Animals;
(function (Animals) {
    (function (Mammals) {
        Mammals.monkey = "monkey";
    })(Animals.Mammals || (Animals.Mammals = {}));
    var Mammals = Animals.Mammals;
})(Animals || (Animals = {}));
```

可能有一些常见的对象类型或实用方法，你希望在另一个模块中能够快速访问。随着我们继续合理地对类型进行拆分，让我们创建一个用于绘图相关功能的新模块。这个模块将包含我们之前创建的 Point 类和 Bounds 类。这两个类都将被导出，因为其他模块需要能够引用它们。虽然将代码分隔到易于管理的隔离段中是一件好事，但现在每当我需要使用另一个模块中的类型时，我都必须提供完全限定名称。幸运的是，TypeScript 提供了一个与 export 关键字相反的关键字，允许我们为另一个模块的成员创建别名。import 关键字会为其他模块的导出类型创建新的局部引用。在下面的代码段中，我们将在 Shapes 模块内为 Bounds 类创建一个别名。这样我们就不必修改 Shape 类的定义，使其包含 Bounds 类型的完整命名空间。

```
module Shapes {
    import Bounds = Drawing.Bounds;
    export enum ShapeType {
        Rectangle = 3,
        Circle,
        Line,
        Freehand,
    }
    export interface IShape {
        Type: ShapeType;
        Bounds: Drawing.IBounds;
    }
    export class Shape {
        public Type: ShapeType = ShapeType.Rectangle;
        public Bounds: Drawing.IBounds = new Bounds();
    }
}
```

如你所见，这个别名既可以用作类型注解，也可以用作对象类型。生成的 JavaScript 只是将 import 关键字替换为 var，并让运行时引擎创建对导出成员的引用。但是，直接在 TypeScript 中这样做会生成编译错误。import 关键字会告知编译器不仅要检查被别名化的对象，还要检查其类型信息，以便将其用于类型声明中。

### 提示

export 关键字和 import 关键字可以结合使用，以创建可从外部访问的别名。

