## 6 Functions and Closures

到目前为止，你可以编写相当复杂的程序，这些程序可以做出决策并重复指令序列。你也可以使用集合类型为程序存储数据。随着你编写的程序规模和复杂性增大，理解它们在做什么会变得更困难。

为了使大型程序更容易理解，Swift 允许你创建 **functions**，它让你可以将多个指令组合起来并通过调用单个名称来执行。你也可以创建 **closures**，它让你可以将多个指令组合起来而无需名称并将其分配给常量或变量。

到本章结束时，你将学习关于函数、嵌套函数、函数作为返回类型、函数作为参数以及 guard 语句的知识。你还将学习如何创建和使用闭包。

本章将涵盖以下主题：

- Functions
- Closures

### Technical requirements

本章的 Xcode playground 位于本书代码包的 Chapter06 文件夹中，可在此下载：

https://github.com/PacktPublishing/iOS-26-Programming-for-Beginners-10E

观看以下视频以查看代码的运行情况：

https://youtu.be/Sv269qS-TGQ

如果你想从头开始，创建一个新的 playground 并将其命名为 FunctionsAndClosures。

你可以在阅读过程中输入并运行本章中的所有代码。让我们先学习关于函数的内容。

### Functions

函数对于封装执行特定任务的多个指令非常有用，例如以下内容：

- 计算餐厅一顿饭的 10% 服务费
- 计算你想要购买的汽车的月付款

函数看起来像这样：

```
func functionName(parameter1: ParameterType, ...) -> ReturnType {
    code
}
```

每个函数都有一个描述性的名称。你可以定义函数作为输入接收的一个或多个值，称为 **parameters**。你也可以定义函数完成时将输出的内容，称为其 **return type**。参数和返回类型都是可选的。

你“调用”函数的名称来执行它。这是一个函数调用的样子：

```
functionName(parameter1: argument1, ...)
```

你提供输入值（称为 **arguments**），其类型与函数的参数匹配。

要了解有关函数的更多信息，请访问 https://docs.swift.org/swift-book/documentation/the-swift-programming-language/functions/。

让我们在下一节中看看如何创建一个函数来计算服务费。

#### Create a function

在其最简单的形式中，函数只是执行一些指令并且没有任何参数或返回类型。你将通过编写一个函数来计算一顿饭的服务费来了解其工作原理。服务费应为餐费的 10%。

将以下代码添加到你的 playground 以创建并调用此函数并运行它：

```
func serviceCharge() {
    let mealCost = 50
    let serviceCharge = mealCost / 10
    print("Service charge is \(serviceCharge)")
}
serviceCharge()
```

你刚刚创建了一个非常简单的名为 serviceCharge() 的函数。它所做的只是计算一顿价值 $50 的餐的 10% 服务费，即 50 / 10，结果为 5。然后你使用其名称调用此函数。你将在 Debug 区域看到 Service charge is 5 显示。

这个函数并不是很有用，因为每次调用此函数时 mealCost 始终为 50。此外，结果仅在 Debug 区域打印，无法在程序的其他地方使用。让我们为此函数添加一些参数和返回类型以使其更有用。

按如下所示修改你的代码：

```
func serviceCharge(mealCost: Int) -> Int {
    return mealCost / 10
}
let serviceChargeAmount = serviceCharge(mealCost: 50)
print(serviceChargeAmount)
```

这样好多了。现在，你可以在调用 serviceCharge(mealCost:) 函数时设置餐费，并且结果可以分配给变量或常量。它看起来有点别扭，不过。你应该尝试使 Swift 中的函数签名读起来像英语句子，因为这是最佳实践。让我们在下一节中看看如何做到这一点，在那里你将使用自定义标签使你的函数更像英语且更容易理解。





## 使用自定义参数标签

请注意，serviceCharge(mealCost:) 函数读起来并不很像英语。你可以为参数添加自定义标签，使函数更易于理解。

按如下方式修改你的代码：

```
func serviceCharge(forMealPrice mealCost: Int) -> Int {
    mealCost / 10
}
let serviceChargeAmount = serviceCharge(forMealPrice: 50)
print(serviceChargeAmount)
```

该函数的工作方式与之前相同，但调用它时，你使用 serviceCharge(forMealPrice:)。这听起来更像英语，并且更容易弄清楚函数的作用。另外，请注意，如果你的函数体仅由单个语句组成，return 关键字是可选的。

在下一节中，你将学习如何在其他函数体内使用多个较小的函数，这些函数被称为嵌套函数。

## 使用嵌套函数

在另一个函数体内可以有一个函数，这些被称为嵌套函数。这允许你将几个相关的函数放在一起，使包含函数更易于理解。

嵌套函数可以使用包含函数的变量。让我们通过编写一个计算贷款月供的函数来看看嵌套函数是如何工作的。

输入并运行以下代码：

```
func calculateMonthlyPayments(carPrice: Double, downPayment: Double, interestRate: Double, paymentTerm: Double) -> Double {
    func loanAmount() -> Double {
        carPrice - downPayment
    }
    func totalInterest() -> Double {
        interestRate * paymentTerm
    }
    func numberOfMonths() -> Double {
        paymentTerm * 12
    }
```

```
return ((loanAmount() + (loanAmount() *
    totalInterest() / 100 )) / numberOfMonths())
}
calculateMonthlyPayments(carPrice: 50000, downPayment: 5000, interestRate: 3.5, paymentTerm: 7.0)
```

这里，calculateMonthlyPayments(carPrice:downPayment:interestRate:paymentTerm:) 内有三个函数。让我们来看看它们：

- 第一个嵌套函数 loanAmount() 通过用 downPayment 从 carPrice 中减去来计算总贷款金额。它返回 50000 - 5000 = 45000。
- 第二个嵌套函数 totalInterest() 通过将 interestRate 乘以 paymentTerm 来计算在还款期内产生的总利息。它返回 3.5 * 7 = 24.5。
- 第三个嵌套函数 numberOfMonths() 通过将 paymentTerm 乘以 12 来计算还款期的总月数。它返回 7 * 12 = 84。

请注意，这三个嵌套函数都使用了包含函数的变量。返回的值是 ( 45000 + ( 45000 * 24.5 / 100 ) ) / 84 = 666.96，这是购买这辆车七年内每月必须支付的金额。

正如你所见，Swift 中的函数就像其他语言中的函数，但它们有一个很酷的功能。函数在 Swift 中是第一类类型，因此可以用作参数和返回类型。让我们在下一节中看看这是如何完成的。

## 使用函数作为返回类型

一个函数可以返回另一个函数作为其返回类型。输入并运行以下代码以创建一个使用两种可能方法之一生成 pi 值的函数：

```
func approximateValueOfPi1() -> Double {
    3.14159
}
func approximateValueOfPi2() -> Double {
    22.0 / 7.0
}
```

```
func pi() -> (() -> Double) {
    approximateValueOfPi1
    // approximateValueOfPi2
}
pi()()
```

approximateValueOfPi1() 和 approximateValueOfPi2() 都是没有参数并返回 pi 近似值的函数。pi() 函数的返回类型是一个没有参数并返回 Double 的函数。这意味着它可以返回 approximateValueOfPi1（正如这里所示）或 approximateValueOfPi2，因为这两个函数都匹配预期的返回类型。

pi()() 调用 approximateValueOfPi1 函数，它返回 3.14159。3.14159 显示在结果区域。

让我们在下一节中看看函数如何用作另一个函数的参数。

## 使用函数作为参数

一个函数可以接受另一个函数作为参数。输入并运行以下代码以创建一个函数，该函数确定在数字列表中是否存在满足特定条件的数字：

```
func isThereAMatch(listOfNumbers: [Int], condition: (Int) -> Bool) -> Bool
{
    for number in listOfNumbers {
        if condition(number) {
            return true
        }
    }
    return false
}
func numberIsOdd(number: Int) -> Bool {
    (number % 2) > 0
}
func numberIsEven(number: Int) -> Bool {
    (number % 2) == 0
}
let numbersList = [1, 3, 5, 7]
isThereAMatch(listOfNumbers: numbersList, condition: numberIsOdd)
```

isThereAMatch(listOfNumbers:condition:) 有两个参数：一个整数数组和一个函数。作为参数提供的函数必须接受一个整数值并返回一个布尔值。

numberIsOdd(number:) 和 numberIsEven(_:) 都接受一个整数并返回一个布尔值，这意味着任一函数都可以作为第二个参数的参数。numbersList，一个包含奇数的数组，被用作第一个参数的参数。当 numberIsOdd 用作第二个参数的参数时，isThereAMatch(listOfNumbers:condition:) 在调用时将返回 true。试着也使用 numberIsEven 作为第二个参数的参数。

作为参数和返回类型的函数可能难以理解，但在你学习的这个阶段相对较少见，所以如果你一开始不明白也不用担心。随着你获得经验，它会变得更清晰。

在下一节中，你将看到如果使用的参数不合适，如何对函数进行早期退出。

## 使用 guard 语句提前退出函数

假设你需要一个函数用于在线购买终端。此函数将在你购买某物时计算借记卡或信用卡的剩余余额。你想购买的商品价格输入到文本字段中。

文本字段中的值被转换为整数，以便你可以计算剩余的卡余额。如果输入数据有问题，能够提前退出函数是很有用的。

输入并运行以下代码：

```
func buySomething(itemValueEntered itemValueField: String, cardBalance: Int) -> Int {
    guard let itemValue = Int(itemValueField) else {
        print("Error in item value")
        return cardBalance
    }
    let remainingBalance = cardBalance - itemValue
    return remainingBalance
}
print(buySomething(itemValueEntered: "10", cardBalance: 50))
print(buySomething(itemValueEntered: "blue", cardBalance: 50))
```

你应该在调试区域看到以下结果：

```
40
Error in item value
50
```

让我们看看这个函数是如何工作的。函数体中的第一行是一个 guard 语句。它检查条件是否为真；如果不是，则退出函数。在这里，它用于检查用户是否在在线购买终端中输入了有效的价格。

如果是，则该值可以成功转换为整数，并且你可以计算剩余的卡余额。否则，guard 语句中的 else 子句将被执行。错误消息打印到调试区域，并返回未更改的卡余额。

对于 print(buySomething(itemValueEntered: "10", cardBalance: 50))，商品价格成功从卡余额中扣除，并返回 40。

对于 print(buySomething(itemValueEntered: "blue", cardBalance: 50))，guard 语句的条件失败，其 else 子句被执行，导致错误消息打印到调试区域并返回 50。

你现在知道如何创建和使用函数。你还看到了如何使用自定义参数标签、嵌套函数、作为参数或返回类型的函数以及 guard 语句。

现在，让我们来看闭包。与函数一样，闭包允许你组合多个指令，但闭包没有名称，可以分配给常量或变量。你将在下一节中了解它们的工作原理。

## 闭包

闭包像函数一样，包含一系列指令，可以接受参数并返回值。然而，闭包没有名称。闭包中的指令序列用大括号 ({ }) 包围，并且 in 关键字将参数和返回类型与闭包体分隔开。

闭包可以分配给常量或变量，因此如果需要在程序内部传递它们会很方便。例如，假设你有一个从互联网下载文件的应用程序，并且文件下载完成后需要对文件执行某些操作。你可以将处理文件的指令列表放在一个闭包中，并在文件下载完成后让程序执行它。





要了解有关闭包的更多信息，请访问 https://docs.swift.org/swift-book/documentation/the-swift-programming-language/closures/。

你现在将编写一个闭包，对数字数组的每个元素应用计算。将以下代码添加到你的 playground 中并点击 Run 按钮运行它：

```
var numbersArray = [2, 4, 6, 7]
let myClosure = { (number: Int) -> Int in
    let result = number * number
    return result
}
let mappedNumbers = numbersArray.map(myClosure)
```

这会将一个计算数字的平方的闭包赋给 myClosure。map() 函数随后将此闭包应用于 numbersArray 中的每个元素。每个元素都乘以自身，[4, 16, 36, 49] 会出现在 Results 区域。

闭包可以用更简洁的方式编写，你将在下一节中看到如何做到这一点。

## 简化闭包

新开发者难以掌握的事情之一是经验丰富的 Swift 程序员用来编写闭包的非常简洁的方法。考虑以下示例中显示的代码：

```
var testNumbers = [2, 4, 6, 7]
let mappedTestNumbers = testNumbers.map({
    (number: Int) -> Int in
    let result = number * number
    return result
})
print(mappedTestNumbers)
```

这里，你有 testNumbers，一个数字数组，并使用 map(_:) 函数将闭包映射到数组的每个元素。闭包中的代码将数字乘以自身，生成该数字的平方。结果 [4, 16, 36, 49] 然后打印到 Debug 区域。正如你将看到的，闭包代码可以写得更简洁。

当闭包的类型已经已知时，你可以删除参数类型、返回类型或两者。单语句闭包隐式返回其唯一语句的值，这意味着你也可以删除 return 语句。因此，你可以按如下方式编写闭包：

```
let mappedTestNumbers = testNumbers.map({ number in
    number * number
})
```

当闭包是函数的唯一参数时，你可以省略包围闭包的括号，如下所示：

```
let mappedTestNumbers = testNumbers.map { number in
    number * number
}
```

你可以通过表达它们在参数列表中的相对位置的数字来引用参数，而不是按名称引用，如下所示：

```
let mappedTestNumbers = testNumbers.map { $0 * $0 }
```

因此，闭包现在非常简洁，但对新开发者来说理解起来会很困难。随意以你感到舒适的方式编写闭包。

你现在知道如何创建和使用闭包，以及如何更简洁地编写它们。太棒了！

## 总结

在本章中，你学习了如何将语句分组为函数。你学习了如何使用自定义参数标签、函数内部的函数、函数作为返回类型、函数作为参数以及 guard 语句。当你需要在程序的不同点完成相同的任务时，这将很有用。

你还学习了如何创建闭包。这在你需要在程序中传递代码块时很有用。

在下一章中，我们将研究类、结构体和枚举。类和结构体允许创建可以存储状态和行为的复杂对象，而枚举可用于限制可以分配给变量或常量的值，从而减少出错的可能性。
