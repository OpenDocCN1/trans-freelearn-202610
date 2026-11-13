

## 数组越界异常

当我们尝试访问不存在的数组元素时，会发生数组越界异常。有时，编译器会为我们捕获它，以防止错误进入可运行的应用。例如，我们可以这样声明数组：

```
int[] ourArray = new int[1000];
int someValue = 1; // Arbitrary value
ourArray[1000] = someValue;
// Won't compile as compiler knows this won't work.
// Only locations 0 through 999 are valid
```

但是，如果我们做这样的事情呢：

```
int[] ourArray = new int[1000];
int someValue = 1; // Arbitrary value
int x = 999;
if (userDoesSomething) { x++; // x now equals 1000
}
```

```
ourArray[x] = someValue;
/*
Array out of bounds exception if userDoesSomething evaluates to
true! This is because we end up referencing position 1000 when
the array only has positions 0 through 999.
Compiler can't spot it. App will crash!
*/
```

避免此问题的唯一方法是了解规则。规则指出，数组从0开始，直到其长度-1。我们也可以使用清晰可读的代码，轻松评估我们所做的事情并更容易发现问题。

## ArrayLists

ArrayList就像打了兴奋剂的普通Java数组。它克服了数组的一些缺点，例如必须预先确定其大小。它添加了一些非常有用的方法来使其数据易于管理，并且它使用了增强版的for循环，比普通for循环更清晰易用。

让我们看看使用ArrayList的一些代码：

```java
// Declare a new ArrayList called myList to hold int variables
ArrayList<int> myList;

// Initialize the myList ready for use
myList = new ArrayList<int>();
```

在前面的代码中，我们声明并初始化了一个名为myList的新ArrayList。我们也可以一步完成，如下面的代码所示：

```java
ArrayList<int> myList = new ArrayList<int>();
```

到目前为止，我们还没有看到特别有趣的东西，所以让我们看看实际上可以用ArrayList做什么。这次我们使用String ArrayList：

```java
// declare and initialize a new ArrayList
ArrayList<String> myList = new ArrayList<String>();

// Add a new String to myList in the next available location
myList.add("Donald Knuth");
// And another
myList.add("Rasmus Lerdorf");
// And another
myList.add("Richard Stallman");
// We can also choose 'where' to add an entry
myList.add(1, "James Gosling");

// Is there anything in our ArrayList?
if (myList.isEmpty()) {
// Nothing to see here
} else {
// Do something with the data
}

// How many items in our ArrayList?
int numItems = myList.size();

// Now where did I put James Gosling?
int position = myList.indexOf("James Gosling");
```

在前面的代码中，我们看到可以在ArrayList对象中使用ArrayList类的一些非常有用的方法。我们可以添加一个项目（myList.add），在特定位置添加它（myList.add(x, value)），检查ArrayList是否为空（myList.isEmpty），查看数组的大小（myList.size()），并获取给定项目的当前位置（myList.indexOf）。

### 注意

ArrayList类中还有更多方法，你可以在http://docs.oracle.com/javase/7/docs/api/java/util/ArrayList.html上阅读它们。到目前为止我们看到的内容足以完成这本书。

有了所有这些功能，我们现在只需要一种动态处理ArrayList的方法。

## 增强for循环

增强for循环的条件如下所示：

```
for (String s : myList)
```

前面的示例将逐个遍历myList中的所有项目。在每一步中，s将保存当前的字符串。

因此，这段代码将在控制台上打印前一节ArrayList代码示例中的所有杰出程序员：

```
for (String s : myList){
    Log.i("Programmer: ","" + s);
}
```

我们也可以在普通数组中使用增强for循环：

```
int[] anArray = new int[];
// We can initialize arrays quickly like this
anArray = {0, 1, 2, 3, 4, 5};
```

```
for (int s : anArray){
    Log.i("Contents = ","" + s);
}
```

还有另一则新闻！

## 数组和ArrayList是多态的

我们已经知道可以将对象放入数组和ArrayList中。但是，多态意味着它们可以处理多种不同类型的对象，只要它们有一个共同的父类型，所有对象都在同一个数组或ArrayList中。

在第9章《面向对象编程》中，你了解到多态大约意味着*不同形式*。但在数组和ArrayList的上下文中，这对我们意味着什么？

简单来说，任何子类都可以用作使用超类的代码的一部分。

例如，如果我们有一个Animal数组，我们可以将任何是Animal类型（即Animal的子类）的对象放入Animal数组中，也许是Cats和Dogs。

这意味着我们可以编写更简单、更易于理解、更容易修改或更改的代码：

```java
// This code assumes we have an Animal class
// And we have a Cat and Dog class that extends Animal
Animal myAnimal = new Animal();
Dog myDog = new Dog();
Cat myCat = new Cat();
Animal[] myAnimals = new Animal[10];
myAnimals[0] = myAnimal; // As expected
myAnimals[1] = myDog; // This is OK too
myAnimals[2] = myCat; // And this is fine as well
```

此外，我们可以为超类编写代码，并依赖于这样一个事实：无论它被子类化多少次，在某些参数范围内，代码仍然有效。让我们继续我们的前一个示例：

```java
// 6 months later we need elephants
// with its own unique aspects
// As long as it extends Animal we can still do this
Elephant myElephant = new Elephant();
myAnimals[3] = myElephant; // And this is fine as well
```

但是，当我们从多态数组中删除一个对象时，必须记得将其强制转换为我们想要的类型。这就像我们每次从XML布局中使用findViewById获取UI元素引用时所做的那样：

```
Cat newCat = (Cat) myAnimals[2];
```

我们刚才讨论的一切对于ArrayLists也成立。有了数组和ArrayLists的这套新工具包，以及它们是多态的这一事实，我们可以继续学习一些我们很快将用于增强我们的Note To Self应用的更多Android类。

## ListView和BaseAdapter

在第5章《现实世界布局》中，我们使用了ScrollView，并用大约20个TextView小部件填充了它，这样我们就可以看到它滚动。我们可以利用刚刚学到的数组和ArrayList知识，创建一个TextView数组并用它们来填充ScrollView。这听起来像是在我们的Note To Self应用中显示笔记摘录的完美解决方案。

我们可以在Java代码中动态创建TextViews，将它们的文本属性设置为笔记的标题，然后将TextViews添加到包含在ScrollView中的LinearLayout中。但是，这并不完美。

### 显示大量小部件的问题

这可能看起来没问题，但如果有几十个、几百个，甚至几千个笔记怎么办？我们不能让几千个TextViews存在内存中，因为Android设备可能会简单地耗尽内存，或者至少在尝试处理如此大量数据的滚动时运行缓慢。

现在考虑一下，我们希望ScrollView中的每个笔记都显示一个关于笔记类型的图片——它是重要的、待办事项列表还是一个想法。再加上描述中的一小段摘录如何。

我们需要设计一些非常巧妙的代码，从数组中加载和销毁Note对象、TextViews和ImageViews。这可以做到，但要高效地做到这一点远非易事。

### 解决方案

幸运的是，这是移动开发者面临的如此常见的问题，以至于Android API内置了解决方案。

我们可以将一个ListView小部件（像加强版的ScrollView）添加到我们的UI布局中，然后使用适配器与它进行交互。我们将使用BaseAdapter类，扩展它，自定义它，然后使用它来控制来自ArrayList的数据并在ListView中显示它。

让我们了解一下ListView和BaseAdapter类的工作原理。

