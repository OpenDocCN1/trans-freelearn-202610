

## 重新审视方法

这张图大概粗略地总结了我们目前对方法的理解状况：

![](img/35d01831c19688a44e50bce7c489007a_260_0.png)

如前图所示，方法周围仍有一些问号。我们将完全揭开方法的面纱，了解它们的工作原理，以及方法的其他部分在稍后的章节中为我们做了什么。在 [第 9 章，面向对象编程]，我们将澄清方法之谜的最后几个部分。

那么，Java 方法究竟是什么？方法是一组变量、表达式和控制流语句，它们被打包在一个由名称引导的左花括号和右花括号内。我们已经使用了很多方法，但只是还没有仔细观察它们。

让我们从方法结构开始。

### 方法结构

我们编写的方法的第一部分称为 **签名**。以下是一个假设的方法签名：

```
public boolean addContact(boolean isFriend, String name)
```

如果我们添加一对左花括号和右花括号，{}，以及方法执行的一些代码，那么我们就有了一个完整的方法——**定义**。以下是另一个编造的、但在语法上正确的方法：

```
private void setCoordinates(int x, int y){
// code to set coordinates goes here
}
```

如前面所示，我们可以从代码的另一部分使用我们的新方法，如下所示：

```
// I like it here
setCoordinates(4,6);
// now I am going off to setCoordinates method
// Phew, I'm back again - code continues here
```

在调用 setCoordinates 的地方，程序的执行将分支到该方法内部包含的代码。方法将逐步执行其内部的所有语句，直到到达末尾并将控制权返回给调用它的代码，或者如果遇到 return 语句则更早返回。然后，代码将从方法调用后的第一行继续运行。

以下是另一个方法示例，其中包含使方法返回到调用它的代码的代码：

```
int addAtoB(int a, int b){
    int answer = a + b;
    return answer;
}
```

调用前面方法的代码可能如下所示：

```
int myAnswer = addAtoB(2, 4);
```

显然，我们不需要编写方法来将两个 int 变量相加，但示例帮助我们更深入地了解方法的工作原理。首先，我们传入值 2 和 4。在签名中，值 2 被赋给 int a，值 4 被赋给 int b。

在方法体中，变量 a 和 b 相加并用于初始化新的 int answer 变量。return answer 行将 answer 中存储的值返回给调用代码，导致 myAnswer 被初始化为值 6。

请注意，前面示例中的每个方法签名都略有不同。原因是 Java 方法签名非常灵活，允许我们构建所需的精确方法。

方法签名如何定义方法必须如何调用以及方法必须如何返回值，值得进一步讨论。让我们给签名的每个部分命名，以便我们可以将其分解为块并了解各个部分。

### 提示

以下是一个带有已标记各部分并准备好讨论的方法签名。同时，请查看表格以进一步识别签名的哪一部分是哪一部分。这将使我们后续关于方法的讨论更加直接：

```
modifier | return type | name of method (parameters)
```

以下是我们目前使用的几个示例，以便您可以清楚地识别正在讨论的签名部分：

| 签名部分 | 示例 |
|---|---|
| 修饰符 | public, private, 和 package-private（未指定修饰符） |
| 返回类型 | int，你也可以使用任何 Java 原始类型（例如 boolean、float、double、char、int 和 long）或任何预定义的引用类型（例如 String）和用户定义的类型（例如 Employee、Person） |
| 方法名称 | addContact、setCoordinates 和 addAToB |
| 参数 | (boolean isFriend, String name)、(int x, int y) 和 (int a, int b) |

### 修饰符

在前面的示例中，我们只在几个示例中使用了修饰符，部分原因是方法不必使用修饰符。修饰符是指定哪些代码可以使用（调用）您的方法的一种方式，使用 public 和 private 等修饰符。实际上，普通变量也可以有修饰符，例如：

```
// Most code can see me
public int a;

// Code in other classes can't see me
private String secret = "Shhh, I am private";
```

修饰符（用于方法和变量）是 Java 的一个基本话题，但最好在我们讨论另一个我们已经绕过很多次的重要 Java 话题时再处理——类。我们将在下一章讨论它们。

### 注意

如前面的示例方法以及我们迄今编写的所有代码运行良好这一事实所示，修饰符对于迄今为止的学习不是必需的，尽管它们将是从第 9 章，面向对象编程开始的未来学习的重要组成部分。

### 返回类型

接下来是返回类型。像修饰符一样，返回类型是可选的。所以，让我们更仔细地看看它。我们已经看到我们的方法会执行某些操作。但是，如果我们需要它们所做操作的结果怎么办？我们迄今为止看到的返回类型的最简单示例是：

```
int addAToB(int a, int b){
```

```
int answer = a + b;
return answer;
}
```

这里，签名中的返回类型被高亮显示，因此返回类型是 int。addAtoB 方法将一个可以装入 int 变量的值发送回（返回给）调用它的代码。

返回类型可以是我们迄今看到的任何 Java 类型。但是，方法不必返回值。在这种情况下，签名必须使用 void 关键字作为返回类型。当使用 void 关键字时，方法体不得尝试返回值，因为这会导致编译器错误。但是，它可以不带值地使用 return 关键字。以下是返回类型和 return 关键字使用的一些有效组合：

```
void doSomething(){
// our code

// I'm done going back to calling code here
// no return is necessary
}
```

另一种组合如下：

```
void doSomethingElse(){
// our code

// I can do this as long as I don't try and add a value
return;
}
```

以下代码是另一种组合：

```
void doYetAnotherThing(){
// some code
if(someCondition){

// if someCondition is true returning to calling code
// before the end of the method body
return;
}

// More code that might or might not get executed

return;
}
```

```
/**
 As I'm at the bottom of the method body
 and the return type is void, I'm
 really not necessary but I suppose I make it
 clear that the method is over.
 */
}
```

在这个最后的示例中，我们返回一个 String：

```
String joinTogether(String firstName, String lastName){
 return firstName + lastName;
}
```

我们可以依次调用前面的每个方法，如下所示：

```
// OK time to call some methods
doSomething();
doSomethingElse();
doYetAnotherThing();
String fullName = joinTogether("Alan ","Turing");
// fullName now = Alan Turing
// continue with code from here
```

前面的代码将依次执行每个方法中的所有代码。

### 方法名称

设计我们自己的方法时，方法名称是任意的。但是，使用动词的惯例来清楚地解释方法将做什么。此外，使用名称中第一个单词的首字母小写和后续单词的首字母大写的惯例。这就像我们在学习变量名称时学到的驼峰命名法，例如：

```
XGHHY78802c(){
 // code here
}
```

前面的方法完全合法且可行；但是，让我们看一个使用惯例的更清晰的示例：

```
doSomeVerySpecificTask(){
 // code here
```

