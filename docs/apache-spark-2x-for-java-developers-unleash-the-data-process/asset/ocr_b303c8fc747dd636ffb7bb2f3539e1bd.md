```
```java
public static void main(String[] args) {
    System.out.println(MyInterface.hello()); // won't compile
    MyInterfaceImpl obj = new MyInterfaceImpl();
    obj.hello(); // works
}
}```
```

Note that default methods are instance methods. They can be called using the instance of the class that implements the interface.