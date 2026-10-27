

# 处理数据类型

## 还有更多...

请关注 Frederik Leonhardt 的 pub 包 `math-expressions`，因为使用复数求值表达式是其目标之一。

## 创建枚举

Dart 中没有内置类型的枚举。枚举提供额外的类型检查，从而有助于增强代码可维护性。那么我们有什么替代方案呢？请查看项目 enum 中的代码，我们希望区分向我们报告的问题的严重程度（我们区分以下级别：TRIVIAL、REGULAR、IMPORTANT 和 CRITICAL）。

### 如何做...

实现创建枚举功能的第一种方式显示在 `enum1.dart` 中：

```
class IssueDegree {
  final _value;
  const IssueDegree(this._value);
  toString() => 'Enum: $_value';

  static const TRIVIAL = const IssueDegree('TRIVIAL');
  static const REGULAR = const IssueDegree('REGULAR');
  static const IMPORTANT = const IssueDegree('IMPORTANT');
  static const CRITICAL = const IssueDegree('CRITICAL');
}

void main() {
  var issueLevel = IssueDegree.IMPORTANT;
  // Warning and NoSuchMethodError for IssueLevel2:
  // There is no such getter ALARM in IssueDegree
  // var issueLevel2 = IssueDegree.ALARM;

  switch (issueLevel) {
    case IssueDegree.TRIVIAL:
      print("Ok, I'll sort it out during lunch");
      break;
    case IssueDegree.REGULAR:
      print("We'll assign it to Ellen, our programmer");
      break;
    case IssueDegree.IMPORTANT:
```

## 还有更多...

Ecma TC52 Dart 标准委员会已调查了将在 2014 年 9 月讨论的枚举提案（请参阅 http://www.infoq.com/news/2014/07/ecma-dart-google），因此为枚举提供内置支持可能将在未来的 Dart 版本中实现。

## 展平列表

列表可以包含其他列表作为元素。这有效地是一个二维列表或数组。展平意味着创建一个包含所有子列表项的单个列表。请查看 flatten_list.dart 中的不同可能性。

### 如何做...

我们在以下代码中展示了三种展平列表的方法：

```
List lst = [[1.5, 3.14, 45.3], ['m', 'pi', '7'], [true, false, true]];
// flattening lst must give the following resulting List flat:
// [1.5, 3.14, 45.3, m, pi, 7, true, false, true]

void main() {
  // 1- using forEach and addAll:
  var flat = [];
  lst.forEach((e) => flat.addAll(e));
  print(flat);
  // 2- using Iterable.expand:
  flat = lst.expand((i) => i).toList();
  // 3- more nesting levels, work recursively:
  lst = [[1.5, 3.14, 45.3], ['m', 'pi', '7'], "Dart", [true, false, true]];
  print(flatten(lst));
}
```

### 它是如何工作的...

最简单的方法使用 forEach 和 addAll 的组合。第二种方法利用了 List 实现 Iterable，因此具有 expand 方法。这里 expand 方法使用其参数作为恒等函数；每个元素都按原样返回，而不应用函数。

如果列表包含作为单个列表元素的 int（或 String、double 等），或存在多级嵌套，则使用 expand 不起作用。在这种情况下，我们必须递归工作，如 flatten 方法中实现的：

```
Iterable flatten(Iterable iterable)
  => iterable.expand((e) => e is List ? flatten(e) : [e]);
```

## 还有更多...

为什么要展平列表的列表？可能有应用需求要这样做，例如，当你在游戏逻辑中使用二维列表或矩阵时，但一个明显的原因是，与列表的列表相比，使用列表的列表在性能上要昂贵得多。

## 在范围内生成随机数

你可能经常想知道如何从某个范围内生成随机数。这正是我们在本配方中要看的内容；我们将获得一个位于最小值 min 和最大值 max 之间的区间内的随机数。

### 如何做...

这很简单；请查看 random_range.dart 中的实现方式：

```
import 'dart:math';

var now = new DateTime.now();
Random rnd = new Random();
Random rnd2 = new Random(now.millisecondsSinceEpoch);

void main() {
  int min = 13, max = 42;
  int r = min + rnd.nextInt(max - min);
  print("$r is in the range of $min and $max"); // e.g. 31
  // used as a function nextInter:
  print("${nextInter(min, max)}"); // for example: 17

  int r2 = min + rnd2.nextInt(max - min);
  print("$r2 is in the range of $min and $max"); // e.g. 33
}
```

## 另请参阅

- 咨询前一个配方以获取有关 Random 用法的更多信息

## 处理日期和时间

几乎在每个数据上下文中都需要适当的日期时间处理。Dart 为我们提供了哪些便利来处理日期和时间？Dart 在 dart:core 中具有出色的内置类 DateTime 和 Duration。作为其众多用途中的几个，你可以执行以下操作：

- 比较和计算日期时间
- 获取日期时间的每个部分
- 使用不同的时区
- 使用 Stopwatch 测量时间段

然而，DateTime 类不提供国际化；为此目的，你需要使用 Dart 团队的 intl 包。

### 如何做...

以下是一些有用的技术（在 date_time.dart 中尝试它们）：

- 将日期格式化（从 DateTime 到字符串）为标准格式，但也可以使用 intl 包格式化为任何格式，如以下代码所示：

```
import 'package:intl/intl.dart';
import 'package:intl/date_symbol_data_local.dart';

    print(now.toIso8601String()); // 2014-05-08T14:03:21.238
    print(now.toLocal());           // 2014-05-08 14:03:21.238
    print(now.toString());          // 2014-05-08 14:03:21.238
    print(now.toUtc());             // 2014-05-08 12:03:21.238Z
    // using intl to format:
    var formatter = new DateFormat('yyyy-MM-dd');
    String formatted = formatter.format(now);
    print(formatted); // 2014-05-08
    print(new DateFormat("EEEEEE").format(now)); // Thursday
    print(new DateFormat("yMMMMEEEEd").format(now)); // Thursday, May 8, 2014
    print(new DateFormat("y-MM-E-d").format(now)); // 2014-05-Thu-8
    print(new DateFormat("jms").format(now)); // 2:19:08 PM
```

```
print(new DateFormat('dd/MMM/y HH:mm:ss').format(now));   // 08/May/2014 14:39:07
// locale data:
initializeDateFormatting("fr_FR", null).then(formatDates);
// ...
}
```

```
formatDates (var d) {
    print(new DateFormat("EEEE", 'fr_FR').format(now));   // jeudi
    print(new DateFormat("yMMMEEEEd", 'fr_FR').format(now)); // jeudi 8 mai 2014
    print(new DateFormat("y-MM-E-d", 'fr_FR').format(now));   // 2014-05-jeu.-8
}
```

解析日期（从字符串到 DateTime），当给定的字符串不在可接受的日期格式之一时；抛出异常并在以下代码中捕获：

```
try {
    DateTime dt = DateTime.parse("2014-05-08T15+02:00");
} on FormatException catch(e) {
    print('FormatException: $e');
}
```

使用时区信息工作：

```
print(now.toLocal());// time in local timezone
print(now.toUtc()); // time in Coordinated Universal Time
print(now.timeZoneName);   // Romance (zomertijd)
print(now.timeZoneOffset); // 2:00:00.000000
```

可以按如下方式找到月份的最后一天：

```
var date = new DateTime(2014,6,0);
print(date.day); // 31
// more general:
var lastDayDateTime = (now.month < 12) ? new DateTime(now.year, now.month + 1, 0) : new DateTime(now.year + 1, 1, 0);
print(lastDayDateTime.day); // 31
```

## 处理数据类型

### 它是如何工作的...

格式化 `DateTime` 实例可以使用来自 `dart:core` 的几个 to... 方法来完成，例如 `toLocal()` 来获取本地时区（由你的机器定义）中的时间。然而，`intl` 为你提供了更大的灵活性；使用特定的格式字符串作为参数创建一个 `DateFormat` 对象，然后调用 `format` 方法以获取格式化的字符串。

例如，`d` 给出天数，`E` 给出缩写的星期几，`EE` 给出完整的星期几名称，`M` 给出月份数字，`y` 给出年份，`H` 给出小时（0-24），`j` 给出小时（0-12）带 AM 或 PM，`m` 给出分钟，等等。许多格式组合是可能的（你也可以构建自己的）。

> 要获得格式组合的完整概述，请参阅
> https://api.dartlang.org/apidocs/channels/stable/dartdoc-viewer/intl/intl.DateFormat。

如果你想要特定语言环境的格式化（例如 `be_NL`、`cs_CZ`、`de_DE` 等），你必须先通过导入 `date_symbol_data_local.dart` 然后调用 `initializeDateFormatting` 来加载特定语言环境数据。第一个参数是特定语言环境。如果你将其值设为 `null`，将加载所有可用的语言环境数据，如以下代码所示：

```
initializeDateFormatting("fr_FR", null).then(formatDates);
```

接下来，在构造 `DateFormat` 对象时，语言环境（例如 `'fr_FR'`）必须作为第二个参数给出：

```
new DateFormat("yMMMMEEEEd", 'fr_FR')
```

`formatDates` 方法展示了以下代码的运行情况：

```
formatDates (var d) {
    print(new DateFormat("EEEEE", 'fr_FR').format(now)); // jeudi
    print(new DateFormat("yMMMMEEEEd", 'fr_FR').format(now)); // jeudi 8 mai 2014
    print(new DateFormat("y-MM-E-d", 'fr_FR').format(now)); // 2014-05-jeu.-8
}
```

将格式良好的字符串解析为 `DateTime` 字符串是通过静态 `DateTime.parse` 方法完成的，该方法接受字符串并生成 `DateTime`。输入格式必须符合 ISO 8601 格式。如果输入格式无法解析，将抛出 `FormatException`，因此使用 try/catch 来处理此问题。

