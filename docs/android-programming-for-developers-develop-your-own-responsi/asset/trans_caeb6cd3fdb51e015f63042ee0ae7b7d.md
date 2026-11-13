
```
}
getMyFriendsList(){
// code here
}

startNewMessage(){
// code here
}
```

现在这清晰多了，让我们来看看方法中的参数。

## 参数

我们知道方法可以将结果返回给调用代码。但是，如果我们需要将一些数据值*从*调用代码*与*方法共享，该怎么办？

**参数**允许我们将值与方法共享。我们在学习返回类型时实际上已经见过一个带参数的示例。我们将再次查看同一个示例，不过会更仔细地看一下参数：

```
int addAtoB(int a, int b){
int answer = a + b;
return answer;
}
```

在前面的代码中，参数已被高亮显示。参数包含在紧跟在方法名称后面的括号内（参数放在这里）。请注意，在方法体的第一行中，我们使用 a + b，就好像它们是已声明并初始化的变量一样。这是因为它们确实如此。方法签名中的参数就是它们的声明，而调用方法的代码会对它们进行初始化，如下一行代码所示：

```
int returnedAnswer = addAtoB(10, 5);
```

同样，如前面的示例部分所示，在参数中我们不必只使用 int，可以使用任何 Java 类型，包括我们自己设计的类型。不仅如此，我们还可以混合使用不同的类型。我们还可以根据需要使用任意数量的参数来解决问题。下面的示例可能会有所帮助：

```java
void addToAddressBook(char firstInitial, String lastName, String city, int age){
/*
 all the parameters are now living breathing,
 declared and initialized variables.

 The code to add details to address book goes here.
 */
}
```

现在，是时候认真对待我们的方法体了（显然是指方法体）。

## 在方法体中工作

方法体是我们之前用诸如这样的注释回避的部分：

```
// code here
```

或者诸如这样的注释：

```
// some code
```

但实际上，我们早就知道方法体中该做什么。我们迄今学过的任何 Java 语法都可以在方法体中使用。事实上，如果回想一下，我们迄今编写的所有代码*都*位于方法之中。

接下来我们能做的最好的事情就是编写一些方法体中真正能执行操作的方法。

## 使用方法 – 演示应用

在此，我们将快速构建两个应用来进一步探索方法。首先，我们将通过 Real World Methods 应用深入了解基础知识，然后通过 Exploring Method Overloading 应用亲身体验新主题**方法重载**。

像往常一样，你可以按常规方式打开已经准备好的代码文件。关于方法的下面两个示例可以在下载包中的第 8 章文件夹以及 Real World Methods 和 Exploring Method Overloading 子文件夹中找到。

### 现实世界中的方法

首先，让我们创建一些简单的可用方法，包含返回类型、参数和完全功能的方法体。

开始之前，创建一个名为 Real World Methods 的新 Android 项目，使用 **空白活动** 模板，并将所有其他设置保持默认。单击编辑器上方的 **MainActivity.java** 标签切换到 MainActivity.java 文件，然后即可开始编码。

首先，将这三个方法添加到 MainActivity。将它们添加在 onCreate 方法的闭合花括号 } 之后：

```java
String joinThese(String a, String b, String c){
  return a + b + c;
}

float getAreaCircle(float radius){
  return 3.141f * radius * radius;
}

void changeA(int a){
  a++;
}
```

我们添加的第一个方法名为 joinThese。它将返回一个 String 变量，并需要传入三个字符串变量。在方法体中，只有一行代码。return a + b + c 代码会连接传入的三个字符串，并将连接后的字符串作为结果返回。

下一个方法 getAreaCircle 接受一个 float 变量作为参数，并返回一个 float 变量。该方法体简单地使用圆面积公式，结合传入的半径，然后将答案返回给调用代码。3.141 末尾那个看起来奇怪的 f 是为了让编译器知道该数字的类型是 float。任何浮点数默认都被视为 double 类型，除非它带有尾随的 f。

第三个也是最后一个方法是所有方法中最简单的。请注意，它不返回任何内容；它具有 void 返回类型。我们包含此方法是为了阐明一个关于方法的重要要点，我们希望记住这一点。不过，让我们先在实际操作中看看它，再讨论它。

现在，在 onCreate 中，在调用 setContentView 之后，添加调用我们方法的代码：

```java
String joinedString = joinThese("Methods ", "are ", "cool ");
Log.i("joinedString = ","" + joinedString);

float area = getAreaCircle(5f);
Log.i("area = ","" + area);

int a = 0;
changeA(a);
Log.i("a = ","" + a);
```

运行应用，并在 logcat 窗口中观察输出，如下所示（供你参考）：

```
joinedString =: Methods are cool
area =: 78.525
a =: 0
```

在 logcat 输出中，我们首先看到的是字符串 joinedString 的值。如预期，它是我们传递给 joinThese 方法的三个单词的连接结果。

接下来，我们可以看到 getAreaCircle 确实根据传入的半径长度计算并返回了圆的面积。

输出的最后一行或许最有趣：a=: 0。在 onCreate 方法中，我们声明并将 int a 初始化为 0，然后调用了 changeA。在 changeA 的方法体中，我们使用 a++ 代码对 a 进行了递增。然而，回到 onCreate 中，我们看到当使用 Log 在 logcat 窗口打印 a 的值时，它仍然是 0。

因此，当我们将 a 传递给 changeA 方法时，实际传递的是 a 中存储的值，而不是变量 a 本身。在 Java 中，这被称为按值传递。

### 提示

当我们在方法中声明变量时，它只能在该方法中被看到。当我们在另一个方法中声明变量时，即使名称完全相同，它也不是同一个变量。变量仅在其声明所在的方法中具有作用域。

对于所有基本类型变量，向方法传递它们的工作方式就是这样。对于引用变量，其工作方式略有不同，我们将在下一章中看到它的工作原理。

### 注意

我曾与许多 Java 新手讨论过这个作用域概念。对一些人来说，这似乎显而易见，甚至是自然而然的。然而，对另一些人来说，这却是持续困惑的原因。如果你属于后者，请不要担心，因为我们将在本章后面再多讨论一些，并在后续章节中详细探讨作用域，确保它不再成为问题。

让我们再看一个实际的方法示例，同时学习一些新知识。

## 探索方法重载

如你所见，方法作为一个主题，确实十分多样且深入。但希望，通过一步一步来，我们会发现它们绝不令人生畏。我们还将在下一章回到方法。现在，让我们创建一个新项目来探索方法重载。

创建一个名为 Exploring Method Overloading 的新空白项目，然后我们将编写三个方法，不过带有一点变化。

如我们现在将看到的，只要参数不同，我们就可以创建多个同名方法。本项目中的代码非常简单。其工作原理乍看之下可能略显奇怪，直到我们事后分析为止。

在第一个方法中，我们将简单地将其命名为 printStuff，并通过参数传入一个要打印的 int 变量。将此方法插入到 onCreate 的闭合花括号 } 之后，但在 MainActivity 的闭合花括号 } 之前。请记住以常规方式导入 Log 类：

```java
void printStuff(int myInt){
    Log.i("info", "This is the int only version");
    Log.i("info", "myInt = "+ myInt);
}
```

在这个第二个方法中，我们也将其命名为 printStuff，但传入一个要打印的 String 变量。将此方法插入到 onCreate 的闭合花括号 } 之后，但在 MainActivity 的闭合花括号 } 之前：

```java
void printStuff(String myString){
    Log.i("info", "This is the String only version");
    Log.i("info", "myString = "+ myString);
}
```

在这个第三个方法中，我们将其命名为 printStuff，但传入一个要打印的 String 变量和 int。将此方法插入到 onCreate 的闭合花括号 } 之后，但在 MainActivity 的闭合花括号 } 之前：

```java
void printStuff(int myInt, String myString){
    Log.i("info", "This is the combined int and String version");
    Log.i("info", "myInt = "+ myInt);
    Log.i("info", "myString = "+ myString);
}
```

现在，将此代码插入到 onCreate 方法的闭合花括号 } 之前，以调用方法并在 Android 控制台打印一些值：

```java
// Declare and initialize a String and an int
int anInt = 10;
String aString = "I am a string";
```
