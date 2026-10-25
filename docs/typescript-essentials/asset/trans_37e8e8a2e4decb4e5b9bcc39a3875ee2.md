

我们将 jQuery 和 Knockout 的引用包含在全局命名空间中，然后使用一个 script 标签引入 require。require 标签中包含一个名为 `data-main` 的绑定，用于定义应用程序的入口点。

**注意**

有关 RequireJS 的更多信息，请访问 [http://requirejs.org/](http://requirejs.org/)。

在 HTML 的 body 部分，我们有标签和输入框定义了 UI 并绑定到视图模型中的各个对象。搜索结果部分做了一些新的事情，值得注意。使用 `searchResult` 可观察数组上的 `foreach` 绑定，我们为数组中的每个项创建了一个模板。这些绑定的上下文基于正在迭代的项的类型，在我们的例子中是一个 Person 对象。

虽然 `firstName`、`lastName` 和 `age` 属性不是可以绑定的可观察对象，但绑定只会发生一次。如果这些字段的值发生变化，UI 不会被更新。我们需要做的最后一件事是创建应用程序的入口点。我们需要导入创建的视图模型，等待 DOM 就绪，然后应用我们的绑定：

```javascript
import PersonFinderViewModel = require("Scripts/PersonFinderViewModel");
$(document).ready(() => {
    ko.applyBindings(new PersonFinderViewModel.PersonFinderViewModel());
});
```

现在我们可以运行应用程序并向目录中添加多个用户。当我们按名字搜索某人时，所有匹配的结果都将被显示：

![](img/83a7f7e1723158e56008cf70304a8da4_197_0.png)

## 第 7 章. 增强绘图应用程序

当我们在第 5 章 *创建简单的绘图应用程序* 中开始绘图应用程序时，我们按功能将所有代码分组在一起。这使我们能够减少应用程序运行所需的 script 标签数量。利用我们在上一章中学到的知识，我们将修改绘图应用程序以使用 AMD 模块和 RequireJS。我们还将集成 Knockout 和 jQuery 以改善整体用户体验。最后，我们将查看如何使用 RequireJS 将所有应用程序代码构建成单个压缩文件以进行部署优化。我们将对应用程序进行的增强包括：

- 转换为 AMD 模块
- 绑定用户控件
- 生成单个输出文件
- 美化应用程序

### 转换为 AMD 模块

转换为 AMD 模块是一个相当简单的过程。然而，这是一个审视项目结构和可维护性的好时机。在开始之前，我们必须安装 RequireJS NuGet 包版本 2.1.14 以及相应的声明文件 requirejs.TypeScript.DefinitelyTyped 版本 0.2.0。

![](img/83a7f7e1723158e56008cf70304a8da4_202_0.png)

TypeScript 构建设置也必须更改为使用上一章所示的 AMD 模块系统。一旦这些步骤完成，我们就可以开始增强我们的应用程序。为此，我们希望为应用程序添加一个文件夹结构，以帮助我们根据功能轻松分离模块。此时，重要的是决定如何划分应用程序的代码。由于我们使用的是 RequireJS 模块，我们将使用 export 关键字为所有希望从其他模块使用的对象。我们可以使用 export 关键字有三种方式，每种方式对生成的 JavaScript 都有不同的影响。以下示例显示了该关键字的更常见用法：

```
export class Freehand implements IFreehand {
    public points: Array<IPoint> = [];
    constructor() {
    }
    public AddPoint(point: IPoint) {
        this.points.push(point);
    }
}
```

在此示例中，export 关键字注释了类型声明。这告诉 TypeScript 编译器将此类型放置在正在生成的模块的 exports 对象上。

**提示**

export 关键字只有在向编译器提供 `--module` 标志时才可用。尝试在不提供此选项的情况下使用它将导致编译器错误。Visual Studio 在项目属性的 TypeScript Build 部分中选择模块系统时提供此标志。

此类定义的 resulting JavaScript 将与我们之前的模块定义非常相似。然而，由于我们选择使用 AMD 模块，会添加一些特殊语法：

```
define(["require", "exports"], function(require, exports) {
    var Freehand = (function () {
        function Freehand() {
            this.points = [];
        }
        Freehand.prototype.AddPoint = function (point) {
            this.points.push(point);
        };
        return Freehand;
    })();
    exports.Freehand = Freehand;
});
```

AMD 模块和标准 JavaScript 模块之间的差异从定义的最开始就显而易见。AMD 规范提供了一个名为 define 的函数，用于协助模块加载器。虽然这里只显示了该函数的两个参数，但该函数可选地接受一个在生成的 JavaScript 代码中显示的两个参数之前的前置参数。此可选第一个参数是模块的标识符，我们将在本章后面讨论此参数，以便我们想将最终的应用程序代码压缩时。define 函数的第二个参数是模块依赖项列表。我们在项目中用来引用另一个模块的每个 require 语句都将包含在此列表中。第三个也是最后一个参数是我们定义模块的唯一强制参数，它是一个将执行以实例化对象或模块的函数。提供给 define 函数的每个依赖项都将作为参数提供给函数，以便在模块内部使用。

export 关键字也可以分配给一个类型。在以下示例中，你可以看到我们不是用 export 关键字装饰我们的类，而是改用赋值运算符为其赋予新值：

```
class Circle implements ICircle {
    constructor(public radius: number) {
    }
    public resize(radius: number) {
        this.radius = radius;
    }
    public area(): number {
        return Math.PI * this.radius * this.radius;
    }
}
export = Circle;
```

使用此方法将我们限制为从文件中导出单个类型；然而，此类型可以是包含任意数量导出对象的模块。生成的 JavaScript 看起来像以下代码：

```
define(["require", "exports"], function(require, exports) {
    var Circle = (function () {
        function Circle(radius) {
            this.radius = radius;
        }
        Circle.prototype.resize = function (radius) {
```

