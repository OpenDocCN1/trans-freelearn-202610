
## 理解运算符

**运算符**是编程中用于执行操作的符号。

这些运算符是必不可少的工具，可让你执行不同的操作，例如数学计算、比较和逻辑评估，帮助你控制代码的行为以及与数据交互的方式。

首先，让我们来看一下**数学运算符：**

- **加法：** + 号用于按一定量增加变量的值
- **减法：** – 号用于按一定量减少变量的值
- **乘法：** * 号用于将变量的值乘以另一个值
- **除法：** / 号用于将变量的值除以另一个值

我们可以用程序中的 player_level 变量来尝试使用这些运算符。

在我们的代码中，玩家等级为 23。如果我们加上 7，它将变为 30。看这里的 _ready 函数即可看到实际效果：

```
func _ready():
    print(player_name)
    print(player_level)
    print(player_exp)
    print(has_key)

    player_level = player_level + 7
    print(player_level)
```

你现在会注意到输出首先是 23，然后再次打印时，它会变为 30。

需要注意的是，赋值操作是从右到左执行的。在这种情况下，我们取值为 7，将其加到 player_level 的当前值上，然后使用赋值运算符 (=) 将结果存储回左侧的 player_level 变量中。这意味着 player_level 的现有值会被新的结果更新。

编程时，我们总是希望关注效率，这意味着避免重复。如你所见，我们重复了变量名。为了避免这种情况，我们可以使用一个名为快捷运算符的新运算符，并更有效地重构代码如下：

```
#player_level = player_level + 7
player_level += 7
print(player_level)
```

如果我们想将等级降到 0，可以减去 30：

```
player_level -= 30
print(player_level)
```

我们也可以对乘法和除法做同样的操作：

```
25  >>  player_level *= 2
26  >>  print (player_level)
27  >>
28  >>  player_level /= 2
29  >>  print (player_level)
```

图 4.7 – 乘法和除法

你也可以将变量相加。如果我们创建一个名为 spike_damage 的变量，那么每次玩家撞到尖刺时，我们就可以按该值减少其生命值：

```
30  >>  var spike_damage : int = 10
31  >>  print(health)
32  >>  health -= spike_damage
33  >>  print(health)
```

图 4.8 – 使用 spike_damage 变量

在开始解决更复杂的表达式之前，了解数学中运算执行的顺序非常重要。

## 运算顺序

尽管计算机一次从程序顶部到底部运行一行代码，就像烹饪一样，你需要按一定顺序做事（比如烘烤前先混合配料），在数学和编程中，计算有一个特定的执行顺序。

运算顺序告诉计算机先执行计算的哪一部分。这里有一个简单的方法使用 **PEMDAS** 来记住它：

- **括号：** 先计算括号内的内容
- **指数：** 然后计算幂或根（例如对数字求平方）
- **乘法和除法：** 接下来，从左到右进行乘法和除法
- **加法和减法：** 最后，从左到右进行加法和减法

例如，在表达式 3 + 5 * 2 中，计算机首先进行乘法（5 * 2 = 10），然后进行加法（3 + 10 = 13），因为在运算顺序中乘法优先于加法。

理解这一点很重要，因为它确保你的计算是正确的！

## 练习

为了练习使用变量，请尝试创建四个可用于存储玩家信息的变量。这些变量应使用不同的数据类型。

*本练习还引入了一个重要概念：变量作用域 – 请在下面的解释中注意它。*

四个变量如下：

- 玩家姓名
- 玩家等级
- 玩家经验值
- 玩家是否有钥匙？

练习的解决方案如下：

- `var player_name : String = "Riptide"`
- `var player_level : int = 23`
- `var player_exp : float = 123.45`
- `var has_key : bool = false`

我们可以在 _ready 函数中将它们全部打印出来，每行一个：

```
func _ready():
    print(player_name)
    print(player_level)
    print(player_exp)
    print(has_key)
```

执行上述代码将把值打印到控制台。

## 变量作用域

在之前的解决方案中，变量是在 _ready 函数内声明并初始化的。这将变量的作用域限制为局部，意味着它只能在声明它的 _ready 函数内访问。与类变量（在程序顶部定义并可在整个类中访问）不同，局部变量只能在定义它的特定函数或代码块中使用。这就是变量作用域。

![](img/d34207e07c133a9e84d9caf1d479006c_125_0.png)

在下一节中，我们将探索 Godot 中的关系运算符和比较运算符。这些运算符允许你比较值，帮助你的代码通过检查条件（例如一个值是否大于、小于或等于另一个值）来做出决策。

## 关系运算符和比较运算符

通常，我们需要比较值以确定它们是否满足特定条件——真或假。这允许我们的代码分支并根据比较结果执行不同的操作。为了评估这些条件，我们使用关系运算符，帮助我们比较值。

可用于比较的不同运算符如下：

- **相等：** 两个等号 (==) 用于测试两个值是否相同
- **大于：** 使用尖括号 (>)，我们可以比较一个值是否更大
- **大于或等于：** 使用 (>=)，我们可以测试该值是否大于或等于另一个值
- **小于：** 使用尖括号 (<)，我们可以比较一个值是否更小
- **小于或等于：** 使用带等号的尖括号 (<=)，我们可以测试该值是否小于或等于另一个值
- **不相等：** 使用感叹号和等号 (!=)，我们可以测试一个值是否与另一个值不相同

我们将这些运算符与 if 语句一起使用，if 语句是编程中的基础。这些 if 语句允许我们检查条件是否成立，只有在为真时语句内的代码块才会运行。这使我们的游戏更灵活，能够根据测试条件的結果执行不同的操作。

所有这些都在以下代码中演示：

```
var player_lives : int = 3
if health == 0:
    player_lives -= 1

if player_lives < 1:
    print("GAME OVER!")

if player_lives <= 0:
    print("GAME OVER!")

if spike_damage > health:
    print("GAME OVER!")

if player_exp >= 1000:
    player_level += 1
    print("LEVEL UP")

if has_key != true:
    print("DOOR LOCKED")
```

在上述代码中，我们的游戏会根据比较的变量值产生不同的输出。

为了让游戏根据某些条件做出不同的响应，我们可以使用 if、elif 和 else 关键字。if 语句检查条件是否成立并运行其中的代码。如果条件为假，我们可以使用 elif（else if 的简写）来检查另一个条件。最后，else 用作默认值，如果之前的所有条件都不成立则运行。

例如，假设我们想检查玩家拥有的生命值数量并相应地做出响应，如以下代码所示：

```
if player_lives > 0:
    print("Keep playing!")
elif player_lives == 0:
    print("Game Over!")
else:
    print("Invalid number of lives!")
```

上述代码允许玩家在仍有生命值时继续游戏；如果没有，则打印游戏结束，对于异常值则显示错误。

## 练习

为了获得一些实践经验，请编写一个脚本，使用我们创建的 health 变量检查玩家的健康状况。根据其健康状况显示不同的消息。

- 如果 health 为 80 或以上，打印 "You're in great shape!"
- 如果 health 在 50 到 79 之间，打印 "You're doing okay, but be careful!"
- 如果 health 低于 50，打印 "Warning! Your health is low!"
- 如果 health 为 0 或更低，打印 "Game over! You've run out of health."

使用 if、elif 和 else 语句完成此任务。

解决方案如下：

```
# Check the player's health and print the appropriate message
if health >= 80:
    print("You're in great shape!")
elif health >= 50:
    print("You're doing okay, but be careful!")
elif health > 0:
    print("Warning! Your health is low!")
else:
    print("Game over! You've run out of health.")
```

上述代码会根据玩家 health 变量的值显示正确的消息。
