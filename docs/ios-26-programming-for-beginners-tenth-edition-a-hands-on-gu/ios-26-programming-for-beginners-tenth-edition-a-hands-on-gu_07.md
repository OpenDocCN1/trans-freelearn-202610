## 8 协议、扩展和错误处理

在上一章中，你学习了如何使用类或结构体表示复杂对象，以及如何使用枚举将相关值分组在一起。

在本章中，你将学习有关 **协议**、**扩展** 和 **错误处理** 的知识。协议定义了可由类、结构体或枚举采用的方法、属性和其他要求的蓝图。扩展使你能够为现有类、结构体或枚举提供新功能。错误处理涵盖了如何在程序中响应和从错误中恢复。

到本章结束时，你将能够编写自己的协议以满足应用的要求，使用扩展为现有类型添加新功能，并在应用中处理错误条件而不导致崩溃。

本章将涵盖以下主题：

- 协议
- 扩展
- 错误处理

### 技术要求

本章的 Xcode 游乐场可在本书代码包的 Chapter08 文件夹中找到，可在此处下载：

https://github.com/PacktPublishing/iOS-26-Programming-for-Beginners-10E

查看以下视频以了解代码的运行情况：

https://youtu.be/0PGZdXA76eY

如果你想从头开始，创建一个新的游乐场并将其命名为 ProtocolsExtensionsAndErrorHandling。你可以在阅读过程中输入并运行本章中的所有代码。让我们从协议开始，协议是一种指定类、结构体或枚举应具有的属性和方法的方式。

### 协议

协议就像蓝图，决定对象必须具有哪些属性或方法。在声明协议后，类、结构体和枚举可以采用它并为所需属性和方法提供自己的实现。

协议声明如下所示：

```
protocol ProtocolName {
    var readWriteProperty1 {get set}
    var readOnlyProperty2 {get}
    func methodName1()
    func methodName2()
}
```

与类和结构体一样，协议名称以大写字母开头。属性使用 var 关键字声明。如果你想要可读写属性，则使用 {get set}，如果你想要只读属性，则使用 {get}。请注意，你只需指定属性和方法的名称；实现是在采用的类、结构体或枚举中完成的。

有关协议的更多信息，请访问 https://docs.swift.org/swift-book/documentation/the-swift-programming-language/protocols。




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
