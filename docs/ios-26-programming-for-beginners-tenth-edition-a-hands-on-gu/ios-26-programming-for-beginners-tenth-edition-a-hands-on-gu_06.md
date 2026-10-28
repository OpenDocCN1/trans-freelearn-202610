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





请注意，这里你先创建实例，然后再为该实例赋值。也可以在创建实例时就赋值，你可以在类声明中实现一个初始化器来做到这一点。

- 3. 初始化器负责确保在创建类时所有实例属性都有有效值。让我们为 Animal 类添加一个初始化器。按如下所示修改你的类定义：

```
class Animal {
    var name: String
    var sound: String
    var numberOfLegs: Int
    var breathesOxygen: Bool
    init(name: String, sound: String, numberOfLegs: Int, breathesOxygen: Bool) {
        self.name = name
        self.sound = sound
        self.numberOfLegs = numberOfLegs
        self.breathesOxygen = breathesOxygen
    }
    func makeSound() {
        print(sound)
    }
}
```

如你所见，初始化器使用 init 关键字，并有一组参数用于设置属性值。请注意，self 关键字区分了属性名和参数名。例如，self.name 指的是属性，而 name 指的是参数。

在初始化过程结束时，类中的每个属性都应具有有效值。

- 4. 此时你的代码会出现一些错误，因为函数调用没有任何参数。你需要更新函数调用来解决这个问题。按如下所示修改你的代码并运行它：

```
func makeSound() {
    print(sound)
}

let cat = Animal(name: "Cat", sound: "Mew", numberOfLegs: 4, breathesOxygen: true)
print(cat.name)
```

结果与第 2 步中的结果相同，但你在一个指令中创建了实例并设置了其属性。太棒了！

现在有不同类型的动物，例如哺乳动物、鸟类、爬行动物和鱼类。你可以为每种类型创建类，但也可以基于现有类创建子类。让我们在下一节中看看如何做到这一点。

### 创建子类

类的子类继承了现有类的所有方法和属性。你也可以根据需要添加额外的属性和方法。例如，对于一家 IT 公司，你可以将 CustomerSupportAgent 作为 Employee 的子类。该类将具有 Employee 类的所有属性，以及客户支持角色所需的额外属性。

你现在将创建 Mammal，它是 Animal 类的子类。按照以下步骤操作：

- 1. 要声明 Mammal 类，在 Animal 类声明之后输入以下代码：

```
class Mammal: Animal {
    let hasFurOrHair: Bool = true
}
```

在类名后输入 : Animal 使 Mammal 类成为 Animal 类的子类。它具有 Animal 类中声明的所有属性和方法，以及一个额外属性：hasFurOrHair。由于 Animal 类是 Mammal 类的父类，你可以将其称为 Mammal 类的超类。

- 2. 按如下所示修改创建类实例的代码并运行它：

```
let cat = Mammal(name: "Cat", sound: "Mew", numberOfLegs: 4, breathesOxygen: true)
```

cat 现在是 Mammal 类的实例，而不是 Animal 类的实例。如你所见，Debug 区域中显示的结果与之前相同，并且没有错误。hasFurOrHair 的值尚未显示。让我们修复这个问题。

- 3. 在游乐场中所有其他代码之后输入以下代码以显示 hasFurOrHair 属性的内容并运行它：

```
print(cat.hasFurOrHair)
```

由于 Animal 类的初始化器没有为 hasFurOrHair 分配值的参数，因此使用默认值，true 将在 Debug 区域中显示。

你已经看到子类可以具有额外的属性。子类也可以具有额外的方法，并且子类中的方法实现可以与超类实现不同。让我们在下一节中看看如何做到这一点。

### 重写超类方法

到目前为止，你一直在使用多个 print() 语句来显示类实例的值。你将实现一个 description() 方法来在 Debug 区域中显示所有实例属性，因此不再需要多个 print() 语句。按照以下步骤操作：

- 1. 按如下所示修改你的 Animal 类声明以实现 description() 方法：

```
class Animal {
    var name: String
    var sound: String
    var numberOfLegs: Int
    var breathesOxygen: Bool = true
    init(name: String, sound: String, numberOfLegs:
    Int, breathesOxygen: Bool) {
        self.name = name
        self.sound = sound
        self.numberOfLegs = numberOfLegs
        self.breathesOxygen = breathesOxygen
    }
    func makeSound() {
        print(sound)
    }
    func description() -> String {
    "name: \(name) sound: \(sound)
    numberOfLegs: \(numberOfLegs)
    breathesOxygen: \(breathesOxygen)"
    }
}
```

description() 方法根据 trafficLightColor 的值返回一个字符串。由于 trafficLightColor 的值是 TrafficLightColor.red，因此 red 将出现在 Debug 区域中。

你已经学习了如何创建和使用枚举来存储分组值，以及如何向它们添加方法。干得好！

### 总结

在本章中，你学习了如何使用类声明复杂对象、创建类的实例、创建子类以及重写类方法。你还学习了如何声明结构体、创建结构体的实例，并理解引用类型和值类型之间的区别。最后，你学习了如何使用枚举来表示一组特定值。

你现在知道如何使用类和结构体在自己的程序中表示复杂对象，以及如何使用枚举将相关值分组在一起。

在下一章中，你将学习如何使用协议在类和结构体中指定通用特征、使用扩展扩展内置类的功能，以及在程序中处理错误。

**获取本书的 PDF 版本和独家附加内容**

扫描二维码（或访问 packtpub.com/unlock）。按名称搜索本书，确认版本，然后按照页面上的步骤操作。

*注意：请保留你的发票。直接从 Packt 购买无需发票。*

![](img/0972ba626734a3986e095c788777818e_137_0.png)
