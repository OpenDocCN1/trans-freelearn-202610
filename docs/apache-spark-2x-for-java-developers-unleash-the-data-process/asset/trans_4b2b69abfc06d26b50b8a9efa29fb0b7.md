
## 泛型

```
List<Integer> listGeneric = new ArrayList<>();
listGeneric.add(1);
listGeneric.add(2);
listGeneric.add("hello"); // won't compile
```

listGeneric 变成整数列表。如果你尝试向其中添加字符串，编译器将不允许。

因此，现在如果你从列表中检索一个元素，它将返回整数类型。不需要类型转换，这将避免我们在运行时遇到 ClassCastException。因此，它提供了编译时安全性：

```
Integer intObject = listGeneric.get(0);
```

## 创建你自己的泛型类型

在本节中，我们将创建一个带有泛型类型 T 的类。该类将包含类型 T 的变量，并且还会有一个返回类型 T 变量的通用方法：

```
public class MyGeneric<T> {
   T input;
   public MyGeneric(T input) {
       this.input = input;
   }
   public T getInput() {
       return input;
   }
}
```

如前所述，MyGeneric 是类。它包含类型 T 的变量 input、用于构建 MyGeneric 类对象并初始化变量 input 的构造函数，以及一个返回 T 值的方法。

现在，让我们实现它：

```
public class MyGenericsDemo {

   public static void main(String[] args) {
       MyGeneric<Integer> m1 = new MyGeneric<Integer>(1);
       System.out.println(m1.getInput());
       // It will print '1'
       MyGeneric<String> m2 = new MyGeneric<String>("hello");
       System.out.println(m2.getInput());
       // It will print 'hello'
   }
}
```

在这里，我们创建了 MyGeneric 类的两个对象，一个是整数类型，另一个是字符串类型。当我们在这两个对象上调用 getInput 方法时，它们将返回各自类型的值。

## 接口

接口是 Java 中的引用类型。它们在 Java 中用于定义类之间的契约。任何实现该接口的类都必须遵守接口定义的契约。

例如，我们有一个名为 car 的接口，如下所示，它包含三个抽象方法：

```
public interface Car {
void shape();
void price();
void color();
}
```

任何实现此接口的类都必须实现此接口的所有抽象方法，除非它是抽象类。接口只能由其他接口实现或扩展，不能被实例化。

在 Java 8 之前，接口只包含抽象方法和 final 变量。在 Java 8 中，接口还可以包含默认方法和静态方法。

### 接口中的静态方法

接口中的静态方法类似于类中的静态方法。用户不能重写它们。因此，即使一个类实现了接口，它也不能重写接口的静态方法。

与类的静态方法一样，接口中的静态方法是通过接口名或类类型引用来调用的，而不是使用实例变量：

```
public interface MyInterface {
static String hello() {
return "Inside static method in interface";
}
void absmethod();
}
```

这是一个包含抽象方法和静态方法的接口：

```
public class MyInterfaceImpl implements MyInterface {
@Override
public void absmethod() {
System.out.println("Abstract method implementation in class");
}
}
```

这是实现前面给出的接口的类，因此为了遵守契约，它必须提供抽象方法的实现，因为它不是抽象类。但是，它不需要实现静态方法。

现在，让我们尝试调用静态方法：

```
public class MyInterfaceDemo {
public static void main(String[] args) {
System.out.println(MyInterface.hello()); // it works
MyInterfaceImpl obj = new MyInterfaceImpl();
obj.hello(); // won't compile
}
}
```

因此，使用接口名引用调用静态方法是有效的，但使用实现了该接口的类的实例变量则无效。

### 接口中的默认方法

默认方法是 Java 8 提供的接口的另一种扩展。接口可以包含默认方法。实现该接口的类可以实现也可以不实现默认方法。默认方法对于扩展现有接口很有用。假设你有一个由许多类实现的接口。现在，你想扩展接口的功能。

你的第一个选择是在接口中添加一个新的抽象方法并强制每个类实现它，这将破坏现有实现并需要大量新代码。第二个选择是在接口中添加默认方法，如果某些类想要重写它们，它可以自由这样做。使用此选项，现有实现也不会破坏。可迭代接口中的 foreach 方法是使用默认方法扩展功能的经典示例。

默认方法可以使用 default 关键字创建。让我们将前面示例中的静态方法更改为默认方法：

```
public interface MyInterface {

default String hello() {
    return "Inside static method in interface";
}

void absmethod();
}
```

实现此接口的类不需要提供此方法的实现：

```
public class MyInterfaceImpl implements MyInterface {

@Override
public void absmethod() {
    System.out.println("Abstract method implementation in class");
}
}
```

现在，让我们尝试调用默认方法：

```
public class MyInterfaceDemo {
public static void main(String[] args) {
    System.out.println(MyInterface.hello()); // won't compile
    MyInterfaceImpl obj = new MyInterfaceImpl();
    obj.hello(); // works
}
}
```

请注意，默认方法是实例方法。它们可以使用实现该接口的类的实例来调用。

### 如果一个类实现了两个具有相同名称和签名的默认方法的接口会怎样？

Java 中的接口用于提供多重继承。类可以实现多个接口。由于 Java 8 中的接口可以包含默认方法，而类不一定需要实现它们。我们可以有两个具有相同名称和签名的默认方法的接口。

让我们创建两个具有相同默认方法的接口：

```
public interface Interface1 {
default void hello() {
System.out.println("Hello from Interface1");
}
}

public interface Interface2 {
default void hello() {
System.out.println("Hello from Interface2");
}
}
```

因此，我们有两个具有相同签名的默认方法 hello 的接口：

```
public class InterfaceImpl implements Interface1, Interface2 {
@Override
public void hello() {
Interface1.super.hello();
}
}
```

现在，一旦你创建了一个同时实现这两个接口的类，编译器将强制你重写 hello 方法。如果你愿意，可以使用前面给出的语法调用其中任何一个接口的 hello，也可以为它提供不同的实现。

## 匿名内部类

匿名内部类本质上是 Java 中的内部类。它们被称为匿名是因为它们没有名称。它们在声明的同时被实例化。

无论何时需要提供接口的实现或覆盖类的抽象方法，都会使用匿名内部类。如果接口或抽象类的实现只需要一次，它们很有用。

让我们以 FilenameFilter 接口为例来讨论它，该接口自 JDK1.0 起已包含在 Java 中。

假设你想打印目录中所有以 java 结尾的文件。

第一个方法是创建一个实现 FilenameFilter 接口的类，如下所示：

```
public class MyFileNameFilter implements FilenameFilter {
@Override
public boolean accept(File dir, String name) {
    return name.endsWith("java");
}
}
```

然后，在你的实现中使用这个类的对象，如下所示：

```
public class MyFilterImpl {
public static void main(String[] args) {
    File dir = new File("src/main/java");
    dir.list(new MyFileNameFilter());
}
}
```

但是，你可能只在代码中一次需要此接口的实现，因此，与其创建一个单独的类，不如如下匿名提供此接口的实现：
