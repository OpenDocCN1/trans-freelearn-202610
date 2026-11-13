

## 类对象的声明、初始化和使用

请记住，Soldier 只是一个类，而非实际可用的对象。它是士兵的蓝图，而非实际的士兵对象，就像 int、String 和 boolean 不是变量一样；它们只是我们可以用来创建变量的类型。以下是如何从我们的 Soldier 类中创建 Soldier 类型的对象：

```
Soldier mySoldier = new Soldier();
```

在代码的第一部分中，Soldier mySoldier 声明了一个名为 mySoldier 的新 Soldier 类型变量。代码的最后一部分，new Soldier()，会调用一个称为 **构造函数** 的特殊方法，该方法由编译器自动为所有类生成，用于创建实际的士兵对象。如您所见，构造函数方法与类同名。我们将在本章后面更深入地探讨构造函数。

当然，中间的赋值运算符 = 会将第二部分的结果赋给第一部分。下图总结了所有这些信息：

![](img/35d01831c19688a44e50bce7c489007a_289_0.png)

这与处理普通变量的方式并无太大区别，只是代码行末尾有点奇怪的圆括号 ()。这些圆括号稍后将加以解释。

嗯，看起来有点像方法调用，对吧？

就像普通变量一样，我们也可以分两步来做，像这样：

```
Soldier mySoldier;
mySoldier = new Soldier();
```

这是如何为我们假设的类分配和使用变量的方式：

```
mySoldier.health = 100;
mySoldier.soldierType = "sniper";
```

```
// Notice that we use the object name mySoldier.
// Not the class name Soldier.
// We didn't do this:
// Soldier.health = 100;
// ERROR!
```

在前面的代码中，点运算符 . 用于访问类的变量；调用方法也是如此，再次使用对象名而不是类名，后跟点运算符：

```
mySoldier.shootEnemy();
```

我们可以用一张图来总结点运算符的使用：

![](img/35d01831c19688a44e50bce7c489007a_291_0.png)

### 提示

我们可以从类的方法能 *做* 什么的角度来思考，并将其实例/成员变量视为了解其自身 *知道* 的内容。

我们还可以继续创建另一个 Soldier 对象并访问其方法和变量：

```
Soldier mySoldier2 = new Soldier();
mySoldier2.health = 150;
mySoldier2.soldierType = "special forces";
mySoldier2.shootEnemy();
```

重要的是要认识到 mySoldier2 是一个完全独立的对象，拥有与 mySoldier 完全分离的实例变量：

![](img/35d01831c19688a44e50bce7c489007a_292_0.png)

同样重要的是，上述代码不会写在类本身内。例如，我们可以在名为 `Soldier.java` 的外部文件中创建 `soldier` 类，然后使用刚才看到的代码，可能在我们的 `MainActivity` 类中。

当我们在实际项目中不久编写第一个类时，这会变得更加清晰。

还要注意，所有操作都直接在对象 _本身_ 上进行。我们必须创建类的对象才能使其有用。

### 注意

一如既往，这条规则也有例外。但例外属于少数情况，我们将在本章后面探讨例外。事实上，迄今为止本书中我们已经见过两个例外：`Toast` 和 `Log` 类。关于它们具体的情况将很快加以说明。

让我们通过实际编写一个类来更深入地探讨基本类。

## 基本类小型应用

将要使用我们应用程序的将军们将需要不止一个 Soldier 对象。在我们即将构建的应用程序中，我们将实例化并使用多个对象。我们还将演示在变量和方法上使用点运算符，以显示不同对象拥有各自独立的实例变量。

您可以在代码下载中获取此示例的完整代码。它位于 chapter 9/Basic Classes 文件夹中。或者，继续阅读以创建您自己的工作示例。

使用空白活动创建一个项目。将应用程序命名为 BasicClasses。现在，我们创建一个名为 Soldier 的新类：

- 1. 在项目资源管理器窗口中右键单击 com.gamecodeschool.basicclasses（或您的包名）文件夹。
- 2. 选择 New | Java Class。
- 3. 在 Name 字段中键入 soldier 并左键单击 OK。

新类已为我们创建，带有一个代码模板，准备好让我们在其中放入实现，就像下图所示：

![](img/35d01831c19688a44e50bce7c489007a_294_0.png)

请注意，Android Studio 已将该类放置与应用程序其余部分相同的包中。现在，我们可以编写其实现。

按所示，在 Soldier 类的开头和结束花括号内编写以下类实现代码。新输入的代码已突出显示：

```
public class Soldier {
    int health;
    String soldierType;

    void shootEnemy(){
    //let's print which type of soldier is shooting
    Log.i(soldierType, " is shooting");
    }
}
```

现在我们有了一个类，一个 Soldier 类型未来对象的蓝图，我们可以开始建立我们的军队了。在编辑器窗口中，左键单击选项卡 **MainActivity.java**。我们将在调用 setContentView 之后的 onCreate 方法中编写此代码。键入此代码：

```
// first we make an object of type soldier
Soldier rambo = new Soldier();
rambo.soldierType = "Green Beret";
rambo.health = 150;
// It takes a lot to kill Rambo

// Now we make another Soldier object
Soldier vassily = new Soldier();
vassily.soldierType = "Sniper";
vassily.health = 50;
// Snipers have less health

// And one more Soldier object
Soldier wellington = new Soldier();
wellington.soldierType = "Sailor";
wellington.health = 100;
// He's tough but no green beret
```

### 提示

如果您还没有这样做，现在是开始利用 Android Studio 自动完成功能的绝佳时机。请注意，在您声明并创建新对象后，只需开始输入对象名称，所有自动完成选项都会显现出来。

![](img/35d01831c19688a44e50bce7c489007a_296_0.png)

现在我们拥有了这个极其多样化且有些不太可能的军队，我们可以使用它并验证每个对象的身份。在上一步的代码下方键入以下代码：

```
Log.i("Rambo's health = ", "" + rambo.health);
Log.i("Vassily's health = ", "" + vassily.health);
Log.i("Wellington's health = ", "" + wellington.health);
```

```
rambo.shootEnemy();
vassily.shootEnemy();
wellington.shootEnemy();
```

现在，我们可以运行我们的应用程序了。所有输出都将显示在 logcat 控制台窗口中。

它的工作原理如下。首先，我们为新的 Soldier 类创建了一个模板。然后，我们实现了该类，包括声明两个字段（成员变量），一个 int 和一个 String，分别名为 health 和 soldierType。

我们的类中还有一个名为 shootEnemy 的方法。让我们再看看它并检查正在发生什么：

```
void shootEnemy(){
//let's print which type of soldier is shooting
Log.i(soldierType, " is shooting");
}
```

在方法体中，我们先将字符串 soldierType 打印到控制台，然后是任意字符串 " is shooting"。这里的妙处在于，字符串 soldierType 将取决于我们在哪个对象上调用 shootEnemy 方法而有所不同。

接下来，我们声明并创建了三个新的 Soldier 类型对象。它们分别是 rambo、vassily 和 wellington。最后，我们为每个对象初始化了不同的 health 和 soldierType 值。

以下是输出：

```
Rambo's health = 150
Vassily's health = 50
Wellington's health = 100
Green Beret: is shooting
Sniper: is shooting
Sailor: is shooting
```

请注意，每次访问每个士兵对象的 health 变量时，都会打印我们为其分配的值，这表明虽然三个对象属于同一类型，但它们是完全独立的单独实例/对象。

或许更有趣的是对 shootEnemy 的三次调用。我们的每个 Soldier 对象的 shootEnemy 方法都会逐个被调用，并且我们将 soldierType 变量打印到控制台。该方法为每个单独的对象提供了相应的值，进一步证明了我们有三个不同的对象（类的实例），尽管它们是从相同的 Soldier 类创建的。

我们看到了每个对象与其他对象是完全独立的。然而，如果我们想象应用程序中有整支士兵对象大军，那么我们意识到将需要学习处理大量对象（以及普通变量）的新方法。

想想仅管理 100 个独立的士兵对象。那当我们有数千个对象时又会怎样？此外，这不够动态。我们目前编写代码的方式依赖于我们（开发者）确切知道将军（用户）将指挥的士兵的详细信息。我们将在第 13 章《处理和显示数据数组》中看到这方面的解决方案。

