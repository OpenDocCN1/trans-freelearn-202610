

## 运算符重载

JavaScript 本质上不是一种面向对象的语言，这在某种程度上限制了我们的能力，因此与 C# 等语言相比，我们在某些方面仍然受到限制。JavaScript 中无法实现运算符重载，因此我们不得不为类实现特定的方法来实现我们想要的功能。在下面的示例中，我们定义了一个 Vector 类型，它允许我们执行向量数学运算：

```
interface IVector {
    x: number;
    y: number;
}
class Vector implements IVector {
    public x: number = 0;
    public y: number = 0;
    constructor(x = 0, y = 0) {
        this.x = x;
        this.y = y;
    }
}
```

我们知道执行向量加法相当简单，因此如果我们能用 + 运算符将这个类的两个实例相加，那就太好了。

```
var v1 = new Vector(2, 5);
var v2 = new Vector(4, 3);
var v3 = v1 + v2;
alert(v3);
```

然而，由于 JavaScript 中不支持运算符重载，最终结果将与下面截图中的情况类似：

![](img/83a7f7e1723158e56008cf70304a8da4_142_0.png)

在幕后，JavaScript 引擎会调用与对象关联的 toString 方法，然后将它们拼接成一个单独的值。这有可能在我们的应用程序中导致意外结果。为了实现我们想要的功能，我们必须显式实现方法来处理所需的功能。

```
public add(v: IVector): IVector {
    var newVector = new Vector();
    newVector.x = this.x + v.x;
    newVector.y = this.y + v.y;
    return newVector;
}
```

```javascript
public static add(v1: IVector, v2: IVector): IVector {
    return v1.add(v2);
}
```

在将这些方法添加到我们的类定义中，并对 `toString` 进行方法重写之后，我们只需修改应用程序代码来调用新的 add 方法，即可获得预期的结果：

```javascript
var v3 = Vector.add(v1, v2);
alert("X: " + v3.x + ", Y: " + v3.y);
```

最终结果将是一个新的 Vector 对象，它是两个向量的和：

![](img/83a7f7e1723158e56008cf70304a8da4_143_0.png)

## 方法重写

方法重写是一种替换从超类继承的功能的方式。方法重写必须与被重写的方法具有相同的调用签名。请看下面的示例：

```typescript
interface IEmployee {
    name: string;
    email: string;
    work(tasks: string[]);
}
class Employee implements IEmployee {
    constructor(public name: string, public email: string) {
    }
    public work(tasks: string[]) {
        for (var i = 0; i < tasks.length; i++) {
            //perform task
        }
    }
}
interface IManager extends IEmployee {
    employees: IEmployee[];
}
class Manager extends Employee implements IManager {
    public employees: IEmployee[] = [];
    constructor(name: string, email: string, employees: IEmployee[]) {
        super(name, email);
        this.employees = employees;
    }
    public work(tasks: string[]) {
        for (var i = 0; i < this.employees.length; i++) {
            for (var j = 0; j < tasks.length; j++) {
                this.employees[i].work([tasks[j]]);
            }
        }
    }
}
```

在这个示例中，你可以看到我们有两个类。首先，一个通用的 Employee 类，它代表了组织中任何员工的基类。该员工拥有一些属性，例如姓名和用于联系他们的电子邮件地址。其次，有一个非常简单的方法，允许向员工传递一个任务列表，该任务列表将按顺序执行，直到所有任务完成。下一个类是 Manager 类，它是 Employee 的子类。Manager 拥有与普通员工相同的所有功能，只是他们还拥有一个自己负责的员工列表。Manager 会收到一个任务列表，而不是显式地执行每个任务，它会把每个任务委托给其下属员工去执行。如果我们从 work 方法中添加或删除参数，那么子类和超类之间的调用签名就会不同，从而违反里氏替换原则。TypeScript 不会阻止你更改方法签名，但这会让你的代码更难维护。

## 参考资料

要了解更多关于面向对象设计和面向对象编程概念的信息，请访问：

- http://www.objectmentor.com/omSolutions/oops_what.html
- http://www.codeproject.com/Articles/22769/Introduction-to-Object-Oriented-Programming-Concepts

## 总结

通过本章的学习，你应该已经对面向对象编程有了基本的了解。我们已经通过几个示例了解了如何将封装、多态、抽象和继承付诸实践，以及它们如何让我们构建可扩展的应用程序。SOLID 原则帮助我们规划了类应该如何设计。接下来，我们将在后续几章中将这些概念付诸实践。我们将在[第5章](Chapter 5)*创建简单的绘图应用程序*中，使用 TypeScript 和少量 HTML 来开发一个交互式绘图应用程序。

## 第5章 创建简单的绘图应用程序

在很短的时间内，我们已经涉及了大量的信息。让我们把这些信息运用起来；让我们构建一个完整的应用程序，而不是仅仅查看代码示例。我们将构建一个类似于“画图”的基于 Web 的绘图应用程序，但它将托管在浏览器中。本章将涵盖的主题包括：

- 创建项目
- 基本形状
- 在画布上绘制对象
- 管理画布状态与决策
- 跟踪应用程序状态

### 设置项目

首先，我们需要创建一个新项目来托管我们的应用程序，并配置编译器设置。在 Visual Studio 中创建一个新项目，使用[第2章](#)*TypeScript 基础*中描述的 TypeScript 项目模板，并将其命名为 `DrawingApplication`。然后，右键单击项目并选择**属性**，打开项目属性。

在下面的屏幕截图中，你可以看到我们将用于此应用程序的设置：

![](img/83a7f7e1723158e56008cf70304a8da4_151_0.png)

正如你所见，我们将以 ECMAScript 3 为目标，以确保我们的应用程序能在更广泛的平台上运行。我们不会使用外部模块系统，注释将从最终输出中移除，其余输出可以保留默认值。接下来，我们需要修改 `index.html` 以包含一个 canvas 元素。HTML canvas 元素是 HTML5 标准的一部分，目前大多数现代浏览器都支持它。由于我们还没有使用 jQuery，我们还需要将 `app.js` 文件的加载移到 body 内部；这样，canvas 元素在运行时就可以供我们使用了。下面的屏幕截图显示了进行这些修改后的 `index.html` 文件：

![](img/83a7f7e1723158e56008cf70304a8da4_152_0.png)

现在我们已经完成了项目的基础设置，让我们开始构建一个面向对象的 TypeScript 应用程序。在本章中，我们将使用几个枚举来帮助定义抽象，并让我们的类型以强类型的方式进行通信。我倾向于保持代码的清晰分离，以便于导航，因此创建的所有枚举都将放在一个名为 Enums.ts 的文件中。这两个枚举如下：

```
enum DrawingToolType {
    Select,
    Rectangle,
    Circle,
    Line,
    Freehand
}
enum CanvasEngineAction {
    None,
    Move,
    Resize
}
```

DrawingToolType 类型将用于帮助确定用户选择了哪个绘图工具。负责与画布交互的类型需要知道这一点才能正常运行。CanvasEngineAction 类型将由负责绘制形状的对象使用，以告知画布操作者如何处理用户的输入。

