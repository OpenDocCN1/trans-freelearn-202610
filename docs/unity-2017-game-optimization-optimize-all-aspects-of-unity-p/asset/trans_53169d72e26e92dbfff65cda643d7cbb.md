

Boxing 可以是隐式的，如前面的示例所示，也可以是显式的，通过将类型转换为 `System.Object`。Unboxing 必须始终是显式的，通过将类型转换回其原始类型。每当我们将 Value type 传递给使用 `System.Object` 作为参数的方法时，都会隐式应用 boxing。

诸如 `String.Format()` 之类的方法，其参数为 `System.Object`，就是这种情况的一个示例。我们通常通过传递 Value types（例如 `int`、`float` 和 `bool`）来使用它们以生成字符串。在这些情况下，boxing 会自动发生，导致我们应当注意的额外堆分配。
`Collections.Generic.ArrayList` 是另一个此类示例，因为 `ArrayList` 始终会将其输入转换为 `System.Object` 引用，而不考虑其中存储的类型是什么。

任何时候我们使用将 `System.Object` 作为参数的函数定义，并且传递的是 Value types，我们都应该意识到我们正在由于 boxing 而隐式地导致堆分配。

## 数据布局的重要性

我们的数据在内存中是如何组织的，这一点的重要性可能出乎意料地容易被忽视，但如果处理得当，可以带来相当大的性能提升。应尽可能避免缓存未命中，这意味着在大多数情况下，应该按顺序遍历内存中连续的数据数组，而不是采用任何其他遍历方式。

这意味着数据布局对于垃圾回收也很重要，因为垃圾回收是以迭代方式执行的，如果我们能找到方法让 GC 跳过有问题的区域，那么我们可能节省大量的迭代时间。

本质上，我们希望将大量的 Reference types 与大量的 Value types 分开。如果 Value type（例如 `struct`）中包含甚至一个 Reference type，那么 GC 会将整个对象及其所有数据成员视为间接可引用的对象。在进行 Mark-and-Sweep 时，它必须在继续之前验证对象的所有字段。但是，如果我们将各种类型分离到不同的数组中，那么我们可以让 GC 跳过大部分数据。

例如，如果我们有一个如下代码所示的 `struct` 对象数组，那么 GC 将需要遍历每个 `struct` 的每个成员，这可能相当耗时：

```
public struct MyStruct {
    int myInt;
    float myFloat;
    bool myBool;
    string myString;
}

MyStruct[] arrayOfStructs = new MyStruct[1000];
```

但是，如果我们将这些数据的所有部分重新组织成每种类型的多个数组，那么 GC 将忽略所有原始数据类型，只检查 `string` 对象。以下代码将导致垃圾回收扫描速度快得多：

```
int[] myInts = new int[1000];
float[] myFloats = new float[1000];
bool[] myBools = new bool[1000];
string[] myStrings = new string[1000];
```

这样做之所以有效，是因为我们给 GC 提供了更少的间接引用需要检查。当数据被拆分为单独的数组（Reference types）时，它会找到三个 Value types 数组，标记这些数组，然后立即继续，因为没有理由标记 Value types 数组的内容。它仍然需要遍历 `myStrings` 中的所有 `string` 对象，因为每个对象都是 Reference 类型，并且它需要验证其中是否没有间接引用。从技术上讲，`string` 对象不能包含间接引用，但 GC 工作在一个只知道对象是 Reference type 还是 Value type 的层面上，因此无法区分 `string` 和 `class`。但是，我们仍然使 GC 免于需要遍历额外的 3,000 个数据片段（`myInts`、`myFloats` 和 `myBools` 中的 3,000 个值）。

## Unity API 中的数组

Unity API 中有几个指令会导致堆内存分配，我们应该意识到这一点。这基本上包括所有返回数据数组的内容。例如，以下方法会在堆上分配内存：

```
GetComponents<T>(); // (T[])
Mesh.vertices; // (Vector3[])
Camera.allCameras; // (Camera[])
```

每次调用返回数组的 Unity API 方法时，都会导致该数据的全新版本被分配。应尽可能避免此类方法，或者至少只调用一次并缓存结果，以免不必要地频繁导致内存分配。

还有其他 Unity API 调用，我们将元素数组提供给方法，它会为我们将必要的数据写入数组中。一个示例是将 `Particle[]` 数组提供给 `ParticleSystem` 以获取其 `Particle` 数据。这类 API 调用的好处是我们可以避免重新分配大型数组，而缺点是数组需要足够大以容纳所有对象。如果我们需要获取的对象数量不断增加，那么我们可能会发现自己正在重新分配更大的数组。对于 `ParticleSystem`，我们需要确保创建一个足够大的数组，以包含其在任何给定时间生成的最大 `Particle` 对象数量。

Unity Technologies 在过去曾暗示，他们可能最终会将某些返回数组的 API 调用更改为需要提供数组的形式。后一种形式的 API 对新程序员乍一看可能令人困惑；但是，与前一种形式不同，它允许负责任的程序员使用内存更加高效。

## 使用 InstanceIDs 作为字典键

如在 `Chapter 2, Scripting Strategies` 中提到的，字典用于在两个不同的对象之间建立映射，它们能够非常快速地告诉我们是否存在映射，以及如果存在，该映射是什么。将 `MonoBehaviour` 或 Scriptable Object 引用作为字典的键是常见的做法，但这会引起一些问题。当访问字典元素时，它需要调用 `UnityEngine.Object` 的几个派生方法，这两种对象类型都从 `UnityEngine.Object` 派生。这使得元素比较和映射获取相对较慢。

可以通过使用 `Object.GetInstanceID()` 来改进这一点，它返回一个整数，表示该对象的唯一标识值，该值永不更改，并且在应用程序的整个生命周期中不会在两个对象之间重复使用。如果我们以某种方式缓存此值，并将其用作字典中的键，那么元素比较将比直接使用对象引用快大约两到三倍。

但是，这种方法也有注意事项。如果未缓存实例 ID 值（我们每次需要为字典编入索引时都调用 `Object.GetInstanceID()`），并且我们使用 Mono 编译（而不是 IL2CPP），那么元素获取可能会变得缓慢。这是因为它会调用一些线程不安全的代码来获取实例 ID，在这种情况下，Mono 编译器无法优化循环，因此与缓存实例 ID 值相比会造成一些额外的开销。如果我们使用 IL2CPP 编译（不存在此问题），那么好处仍然不如我们事先缓存值那么大（仅快大约 50%）。因此，我们应该以某种方式缓存整数值，以避免过于频繁地调用 `Object.GetInstanceID()`。

## foreach 循环

`foreach` 循环关键字在 Unity 开发圈子中是一个有点争议的问题。事实证明，Unity C# 代码中实现的许多 `foreach` 循环在调用过程中会产生不必要的堆内存分配，因为它们将 `Enumerator` 对象作为 `class` 分配在堆上，而不是作为 `struct` 分配在栈上。这完全取决于给定集合对 `GetEnumerator()` 方法的实现。

事实证明，Unity 随附的 Mono 版本（Mono 版本 2.6.5）中实现的每个集合都会创建类而不是结构，从而导致堆分配。这包括但不限于 `List<T>`、`LinkedList<T>`、`Dictionary<K, V>` 和 `ArrayList`。

> 注意，在典型数组上使用 `foreach` 循环是安全的。Mono 编译器会悄悄地将数组上的 `foreach` 转换为简单的 for 循环。

成本相当可忽略，因为堆分配成本不会随迭代次数而增加。只分配一个 `Enumerator` 对象，并反复重复使用，总共只消耗几个字节的内存。因此，除非我们的 `foreach` 循环在每次更新时都被调用（这本身通常就很危险），否则在小型项目中成本大多可忽略。将所有内容转换为 `for` 循环所花费的时间可能不值得。但是，这绝对是我们在开始下一个项目时需要牢记的事情。

