
# 大多数有用的函数都接受参数

大多数有用的函数都接受**参数**。参数是从调用者接收值的变量。它们让函数每次被调用时都能处理不同的数据，如下面的代码所示：

```
def greet(name):
    print("Hello,", name)
greet("Alice")
greet("Bob")
```

在前面的代码中，`name` 是一个参数。当你调用 `greet("Alice")` 时，值 `"Alice"` 被传入函数并赋值给 `name`。

## 从函数返回值

函数也可以使用 `return` 语句返回值。返回值将数据发送回调用者并立即结束函数，如下面的代码所示：

```
def add(a, b):
    return a + b
result = add(3, 4)
print(result)
```

函数不仅仅执行动作。它们可以计算结果并将其交还给调用者。

一个函数可以有多个 `return` 语句，但每次调用只会执行其中一个，如下面的代码所示：

```
def check_age(age):
    if age >= 18:
        return "Adult"
    else:
        return "Minor"
```

一旦执行 `return`，函数就会退出。

## 局部变量和作用域

在函数内部定义的变量是该函数的局部变量。这意味着它们只在函数运行时存在，并且无法从外部访问，如下面的代码所示：

```
def example():
    x = 10
    print(x)
example()
# print(x)  # This would cause an error
```

这个概念称为**作用域**。局部变量有助于防止程序不同部分之间的意外干扰。如果所有变量都是全局的，即使是小程序也会变得非常脆弱。

函数可以调用其他函数，包括你自己编写的函数，如下面的代码所示：

```
def square(n):
    return n * n
def print_square(n):
    result = square(n)
    print(result)
print_square(5)
```

这种组合在 Python 中很常见。只做一件事且做好的小函数比试图一次做所有事情的大函数更容易测试，也更容易复用。

## 默认参数值

还值得注意的是，Python 允许**默认参数值**，如下面的代码所示：

```
def greet(name, greeting="Hello"):
    print(greeting, name)
greet("Alice")
greet("Bob", "Hi")
```

默认值使函数灵活而不复杂。

# 使用类将数据和行为组合在一起

到目前为止，你的 Python 代码一直围绕变量和函数组织。

这对小脚本效果很好。但随着程序增长，你通常需要一种方法将相关的数据和行为组合在一起。这就是类的用途。

一个**类**让你定义一种新的对象，它具有：

- **数据**（称为属性）
- **行为**（称为方法）

这是一个基本的类：

```
class Task:
    def __init__(self, title):
        self.title = title
        self.completed = False
    def mark_complete(self):
        self.completed = True
```

这个类是创建任务对象的模板。

现在你可以创建并使用一个任务：

```
task = Task("Write Chapter 8")
print(task.title)        # Write Chapter 8
print(task.completed)    # False
task.mark_complete()
print(task.completed)    # True
```

### `__init__` 方法

`__init__` 方法在你创建新对象时运行：

```
task = Task("Write Chapter 8")
```

它用于设置对象的初始状态：

- `self` 指代当前对象
- `self.title` 和 `self.completed` 是存储在该对象上的属性

`__init__` 方法定义了类的每个实例初始拥有什么数据。`self` 是你正在操作的对象。

当你写：

```
task.mark_complete()
```

Python 将其转换为：

```
Task.mark_complete(task)
```

所以 `self` 是被传入的实例。你不必将其命名为 `self`，但你应该这么做。这是一个强约定，每个 Python 开发者都期望如此。

### 使对象可打印

默认情况下，打印对象不是很有帮助：

```
print(task)
```

你可能会看到类似这样的内容：

```
<__main__.Task object at 0x...>
```

你可以通过定义一个特殊方法来改进这一点：

```
def __str__(self) -> str:
    status = "✓" if self.completed else "✗"
    return f"{status} {self.title}"
```

现在：

```
print(task)
```

输出：

```
✗ Write Chapter 8
```

让我们分解一下这个方法：

- `__str__` 是一个特殊的方法名，当 Python 需要将对象转换为字符串时会调用它
- `self` 是实例
- `-> str` 是一个类型提示，表示该方法返回一个字符串

因此，每当你在类中看到 `def __str__(self) -> str:` 时，它的意思是：*当 Python 需要该对象的字符串表示时，调用此方法并期望返回一个字符串*。

你会在真实的 Python 代码中经常看到这种模式，特别是在 Django 等框架中。

### 方法是类内部的函数

方法是在类内部定义的函数：

```
def mark_complete(self):
    self.completed = True
```

唯一的区别是它作用于一个实例（`self`）。

这意味着你学到的关于函数的一切仍然适用，包括参数、返回值和控制流。区别在于方法通常处理对象的内部数据。

### 添加更多结构

你可以自然地扩展类：

```
class Task:
    def __init__(self, title, priority="medium"):
        self.title = title
        self.completed = False
        self.priority = priority
    def mark_complete(self):
        self.completed = True
    def is_high_priority(self):
        return self.priority == "high"
    def __str__(self) -> str:
        status = "✓" if self.completed else "✗"
        return f"{status} [{self.priority}] {self.title}"
```

现在对象拥有多个属性、多个方法和清晰的字符串表示。

### 为什么类对 Django 很重要

简单的脚本不需要类。

但 Django 到处都在使用类。

例如：

- 数据库模型是类
- 视图通常是类
- 表单是类

一个 Django 模型可能如下所示：

```
class Task(models.Model):
    title = models.CharField(max_length=200)
    completed = models.BooleanField(default=False)
    def __str__(self):
        return self.title
```

如果你理解属性、方法、`__init__` 和 `__str__`，这段代码就不神秘也不吓人了。它只是一个带有一些框架特定行为的类。

**良好实践**：当你要建模具有状态的事物（如任务、用户或订单）、想要将相关数据和行为分组、或想要同类对象的多个实例时，使用类。当简单的函数或字典足够时，或添加类会使代码更难理解时，避免使用类。

# Python 的可选类型提示

**类型提示**是可选的注解，用于描述变量、参数和返回值的预期类型。它们有助于提高清晰度和工具支持，但不会改变 Python 的运行方式。

类型提示让你直接在代码中记录意图：

```
def total_runtime(minutes: list[int]) -> int:
    return sum(minutes)
```

前面的代码表示：

- `minutes` 参数预期是一个整数列表
- `total_runtime` 函数预期返回一个整数

无论有没有提示，代码运行完全相同。Python 在运行时不强制执行它们。它们是给人类和工具看的，不是给解释器的。

添加类型提示是为了解决大型 Python 代码库中的实际问题。带有类型提示的函数签名告诉读者如何使用它，而无需阅读实现。

像 VS Code 这样的代码编辑器可以利用类型提示来：
