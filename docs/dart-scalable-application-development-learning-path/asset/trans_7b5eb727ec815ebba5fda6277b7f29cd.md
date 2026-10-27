

```dart
print(catSound); // Meow
}

abstract class Animal {
  String makeNoise();
  factory Animal() => new Cat();
}

class Cat implements Animal { String makeNoise() => 'Meow'; }
class Dog implements Animal { String makeNoise() => 'Woof';}
```

使用工厂构造函数与抽象类结合的另一个示例在以下代码中展示：

```dart
import 'dart:math';

void main() {
  Cat an = new Animal();
  print(an.makeNoise());
}

abstract class Animal {
  // simulates computation:
  factory Animal() {
    var random = new Random();
    if (random.nextBool())
      return new Cat();
    else
      return new Dog();
  }
}

class Cat implements Animal { String makeNoise() => 'Meow'; }
class Dog implements Animal { String makeNoise() => 'Woof'; }
```

下一个示例也说明了如何使用工厂构造函数与抽象类：

```dart
void main() {
  Cat cat = new Animal("cat");
  Dog dog = new Animal("dog");
  print(cat.makeNoise());
}

abstract class Animal {
  String makeNoise();
  factory Animal(String type) {
    switch(type) {
      case "Cat":
        return new Cat();
      case "Dog":
        return new Dog();
      default:
        throw "The '$type' is not an animal";
    }
  }
}

class Cat implements Animal { String makeNoise() => 'Meow'; }
class Dog implements Animal { String makeNoise() => 'Woof'; }
```

### How it works...

在第一个示例中，我们有一组收集在 _cache 中的服务，但每个服务只能通过私有的 _internal 构造函数创建一次；每个命名服务都是唯一的。

在第二个示例中，我们展示了具有工厂构造函数的类不能直接被扩展；相反，子类型必须实现该类。在第三个示例中，我们展示了使用抽象类与工厂构造函数的三种变体。

为什么以及何时想要使用工厂构造函数？有时，我们不希望构造函数总是创建它所在类的新对象。以下是工厂构造函数的一些用例；代码示例向您展示了所有这些的用法示例：

- 为了从缓存中返回对象以便重用它
- 为了从构造函数所在类的子类型创建对象
- 为了将实例限制为一个唯一对象（单例模式）
- 即使是抽象类也可以包含工厂构造函数来返回具体类的默认实现（这是抽象类可以拥有构造函数的唯一方式）

工厂构造函数与任何其他构造函数一样通过 new 调用。类的使用者不知道构造函数实际上是一个工厂，因此您可以将常规构造函数重构为工厂构造函数以增强灵活性，而无需强制客户端更改其代码。工厂还可能涉及更多准备和计算，而类的使用者可能并不知情；使用者可能只是创建一个新实例。通过工厂调用私有构造函数也是一个常见模式，正如我们在第一个示例中所看到的。

### How it works...

`Immortal` 类包含其自身类型的一个对象，该对象通过调用私有 `_internal` 构造函数自行实例化。因为它将是唯一的，我们将其声明为 static。工厂构造函数始终返回此实例；执行隔离区中永远只能存在一个 `Singleton` 类的实例（此处命名为 `Immortal`）。它必须是一个工厂构造函数，因为只有这种类型可以返回值。代码甚至可以缩短，如下所示：

```dart
class Singleton {
  factory Singleton() => const Singleton._internal();
  const Singleton._internal();
}
```

## Using reflection

Dart 基于镜像的反射 API（包含在 `dart:mirrors` 库中）提供了一套强大的工具来反射代码。这意味着可以检查程序的完整结构并发现所有对象的全部属性。通过这种方式，可以反射性地调用方法。它甚至将能够动态评估尚未在源代码中字面指定的代码。一个例子是调用方法名作为参数提供的方法，因为它在数据库表中查找。

### Getting ready

使用反射的代码部分应具有以下导入代码：

```dart
import 'dart:mirrors';
```

### How to do it...

为了执行反射，我们执行以下操作：

- 在 `reflection` 项目中，我们使用类 `Embrace` 来进行反射：

  ```dart
  void main() {
    var embr = new Embrace(5);
    print(embr);  // Embraceometer reads 5
    embr.strength += 5;
    print(embr.toJson()); // {strength: 10}
    var embr2 = new Embrace(10);
    var bigHug = embr + embr2;
    // Start reflection code:
  ```

- 使用 MirrorSystem，如以下代码所示：

  ```dart
  final MirrorSystem ms = currentMirrorSystem();
  // Iterating through libraries
  ms
    .libraries
    .forEach((Uri name, LibraryMirror libMirror){
      print('$name $libMirror');
    });
  ```

- 使用 InstanceMirror 和 ClassMirror，如以下代码所示：

  ```dart
  InstanceMirror im = reflect(embr);
  InstanceMirror im2 = im.invoke(#toJson, []);
  print(im2.reflectee); // {strength: 10}
  ClassMirror cm = reflectClass(Embrace);
  ClassMirror cm2 = im.type;
  printAllDeclarationsOf(cm);
  InstanceMirror im3 = cm.newInstance(#light, []);
  print(im3.reflectee.strength);
  im3.reflectee.withAffection();
  }

  printAllDeclarationsOf(ClassMirror cm) {
    for (var k in cm.declarations.keys)  print(MirrorSystem.
  getName(k));
    print(MirrorSystem.getName(m.simpleName));
  }

  class Embrace {
    num _strength;
    num get strength => _strength;
    set strength(num value) => _strength=value;
    Embrace(this._strength);
    Embrace.light(): _strength=3;
    Embrace.strangle(): _strength=100;
    Embrace operator +(Embrace other) =>
        Embrace(strength + other.strength);
    String toString() => "Embraceometer reads $strength";
    Map toJson() => {'strength': '$_strength'};

    withAffection() {
      for (var no=0; no <= 3; no++) {
        for (var s=0; s <=5; s++) { strength = s; }
      }
    }
  }
  ```

运行前面的程序会产生以下输出：

```text
Embraceometer reads 5
{strength: 10}
dart:core LibraryMirror on 'dart.core'
dart:mirrors LibraryMirror on 'dart.mirrors'
dart:nativewrappers LibraryMirror on ""
dart:typed_data LibraryMirror on 'dart.typed_data'
dart:async LibraryMirror on 'dart.async'
dart:convert LibraryMirror on 'dart.convert'
dart:collection LibraryMirror on 'dart.collection'
dart:_internal LibraryMirror on 'dart._internal@0x1f109d24'
dart:isolate LibraryMirror on 'dart.isolate'
dart:math LibraryMirror on 'dart.math'
dart:builtin LibraryMirror on 'builtin'
dart:io LibraryMirror on 'dart.io'
file:///F:/Dartiverse/ADartCookbook/book/Chapter 4 - Object orientation/code/reflection/bin/reflection.dart LibraryMirror on ""
{strength: 10}
_strength
strength
strength=
+
toString
toJson
withAffection
Embrace
Embrace.light
Embrace.strangle
3
```

### How it works...

currentMirrorSystem 类返回当前隔离区上的 MirrorSystem 对象；libraries getter 为您提供当前代码范围内库的列表。

InstanceMirror 子类是对象实例的表示，ClassMirror 是类定义的表示。

使用顶层 reflect 方法对对象进行操作以获取 InstanceMirror。这允许您动态地在对象上调用代码产生另一个 InstanceMirror；使用其 reflectee 属性可以访问实际实例。

请注意，invoke 方法的第一个参数是方法名的符号（可从其 # 前缀识别）。符号是在 Dart 中引入的，因为它们在压缩后仍然存在。

顶层 reflectClass 方法对类进行操作会产生 ClassMirror；通过对该类的 InstanceMirror 调用 type 也会给出相同类型的对象。ClassMirror 类具有 declarations getter，它返回从声明名称到其镜像的映射。可以在 ClassMirror 上调用静态方法。

对于 Dart 中的每种对象类型，都存在一个对应的镜像对象。因此我们有 VariableMirror、MethodMirror、ClassMirror、LibraryMirror 等。在 ClassMirror 类上使用构造函数名称作为符号调用 newInstance 会产生 InstanceMirror；然后您可以通过 reflectee 在真实对象上调用方法。

### There's more...

在使用反射时，我们应该注意一些事情：

- 镜像 API 仍在演进中，因此请期待未来的一些增添和调整。实现在 Dart VM 中运行的代码时最为完整。
- dart2js 中的镜像功能稍显滞后。dart2js 对您的应用执行的压缩和树摇过程通常无法检测到反射代码。因此，运行时使用反射可能会失败，导致 noSuchMethod() 错误。为了防止这种情况发生，请使用 Mirrors 注解，如以下代码所示，这有助于 dart2js 编译器生成更小的代码：

  ```dart
  @MirrorsUsed(override: '*')
  import 'dart:mirrors';
  ```

- 其中一个限制是跨隔离区的反射。在撰写本书时，反射仅在反射代码和被反射对象在同一隔离区运行时才有效。

```dart
@MirrorsUsed(override: '*')
import 'dart:mirrors';
```
