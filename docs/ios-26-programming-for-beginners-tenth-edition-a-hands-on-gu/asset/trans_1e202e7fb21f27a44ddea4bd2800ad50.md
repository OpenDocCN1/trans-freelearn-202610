

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

## 7 类、结构体和枚举

在上一章中，你学习了如何使用函数和闭包对指令序列进行分组。

是时候思考如何在代码中表示复杂对象了。例如，考虑一辆汽车。你可以使用 String 常量存储汽车名称和 Double 变量存储汽车价格，但它们彼此没有关联。你已经看到你可以将指令分组以制作函数和闭包。

在本章中，你将学习如何使用 **类** 和 **结构体** 将常量和变量分组为单个实体，以及如何操作它们。你还将学习如何使用 **枚举** 将一组相关值组合在一起。

到本章结束时，你将学习如何创建和初始化一个类，从现有类创建子类，创建和初始化一个结构体，在类和结构体之间进行区分，以及创建枚举。

本章将涵盖以下主题：

- 类
- 结构体
- 枚举

## 技术要求

本章的 Xcode playground 可以在本书代码包的 Chapter07 文件夹中找到，可以在此处下载：

https://github.com/PacktPublishing/iOS-26-Programming-for-Beginners-10E

观看以下视频以查看代码的运行情况：

https://youtu.be/Fbh1FIdXxro

如果你想从头开始，创建一个新的 playground 并将其命名为 ClassesStructuresAndEnumerations。你可以在阅读过程中输入并运行本章中的所有代码。让我们从学习什么是类以及如何声明和定义它开始。

## 类

类对于表示复杂对象很有用；以下是一些示例：

- 公司员工的个人资料信息
- 电子商务网站上的待售商品
- 你为保险目的在家中拥有的物品

下面是类声明和定义的外观：

```
class ClassName {
    property1
    property2
    property3
    method1() {
        code
    }
    method2() {
        code
    }
}
```

每个类都有一个描述性名称，并且包含用于表示对象的变量或常量。与类关联的变量或常量称为 **属性**。

类还可以包含执行特定任务的函数。与类关联的函数称为 **方法**。

一旦你声明并定义了类，你就可以创建该类的 **实例**。想象你正在为动物园创建应用程序。如果你有一个 Animal 类，你可以使用该类的实例来表示动物园中的不同动物。这些实例中的每一个都将具有不同的属性值。

要了解有关类的更多信息，请访问 https://docs.swift.org/swift-book/documentation/the-swift-programming-language/classes-and-structures。

让我们看看如何使用类。你将学习如何声明和定义类，基于类声明创建实例，并操作这些实例。你将通过在下一节中创建表示动物的类开始。

### 创建类声明

让我们声明并定义一个可以存储动物详细信息的类。将以下代码添加到你的 playground：

```
class Animal {
    var name: String = ""
    var sound: String = ""
    var numberOfLegs: Int = 0
    var breathesOxygen: Bool = true
    func makeSound() {
        print(sound)
    }
}
```

你刚刚声明了一个名为 Animal 的非常简单的类。惯例规定类名以大写字母开头。此类具有属性来存储动物的名称、它发出的声音、它拥有的腿数以及它是否呼吸氧气。此类还具有一个方法 makeSound()，它将发出的噪音打印到 Debug 区域。

现在你有了 Animal 类，让我们在下一节中使用它来创建一个代表动物的实例。

### 创建类的实例

一旦你声明并定义了类，你就可以创建该类的实例。你现在将创建 Animal 类的一个实例来表示一只猫。按照以下步骤操作：

- 1. 要创建 Animal 类的实例，列出其所有属性并调用其 makeSound() 方法。在类声明后输入以下代码并运行它：

  ```
  let cat = Animal()
  print(cat.name)
  print(cat.sound)
  print(cat.numberOfLegs)
  print(cat.breathesOxygen)
  cat.makeSound()
  ```

  你可以通过在实例名称后键入一个点，然后跟上你想要的属性或方法来访问实例属性和方法。你将在 Debug 区域看到实例属性和方法的调用值。由于这些值是创建类时分配的默认值，name 和 sound 包含空字符串，numberOfLegs 包含 0，breathesOxygen 包含 true，并且 makeSound() 方法打印一个空字符串。

- 2. 让我们为此实例的属性分配一些值。按如下方式修改你的代码：

  ```
  let cat = Animal()
  cat.name = "Cat"
  cat.sound = "Mew"
  cat.numberOfLegs = 4
  cat.breathesOxygen = true
  print(cat.name)
  print(cat.sound)
  print(cat.numberOfLegs)
  print(cat.breathesOxygen)
  cat.makeSound()
  ```

  现在，当你运行程序时，以下内容会显示在 Debug 区域：

  ```
  Cat
  Mew
  4
  true
  Mew
  ```

  所有实例属性的值和 makeSound() 方法的结果都打印到 Debug 区域。

