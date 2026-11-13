

## 方法也有访问修饰符

我们在上一章中已经简要提到过，方法具有访问修饰符。这很合理，因为方法是我们的类能够 _做_ 的事情。我们希望控制类的用户能做什么和不能做什么。

这里的大致思路是，有些方法只在内部执行操作，因此类的用户不需要它们，而有些方法则是类的用户使用你的类的基础。

方法的访问修饰符与类变量的相同。这使得事情易于记忆，但同样表明，成功的封装是设计问题，而不是遵循任何特定规则集的问题。

例如，只要此方法位于公共类中，任何其他类都可以使用它：

```
public useMeEverybody(){
//do something everyone needs to do here
}
```

而此方法只能由创建它的类在内部使用：

```
private secretInternalTask(){
/*
do something that helps the class function internally
Perhaps, if it is part of the same class,
useMeEverybody could use this method...
On behalf of the classes outside of this class.
Neat!
*/
}
```

而下一个未指定访问修饰符的方法具有默认可见性。它只能由同一包中的其他类使用。如果我们扩展包含此默认访问方法的类，该类将无法访问此方法：

```
fairlySecretTask(){
// allow just the classes in the package
// Not for external use
}
```

在继续之前，作为最后一个示例，这里是一个 protected 方法，仅对包可见，但可被扩展它的我们的类使用，就像 onCreate 一样：

```
protected packageTask(){
// Allow just the classes in the package
// And you can use me if you extend me too
}
```

## 方法访问修饰符总结

应选择方法访问权限以最好地强制执行我们已经讨论的原则。它应为类的用户提供其所需的访问权限，且最好不多提供任何权限。这样一来，我们就实现了封装目标，例如保护代码的内部工作机制免受使用它的程序的干扰，原因都已经讨论过了。

## 使用获取器和设置器访问私有变量

现在，我们需要考虑，如果将变量隐藏为私有是最佳实践，那么我们如何在不破坏封装的情况下允许访问它们呢？如果 Hospital 类的一个对象想要访问 Soldier 类型对象的 health 成员变量，以便将其增加，该怎么办？health 变量应该是私有的，因为我们不希望任何一段代码都能更改它。

为了尽可能多地将成员变量设为私有，同时仍允许对其中一些变量进行某种有限的访问，我们使用 **获取器** 和 **设置器**。获取器和设置器只是获取和设置变量值的方法。

这不是我们必须学习的什么特殊的新 Java 技术。这只是使用我们已知内容的一种约定。让我们使用我们的 Soldier 和 Hospital 类示例来查看获取器和设置器。

在此示例中，我们的两个类分别在各自的文件中创建，但位于同一个包中。首先，这是我们假设的 Hospital 类：

```
class Hospital{
    protected void healSoldier(Soldier soldierToHeal){
    int health = soldierToHeal.getHealth();
    health = health + 10;
    soldierToHeal.setHealth(health);
    }
}
```

我们对 Hospital 类的实现只有一个方法，即 healSoldier。它将 Soldier 对象的引用作为参数接收。因此，此方法将作用于传入的任何 Soldier 对象：vassily、wellington、Rambo 或其他任何人。

它还有一个局部 health 变量，用于暂存并增加士兵的健康值。在同一行中，它将 health 变量初始化为 Soldier 对象的当前健康值。Soldier 对象的 health 是私有的，因此改用公共获取器方法 getHealth。

然后，health 增加 10，setHealth 设置器方法将新的复活后的健康值重新加载回 Soldier 对象。

关键在于，尽管 Hospital 对象可以更改 Soldier 对象的 health，但它只能在获取器和设置器方法的范围内这样做。可以编写获取器和设置器方法来控制和检查可能出现的错误或有害值。

接下来，看看我们假设的 Soldier 类，其获取器和设置器方法采用最简单的实现：

```
public class Soldier{
    private int health;
    public int getHealth(){
        return health;
    }
    public void setHealth(int newHealth){
        // Check for stupid values of newHealth
        health = newHealth;
    }
}
```

我们有一个名为 health 的实例变量，它是私有的。私有意味着它只能由 Soldier 类的方法更改。然后我们有一个公共 getHealth 方法，它不出所料地返回私有 health int 变量中保存的值。由于此方法是公共的，任何可以访问 Soldier 类型对象的代码都可以使用它。

接下来，实现 setHealth 方法。同样，它是公共的，但这次它将 int 数据类型作为参数，并将传入的内容分配给私有 health 变量。在更贴近实际的示例中，我们会在此编写更多代码，以确保传入的值在我们预期的范围内。

现在，我们声明、创建并赋值，来创建我们两个新类的对象，并看看我们的获取器和设置器如何工作：

```
Soldier mySoldier = new Soldier();
// mySoldier.health = 100;//Doesn't work, private
```

```
// we can use the public setter setHealth()
mySoldier.setHealth(100);//That's better

Hospital militaryHospital = new Hospital();
```

```
// Oh no mySoldier has been wounded
mySoldier.setHealth(10);
```

```
/*
 Take him to the hospital
 But my health variable is private
 And Hospital won't be able to access it
 I'm doomed - tell Laura I love her

 No wait-what about my public getters and setters?
 We can use the public getters and setters from another class
 */
```

```
militaryHospital.healSoldier(mySoldier);
```

```
// mySoldiers private variable health has been increased by 10
// I'm feeling much better thanks!
```

我们可以看到，可以直接在 Soldier 类型的对象上调用公共 setHealth 和 getHealth 方法。不仅如此，我们可以调用 Hospital 对象的 healSoldier 方法，传入 Soldier 对象的引用，该对象也可以使用公共获取器和设置器来操纵私有 health 变量。

我们可以看到，私有 health 变量易于访问，但完全受 Soldier 类设计者的控制。

如果你想玩弄这个示例，第 9 章文件夹中的代码包里有一个名为 GettersAndSetters 的可运行应用代码。我添加了几行代码来在控制台打印输出。我们故意以这种方式讲解，是为了让代码的关键部分尽可能清晰。

## 注意

获取器和设置器有时也被称为它们更准确的名称，**访问器** 和 **修改器**。我们将继续使用获取器和设置器。我只是觉得你可能想了解这些行话。

再次，我们的示例和解释可能会引发更多问题。这很好。

通过使用封装特性（例如访问控制），有点像是签署一份关于如何使用和访问类及其方法和变量的非常重要的协议。该契约不仅仅是关于当前的协议，更是对未来的一种隐含担保。随着本章的深入，我们会看到，有更多方法可以细化和加强这个契约。

## 提示

在需要的地方使用封装，或者，当然，如果你受雇主要求使用它。通常，在小型学习项目中（例如本书中的一些示例），封装属于大材小用，当然，除非你学习的主题本身就是封装。

我们学习这些 Java OOP 内容是基于这样的假设：总有一天你会想要编写更复杂的应用，无论是在 Android 上还是使用 OOP 的其他平台上。此外，我们将使用大量使用 OOP 的 Android API 类，这也将帮助我们理解那时发生的事情。通常，在本书中，我们会在实现完整项目时使用封装，但在展示用于演示单个理念或主题的小代码示例时，往往会忽略它。

