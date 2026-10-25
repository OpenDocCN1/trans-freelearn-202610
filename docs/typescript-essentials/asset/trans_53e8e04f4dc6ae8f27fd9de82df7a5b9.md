

## 添加测试

现在我们已经安装了工具链，让我们开始对库进行单元测试的过程。首先需要做的是将 Shapes 库添加到测试项目中。此过程可以手动完成，也可以使用 PowerShell 自动部署我们代码的最新版本。对于现在，我们将手动处理它，以便我们可以专注于测试。一旦我们的库被添加到项目中，我们就可以开始编写测试。我们将为 Shapes 库中的每个对象创建一个单独的测试类来进行测试。让我们从库中最简单的类 Point 类开始，以感受测试。在下面的示例中，你可以看到我们创建了一个名为 test_Point 的新类，其中包含针对该特定类的所有测试：

```
/// <reference path="../../maxunit.ts" />
/// <reference path="../../scripts/typings/shapes/shapestypes.d.ts" />
class test_Point extends maxUnit.TestClassBase {
    private _shapes: Shapes = null;
    constructor() {
        //give test suite a name
        super('test_Point');
        //add test by name
    }
    setUpAsync(dfdSetup: jQueryDeferred<any>) {
        var cb = super.setUpAsync;
        require(['/Scripts/Shapes/Shapes.js'], (Shapes) => {
            this._shapes = Shapes;
            cb(dfdSetup);
        });
    }
    tearDown() {
        super.tearDown();
    }
}

export = test_Point;
```

该类继承自 maxUnit 基类 TestClassBase，它提供了用于将测试注册到测试框架的方法。构造函数调用基构造函数并提供此组测试的名称。最终，此部分还将包含我们每个单元测试的注册。setUpAsync 方法将用于设置任何必要的模拟对象并创建对 Shapes 库的引用。如果我们的库包含 jQuery 或任何其他依赖项，我们将模拟这些对象以确保我们的库按预期执行。tearDown 方法提供了一个位置，用于在测试运行后执行你可能想要的任何清理逻辑。现在，让我们开始编写一些非常简单的测试来验证 Point 对象的创建。每个单元测试应分为三个部分：

- Arrange
- Act
- Assert

Arrange 部分将创建我们执行测试所需的任何对象，act 将执行

## 欢迎使用 MaxUnit

## Tests:(Total Failed : 0)
(Total Passed: 2)

Test:
ShapeTests/test_Point
(failed: 0) (passed: 2)

## Passed

| Test Name | Test Description | Message |
|---|---|---|
| test_PointInitX | Test Point.X value init | OK |
| test_PointInitY | Test Point.Y value init | OK |

这些都是非常简单的测试，涵盖了创建 Point 对象所使用的最基本路径；然而，在编写单元测试时，我们希望覆盖尽可能多的场景以验证正确的功能。例如，与其只测试特定的类，让我们围绕一个接口编写一些测试，这将创建一系列可以在多个具体实现上成功运行的测试。在下面的示例中，你可以看到我们创建了一组新的测试，将用于测试 IRectangle 接口：

```
/// <reference path="../../maxunit.ts" />
/// <reference path="../../scripts/typings/shapes/shapetypes.d.ts" />
class test_IRectangle extends maxUnit.TestClassBase {
    private _shapes: Shapes = null;
    constructor() {
        //give test suite a name
        super('test_Point');
        //add test by name
        this.AddTest('test_RectangleResize', 'Test resizing of Rectangle');
    }
    setUpAsync(dfdSetup: JQueryDeferred<any>) {
        var cb = super.setUpAsync;
```

```
```typescript
require(['/Scripts/Shapes/Shapes.js'], (Shapes) => {
    this._shapes = Shapes;
    cb(dfdSetup);
});
}
tearDown() {
    super.tearDown();
}
test_RectangleResize() {
    //Arrange
    var rect: IRectangle = new this._shapes.Rectangle(4, 6);
    this.test_IRectangleResize(rect);
}
test_IRectangleResize(r: IRectangle) {
    //Act
    r.resize(3, 9);
    //Assert
    this.Assert.AreIdentical(9, r.width, "Should return 9");
}
}

export = test_IRectangle;
```
```

正如你所见，我们的类结构非常相似；然而，单元测试的各个部分已被分离为两个逻辑部分。第一部分是我们尝试测试的对象的创建，第二部分是我们将要测试的接口。此测试的第二部分可以一遍又一遍地重用于我们创建的每个实现 IRectangle 的类。例如，正方形与矩形非常相似；事实上，我们可以以这样的方式实现它们，使它们具有完全相同的属性集。我们可以非常轻松地向我们的 Shapes 库添加一个新类来表示正方形：

```
```typescript
export class Square implements IRectangle {
    constructor(public height: number, public width) {
        this.width = this.height;
    }
    public resize(height: number, width: number) {
        this.height = height;
        this.width = height;
    }
}
```
```

正如你所见，这个类同时维护了 IRectangle 接口和正方形的概念，通过确保 width 和 height 属性被设置为相同的值。我们现在可以在 test_IRectangle 类中编写第三个测试方法，该方法将实例化一个新的 Square 对象并将其通过用于 Rectangle 对象的 resize 测试：

```
```typescript
test_SquareResize() {
    //Arrange
    var square: IRectangle = new this._shapes.Square(4, 6);
    this.test_IRectangleResize(square);
}
```
```

通过使用我们的接口抽象，我们的测试成为可重用的代码单元，可用于多种

对象类型。一起运行这些测试会产生以下输出：

## Tests:(Total Failed : 1)
(Total Passed: 3)

- Test:
ShapeTests/test_Point
(failed: 0) (passed: 2)

- Test:
ShapeTests/test_IRectangle
(failed: 1) (passed: 1)

## Failed

| Test Name | Test Description | Message |
| test_SquareResize | Test resizing of Squares | areIdentical failed when passed expected: {number} "9" and actual:{number} "3" Should return 9 |

## Passed

| Test Name | Test Description | Message |
| test_RectangleResize | Test resizing of Rectangle | OK |

正方形测试失败了。接口已实现，因此我们的代码能够编译；然而，这暴露了我们对正方形的假设存在问题。通过我们的实现，我们将正方形的行为强加给了 IRectangle 类型。此测试暴露了我们破坏了我们在第 4 章《使用 TypeScript 进行面向对象编程》中讨论的里氏替换原则的事实。我们无法安全地用另一个 IRectangle 对象替换一个 IRectangle 对象，这可能导致意外的行为。

## Summary

在本章中，我们讨论了 TypeScript 开发的最后几个部分。我们讨论了源映射以及 TypeScript 如何使用它们将输出 JavaScript 文件映射回原始 TypeScript 文件，使我们能够在编译前逐步执行代码。我们查看了调试代码的不同环境，无论是在浏览器中还是在 IDE 中。最后，我们查看了测试驱动开发以及它如何以可重用和一致的方式让我们捕获错误和意外功能。我们在短时间内讨论了大量主题：

- Working with the compiler
- TypeScript’s language constructs
- Object-oriented development with TypeScript
- Asynchronous module definitions
- Integrating with external libraries
- Test-driven development

我们使用这些不同的主题构建了一个绘图应用程序，并在需求变化和知识增加时对其进行了改进。你现在有能力在 TypeScript 中启动一个新应用程序，并将其从初始需求带到抛光的最终产品。

## B

- build options / [Build options](#)

