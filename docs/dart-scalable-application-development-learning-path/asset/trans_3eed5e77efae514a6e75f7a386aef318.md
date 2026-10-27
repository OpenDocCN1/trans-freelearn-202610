

## 将函数作为参数传递给另一个函数

当我们需要实现策略设计模式，使程序代码能够在运行时被选择和执行时，将函数作为参数传递给其他函数会非常有用，如下面的代码所示：

```
library function_param;

// Returns sum of [a] and [b]
add(a, b) {
  return a + b;
}

// Operation executor
executor(operation, x, y) {
  return operation(x, y);
}

void main() {
  // Execute operation
  var result = executor(add, 2, 1);
  print("Result is ${result}");
}
```

上述代码的执行结果如下：

```
Result is 3
```

上述示例中的全局 executor 函数可以调用任何接受两个参数的函数。你可以看到策略设计模式以匿名函数的形式实现，这些匿名函数作为集合中方法的参数传递。

## 将一个函数作为另一个函数的结果返回

有时，一个函数可以作为另一个函数的结果返回，如下面的代码所示：

```
library function_return;

// Returns sum of [a] and [b]
```

上述代码的执行结果如下：

```
Result is 3
```

在我们的示例中，有两个函数和一个数组 **operations**，该数组存储了对它们的引用。

## 闭包

函数可以在全局作用域中创建，也可以在另一个函数的作用域中创建。如果一个函数可以被引用，并能访问其词法作用域中的变量，那么它被称为闭包，如下面的代码所示：

```
library function_closure;

// Function returns closure function.
calculate(base) {
  // Counter store
  var count = 1;
  // Inner function - closure
  return () => print("Value is ${base + count++}");
}

void main() {
  // The outer function returns inner
  var f = calculate(2);
  // Now we call closure
  f();
  f();
}
```

上述代码的执行结果如下：

```
Value is 3
Value is 4
```

我们有一个 `calculate` 函数，它包含 `count` 变量并返回一个内部函数。内部函数可以访问 `count` 变量，因为两者定义在相同的作用域中。`count` 变量只存在于 `calculate` 的作用域内，正常情况下会在函数退出时消失。但这里情况并非如此，因为 `calculate` 返回的内部函数持有对 `count` 的引用。该变量已被“闭合”，意味着它处于一个闭包中。

最后，我们了解了 **一等函数** 是什么、可以在哪里使用它们，以及使用闭包的重要性。让我们继续学习类和混入。

## 类和混入

我们都知道，尝试重新发明轮子是浪费时间的。每次想要造一辆汽车时都这样做，更是浪费。那么，如何更高效地编写程序代码并使其可复用，以帮助我们开发更强大的应用程序呢？在大多数情况下，我们在回答这个问题时会求助于面向对象编程（OOP）范式。OOP 代表了对象的概念，这些对象包含数据字段以及作用于这些数据的方法。程序被设计为使用对象作为类的实例，这些实例相互交互以组织功能。

### 类型

Dart 语言是动态类型的，因此我们可以在代码中编写带或不带类型注解的程序。最好使用类型注解，原因如下：

- 类型注解可以早期发现错误。静态分析器可以在你犯错的地方提醒我们潜在的问题。
- Dart 会自动将类型注解转换为运行时断言检查。在检查模式下，动态类型断言处于启用状态，当类型不匹配时可以捕获一些错误。
- 类型注解可以提高编译为 JavaScript 的代码的性能。
- 它们可以改进文档，使代码更容易阅读。
- 它们在特殊工具和 IDE（如名称补全）中非常有用。

我们的代码中没有包含类型注解，这一事实并不会阻止程序运行。没有类型注解的变量具有动态类型，并使用 var 或 dynamic 标记。以下是适合使用类型注解的一些建议：

- 你应该为公有和私有变量添加类型
- 你可以为方法和函数的参数添加类型
- 你应该避免在方法或函数的主体中添加类型

如果我们定义一辆赛车，那么我们必须实现 Car 类的所有特征和行为，但要对发动机、悬架、刹车等进行大幅修改：

```
import 'car.dart';
import 'passenger_car.dart';

void main() {
  // Create an instance of passenger car of white color,
  // carrying 750 kg and max passengers 5.
  Car car = new PassengerCar('white', 750.0, 5);
  // Move it
  car.move(100.0);
}
```

这里，我们只是创建了一个 PassengerCar 实例，并将其赋值给 car 变量，而无需定义任何特殊的接口。

### 混入

Dart 具有基于混入的继承，因此类主体可以在多个类层次结构中复用，如下面的代码所示：

```
library trailer;

// The trailer
class Trailer {
  // Access to car's [carrying] info
  double carrying = 0.0;

  // Trailer can carry [weight]
  void carry(double weight) {
    // Car's carrying increases on extra weight.
    carrying += weight;
  }
}
```

Trailer 类独立于 Car 类，但可以增加汽车的载重能力。我们在下面的代码中使用 with 关键字后跟 Trailer 类，将混入添加到 PassengerCar 类：

```
library passenger_car;

import 'car.dart';
import 'trailer.dart';

// Passenger car with trailer.
class PassengerCar extends Car with Trailer {
  // Max number of passengers.
  int maxPassengers = 4;

  /**
   * Create [PassengerCar] with [color], [carrying] and [maxPassengers].
   * We can use [Trailer] to carry [extraWeight].
   */
  PassengerCar(String color, double carrying, this.maxPassengers,
      {double extraWeight: 0.0}) : super(color, carrying) {
    // We can carry extra weight with [Trailer]
    carry(extraWeight);
  }
}
```

我们将 `Trailer` 作为混入添加到 `PassengerCar`，结果 `PassengerCar` 现在可以承载更多重量。请注意，我们没有修改 `PassengerCar` 本身，只是扩展了它的功能。同时，`Trailer` 可以与 `Truck` 或 `Bus` 类结合使用。混入看起来像一个接口，并通过类声明隐式定义，但有以下限制：

- 它没有声明的构造函数
- 混入的超类只能是 Object
- 它们不包含对 `super` 的调用

### 设计良好的类

设计良好的类与设计不良的类有什么区别？
以下是设计良好的类的特征：

- 它隐藏了所有实现细节
- 它通过使用抽象类将接口与实现分离
- 它只通过接口与其他类进行通信

上述所有特性都导致了封装。封装在 OOP 中扮演着重要角色。封装具有以下好处：

- 类可以独立地开发、测试、修改和使用
- 程序可以快速开发，因为类可以并行开发
- 类的优化可以在不影响其他类的情况下进行
- 类可以被更频繁地复用，因为它们没有紧密耦合
- 每个类的成功开发都会带来应用程序的成功

我们前面所有的示例都包含公有成员。这样对吗？那么，我们必须遵循什么规则来创建设计良好的类呢？

### 公有还是私有

让我们遵循以下简单原则来创建设计良好的类：

- 为类定义一个最小的公有 API。类的私有成员始终可以在库作用域内访问，所以不要犹豫去使用它们。
- 为了便于测试而将成员变量的私有级别改为公有级别，这是不可接受的。
- 非 final 的实例变量绝不应该是公有的；否则，我们就放弃了限制变量可存储值以及强制与该变量相关的不变式的能力。
- 当 final 实例变量或静态常量指向可变对象时，绝不应是公有的；否则，当 final 变量被修改时，我们限制了采取任何行动的能力。
- 不应有公有的、静态 final 的集合实例；否则，getter 方法会返回它，而我们就限制了修改集合内容的能力。

最后两条原则可以在下面的示例中看到。假设我们有一个 Car 类，其中定义了一个 final 静态的零件列表。我们可以用 Pedal 和 Wheel 来初始化它们，如下面的代码所示：

```
class Car {
  // Be careful with that code !!!
  static final List PARTS = ['Pedal', 'Wheel'];
}
void main() {
  print('${Car.PARTS}'); // Print: [Pedal, Wheel]
```

