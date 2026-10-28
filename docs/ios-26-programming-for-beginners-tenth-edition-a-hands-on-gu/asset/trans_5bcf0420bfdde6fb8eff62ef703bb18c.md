
这个协议名为 `CalorieCountable`。它规定任何遵循该协议的对象都必须有一个名为 `calories` 的属性来存储卡路里计数，以及一个返回字符串的 `description()` 方法。`{ get }` 表示你只需要能够读取 `calories` 中存储的值，而不必写入该值。请注意，`description()` 方法的定义没有指定，因为这将在类、结构体或枚举中完成。要遵循一个协议，你只需在类名后输入冒号，后跟协议名称，然后实现所需的属性和方法即可。

要使 `Burger` 类遵循此协议，请按如下方式修改代码：

```
class Burger: CalorieCountable {
    let calories = 800
    func description() -> String {
        "This burger has \(calories) calories"
    }
}
```

正如你看到的，`calories` 属性和 `description()` 方法已添加到 `Burger` 类中。尽管协议指定的是变量，但这里可以使用常量，因为协议只要求你获取 `calories` 的值，而不要求设置它。

让我们也让 `Fries` 结构体采用此协议。请按如下方式修改 `Fries` 结构体的代码：

```
struct Fries: CalorieCountable {
    let calories = 500
    func description() -> String {
        "These fries have \(calories) calories"
    }
}
```

添加到 `Fries` 结构体的代码与添加到 `Burger` 类的代码类似，现在它也遵循 `CalorieCountable` 协议了。

你可以用同样的方式修改 `Sauce` 枚举，但我们改用扩展（extensions）来实现。扩展可以扩展现有类、结构体或枚举的功能。你将在下一节中使用扩展将 `CalorieCountable` 协议添加到 `Sauce` 枚举中。

2. 要获取总卡路里数，请在创建 `foodArray` 常量的行之后添加以下代码：

```
let totalCalories = foodArray.reduce(0, {$0 + $1.calories})
print(totalCalories)
```

`reduce` 方法用于从 `foodArray` 数组的元素中生成单个值。该方法的第一个参数是初始值，设置为 0。第二个参数是一个闭包，它将初始值与元素 `calories` 属性中存储的值合并。对于 `foodArray` 数组中的每个元素都会重复此过程，结果被赋值给 `totalCalories`。总数 1315 将显示在调试区域中。

你已经学习了如何创建协议，并让类、结构体或枚举遵循它，无论是通过类定义内部还是通过扩展。接下来让我们看看错误处理，以及如何在程序中响应或从错误中恢复。

## 错误处理

编写应用时，请记住错误情况可能发生，而错误处理就是你的应用如何响应并从中恢复的方式。

首先，你创建一个遵循 Swift 的 `Error` 协议的类型，这使该类型可用于错误处理。通常使用枚举，因为你可以为不同种类的错误指定关联值。当发生意外情况时，你可以通过抛出错误来停止程序执行。你使用 `throw` 语句来做到这一点，并提供遵循 `Error` 协议的类型实例，并附带相应的值。这让你能够看到出了什么问题。

当然，如果你能在不停止程序的情况下响应错误会更好。为此，你可以使用 `do-catch` 块，如下所示：

```
do {
    try expression1
    statement1
} catch {
    statement2
}
```

在这里，你使用 `try` 关键字尝试执行 `do` 块中的代码。如果抛出错误，就会执行 `catch` 块中的语句。你还可以有多个 `catch` 块来处理不同的错误类型。

+   3. 在 `checkWebsite(siteUp:)` 函数定义之后输入以下代码，然后运行你的程序：

```
let siteStatus = true
try checkWebsite(siteUp: siteStatus)
```

由于 `siteStatus` 为 `true`，结果区域将显示 `Site is up`。

+   4. 将 `siteStatus` 的值改为 `false` 并运行你的程序。你的程序会崩溃，并在调试区域显示以下错误信息：

```
Playground execution terminated: An error was thrown and was not caught.
```

+   5. 当然，如果你能处理错误而不让程序崩溃，那总是更好的。你可以通过使用 `do-catch` 块来做到这一点。按如下所示修改代码并运行：

```
let siteStatus = false
do {
    print(try checkWebsite(siteUp: siteStatus))
} catch {
    print(error)
}
```

`do` 块尝试执行 `checkWebsite(siteUp:)` 函数，如果成功则打印状态。如果出现错误，程序不会崩溃，而是执行 `catch` 块中的语句，`siteDown` 错误消息会出现在调试区域。

你已经学会了如何在不让应用崩溃的情况下处理错误。太棒了！

## 小结

在本章中，你学习了如何编写协议，以及如何让类、结构体和枚举遵循它们。你还学习了如何通过使用扩展来扩展类的功能。最后，你学习了如何使用 `do-catch` 块处理错误。

这些内容现在可能看起来相当抽象且难以理解，但在本书的第三部分中，你将看到如何使用协议在程序的不同部分实现常见功能，而不是一遍又一遍地编写相同的代码。你将看到扩展在组织代码方面有多么有用，它使代码易于维护。最后，你将看到良好的错误处理如何让你轻松定位编写应用时犯的错误。

## 9

## Swift 并发

Apple 在 WWDC21 上推出了 **Swift 并发**，它为 Swift 5.5 添加了对结构化异步和并行编程的支持。它允许你编写更易读、更易理解的并发代码。在 WWDC24 期间，Apple 推出了 **Swift 6**，通过在编译时诊断**数据竞争**使并发编程更加容易。Swift 并发文档在 WWDC25 之后进一步更新。

> 目前，不建议为大型现有项目开启严格并发模式，因为这很可能会产生多个错误和警告。但是，由于这是 Apple 未来的发展方向，你将在本章以及本书*第三部分*的项目中开启它，以便你学习和积累相关经验。

在本章中，你将学习 Swift 并发的基本概念。接下来，你将检查一个没有使用并发的应用并探讨其问题。之后，你将使用 `async/await` 在应用中实现并发。最后，你将通过使用 `async-let` 使应用更高效。

到本章结束时，你将学习到 Swift 并发工作的基础知识，以及如何更新你自己的应用来使用它。

将涵盖以下主题：

- Swift 并发
- 检查没有使用并发的应用
- 使用 `async/await` 更新应用
- 使用 `async-let` 提高效率

## 技术要求

我们将使用一个示例应用 *BreakfastMaker* 来理解 Swift 并发的概念。

本章完整的 Xcode 项目位于本书代码包的 Chapter09 文件夹中，可从此处下载：

https://github.com/PacktPublishing/iOS-26-Programming-for-Beginners-10E

观看以下视频查看代码的实际运行：

https://youtu.be/QzsaoTmCp5s

让我们从下一节开始学习 Swift 并发。

## Swift 并发

在 Swift 5.5 中，Apple 添加了对以结构化方式编写**异步**和**并行**代码的支持。

异步代码允许你的应用挂起和恢复代码。并行代码允许你的应用同时运行多段代码。这使你的应用可以在下载互联网数据等操作进行的同时更新用户界面。

你可以通过以下链接阅读 Apple 的 Swift 并发文档：https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency/。

在 WWDC24 期间，Apple 发布了 Swift 6。借助 Swift 6 语言模式，编译器现在可以保证并发程序没有数据竞争。这意味着来自应用某一部分的代码不能再访问应用中另一部分代码正在修改的同一内存区域。但是，当你创建新的 Xcode 项目时，默认是 Swift 5 语言模式，你必须开启 Swift 6 语言模式才能启用此功能。WWDC25 之后，Apple 更新了 Swift 并发的文档和迁移指南。

要查看 Apple 关于将应用迁移到 Swift 6 的文档，请点击此链接：https://www.swift.org/migration/documentation/migrationguide/。

要查看 Apple 关于 Swift 并发最新更新的 WWDC25 视频，请点击此链接：https://developer.apple.com/videos/play/wwdc2025/268/。
