# 1 ASP.NET Core 2 中的新特性
ASP.NET Core 2 有哪些新特性？

ASP.NET Core 框架的版本 2 中有许多变化。同时，它的一些支持技术也进行了大量改进。现在是尝试它的好时机，因为它的代码已经稳定，变更速度也有所放缓。

原始发布候选版与 ASP.NET Core 版本 1 之间存在显著差异，而版本 1 和版本 2 之间也有进一步改动。其中一些变化存在争议，尤其是与工具相关的变化；然而，.NET Core 的适用范围已经大幅增长，这是一件好事。

版本 1 和版本 2 之间的一个引人注目的差异是从基于 JavaScript 对象表示法（JSON）的新项目格式改回了基于可扩展标记语言（XML）的 csproj 格式（有些人会说是倒退）。不过，与原始 .NET Framework 中使用的格式相比，它是一个简化并精简过的版本。

不同的 .NET Framework 之间出现了标准化趋势，因此 .NET Core 2 的 API 覆盖面要大得多。被称为 .NET Standard 2 的接口规范涵盖了 .NET Core、.NET Framework 和 Xamarin 的交集。此外，还正在努力将可扩展应用程序标记语言（XAML）纳入 XAML 标准，该标准将可跨通用 Windows 平台（UWP）和 Xamarin.Forms 应用程序使用。

C# 和 .NET 可以用于各种不同的平台以及大量不同的用例，从服务器端 Web 应用程序到移动应用，甚至游戏（使用 Unity 3D 等游戏引擎）。在本书中，我们将专注于 Web 应用程序编程，特别是让 Web 应用良好运行的通用方法。这意味着我们还将介绍客户端 Web 浏览器中的 JavaScript 脚本编写，以及其中涉及的性能影响。

本书不仅涉及 C# 和 ASP.NET。它采用整体性性能方法，旨在让你了解各种相关主题。我们没有足够的空间深入探讨所有内容，因此这里的想法是帮助你发现一些有用的工具、技术和技巧。

在本章中，我们将逐一介绍 .NET Core 和 ASP.NET Core 从版本 1 到版本 2 的变化。我们还将了解 C# 语言的一些新特性。其中有许多有用的新增内容，也有大量性能改进。

在本章中，我们将涵盖以下主题：

- .NET Core 2.0 的新特性
- ASP.NET Core 2.0 的新特性
- 性能改进
- .NET Standard 2.0
- C# 6.0 的新特性
- C# 7.0 的新特性
- JavaScript 相关考量

## ASP.NET Core 2 中的新特性

Core 家族中有两个主要产品。第一个是 .NET Core，它是一个提供基础库的底层框架。它可以用于编写控制台应用程序，同时也是更高级别应用程序框架的基础。

第二个是 ASP.NET Core，它是一个用于构建在服务器上运行并为客户端（通常是 Web 浏览器）提供服务的 Web 应用程序的框架。这最初是 .NET Core 唯一的用途，直到它的范围扩大到处理更多不同的场景。

我们将分别介绍每个框架在较新版本中的差异。.NET Core 中的变化也适用于 ASP.NET Core，除非你是在 .NET Framework（4 版）之上运行它。

## .NET Core 2 中的新特性

.NET Core 2 的主要焦点是范围的巨大扩展。所包含的 API 数量是原来的两倍以上，并且它支持 .NET Standard 2（本章稍后介绍）。你还可以引用 .NET Framework 程序集，而无需重新编译。只要这些程序集只使用 .NET Core 中已实现的 API，这应该就能正常工作。

这意味着更多 NuGet 包将与 .NET Core 兼容。在之前的版本中，检查你喜欢的库是否受支持总是一个挑战。作者为此建立了一个列出包兼容性的仓库。你可以在 https://anclafs.com/ 和 https://github.com/jpsingleton/ANCLAFS 上找到 ASP.NET Core 库和框架支持（ANCLAFS）列表。如果你想进行更改，请发送拉取请求。希望将来所有包都能支持 Core，那么这个列表就不再需要了。

现在 .NET Core 支持 Visual Basic，并且支持更多 Linux 发行版。你还可以使用 Visual Studio 2017（仅企业版）进行实时单元测试，就像以前的 NCrunch 扩展一样。我们将在第 3 章“设置你的环境”中进一步讨论工具，其中还将涵盖容器化。

## 性能改进

.NET Core 2.0 中一些更有趣的变化是相对于原始 .NET Framework 的性能改进。许多框架数据结构的实现都进行了调整。一些已经获得速度提升或内存减少的类和方法包括：

- `List<T>`
- `Queue<T>`
- `SortedSet<T>`
- `ConcurrentQueue<T>`
- `Lazy<T>`
- `Enumerable.Concat()`
- `Enumerable.OrderBy()`
- `Enumerable.ToList()`
- `Enumerable.ToArray()`
- `DeflateStream`





## C# 6

尽管 C# 的最新版本是 7，但 C# 6 中有一些非常实用的功能却经常被忽视。此外，7 中的一些新增内容是对 6 中添加的功能的改进，如果没有上下文就意义不大。我们将在此快速介绍 C# 6 的几个功能，以防你不知道它们有多有用。

### 字符串插值

字符串插值是熟悉的字符串格式方法的更优雅、更易于使用版本。无需再单独提供要嵌入到字符串占位符中的参数，现在可以直接将它们嵌入到字符串中。这更易读且不易出错。

让我们用一个例子来演示。考虑以下将异常嵌入到字符串中的代码：

```
catch (Exception e)
{
    Console.WriteLine("Oh dear, oh dear! {0}", e);
}
```

这会将第一个（也是本例中唯一一个）对象嵌入到字符串中，位置由零标记。看起来很简单，但如果有许多对象并且想要在开头添加另一个对象，就会变得复杂。你必须正确重新编号所有占位符。

相反，现在你可以在字符串前加上美元字符并直接在其中嵌入对象。以下代码的行为与前一个示例相同：

```
catch (Exception e)
{
    Console.WriteLine($"Oh dear, oh dear! {e}");
}
```

异常上的 `ToString()` 方法会输出所有所需信息，包括名称、消息、堆栈跟踪以及任何内部异常。无需手动拆解它；如果你手动操作甚至可能遗漏某些内容。

你也可以使用你熟悉的相同格式字符串。考虑以下以自定义方式格式化日期的代码：

```
Console.WriteLine($"Starting at: {DateTimeOffset.UtcNow:yyyy/MM/dd HH:mm:ss}");
```

该功能在构建时语法略有不同。因此，请注意任何可能不正确旧的博客文章或文档。

### 空条件运算符

空条件运算符是一种简化空值检查的方法。你现在可以在内联中检查空值，而无需使用 if 语句或三元运算符。这使其更容易在更多地方使用，并有望帮助你避免令人 dreaded 的空引用异常。

你可以避免像以下代码那样进行手动空值检查：

```
int? length = (null == bytes) ? null : (int?)bytes.Length;
```

现在只需添加一个问号即可简化为以下语句：

```
int? length = bytes?.Length;
```

### 异常过滤器

你可以使用 `when` 关键字更轻松地过滤异常。你不再需要捕获所有感兴趣的异常类型，然后在 catch 块内部手动过滤它。这是在 VB 和 F# 中已有的功能，因此 C# 终于赶上来了，这很不错。

这种方法有一些小好处。例如，如果你的过滤器不匹配，则异常仍会被同一 try 语句中的其他 catch 块捕获。你也不需要记住重新抛出异常以避免它被吞掉。这有助于调试，因为 Visual Studio 不会在你使用 throw 时中断。

例如，你可以检查异常中是否有消息并不同地处理它，如下所示：

```
catch (Exception e) when (e?.Message?.Length > 0)
```

该功能在开发期间使用了不同的关键字（`if`）。因此请注意网上任何旧信息。

需要注意的一点是，依赖特定的异常消息是脆弱的。如果你的应用程序已本地化，则消息可能与你预期的语言不同。这在异常过滤之外也同样适用。

### 字面量

在代码中指定字面量值时，有一些小的额外功能和可读性增强。你可以指定二进制字面量，这意味着你不再需要用不同的基数来表示它们。你还可以在字面量内的任何位置放置下划线，以使数字更易于阅读。下划线会被忽略，但允许你将数字按约定分组。这对于新的二进制字面量特别适用，因为它可能非常冗长，列出所有那些零和一。

以下示例使用新的 `0b` 前缀指定二进制字面量，该字面量将作为整数呈现在字符串中：

```
Console.WriteLine($"Binary solo! {0b0000001_00000011_000000111_00001111}");
```

你也可以对其他基数执行此操作，例如此整数，其格式使用千位分隔符：

```
Console.WriteLine($"Over {9_000:#,0}!"); // Prints "Over 9,000!"
```

### 元组

C# 7 中的一大新功能是对元组的支持。元组是值的集合，现在可以直接从方法调用中返回它们。你不再受限于返回单个值。以前，你可以通过几种不太理想的方式解决此限制，包括创建自定义复杂对象返回，也许使用 Plain Old C# Object (POCO) 或 Data Transfer Object (DTO)，它们是同一回事。你也可以使用 ref 或 out 关键字传入引用，这仍然不理想，尽管语法有所改进。

C# 6 中有 `System.Tuple`，但并不理想。它是框架功能，而不是语言功能，并且项目只有编号而没有命名。借助 C# 7 元组，你可以命名对象，并且它们是匿名类型的绝佳替代方案，特别是在 LINQ 查询表达式和 lambda 函数中。例如，如果你只想在可用数据的一个子集上工作，也许在过滤使用 O/RM 的数据库表时，例如 Entity Framework，那么你可以使用元组来实现。

以下示例从方法返回元组。你可能需要为此添加 `System.ValueTuple` NuGet 包：

```
private static (int one, string two, DateTime three) GetTuple()
{
    return (one: 1, two: "too", three: DateTime.UtcNow);
}
```

你还可以在字符串插值中使用元组，并且所有值都将被呈现，如下所示：

```
Console.WriteLine($"Tuple = {GetTuple()}");
```

### 输出变量

如果你想将参数传递给方法以进行修改，则始终需要先声明它们。这不再必要，你可以在传入时直接声明变量。你还可以使用下划线声明要丢弃的变量。这在你不想使用返回值时特别有用，例如在原生框架数据类型中的一些 try parse 方法中。

在这里，我们在不先声明 `dt` 变量的情况下解析日期：

```
DateTime.TryParse("2017-08-09", out var dt);
```

在此示例中，我们测试整数，但我们不在乎它是什么：

```
var isInt = int.TryParse("w00t", out _);
```

### 引用

你现在也可以从方法按引用返回值以及消费它们。这有点像在 C 中使用指针，但更安全。例如，你只能返回传递给该方法的引用，并且不能修改引用以指向内存中的不同位置。这是一个非常专业的特性，但在某些特定情况下，它可以显著提高性能。

考虑以下方法：

```
private static ref string GetFirstRef(ref string[] texts)
{
    if (texts?.Length > 0)
    {
        return ref texts[0];
    }
    throw new ArgumentOutOfRangeException();
}
```

你可以像这样调用此方法，第二个控制台输出行会显示不同（one 而不是 1）：

```
var strings = new string[] { "1", "2" };
ref var first = ref GetFirstRef(ref strings);
Console.WriteLine($"{strings?[0]}"); // 1
first = "one";
Console.WriteLine($"{strings?[0]}"); // one
```

### 模式

另一个重大补充是，你现在可以使用 `is` 关键字在 C# 7 中匹配模式。这简化了对空值的测试和与类型的匹配等操作。它还让你可以轻松使用转换后的值。这是使用完整多态性（其中派生类可以被视为基类并重写方法）的更简单替代方案。然而，如果你控制代码库并且能够正确使用多态性，那么你仍应这样做并遵循良好的面向对象编程 (OOP) 原则。

在以下示例中，模式匹配用于解析未知对象的类型和值：

```
private static int PatternMatch(object obj)
{
    if (obj is null)
    {
        return 0;
    }
    if (obj is int i)
    {
        return i++;
    }
    if (obj is DateTime d ||
        (obj is string str && DateTime.TryParse(str, out d)))
    {
        return d.DayOfYear;
```




