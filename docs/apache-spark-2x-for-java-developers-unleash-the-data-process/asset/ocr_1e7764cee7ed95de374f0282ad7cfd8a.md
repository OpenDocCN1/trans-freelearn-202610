# # Static method in an interface

The static method in an interface is similar to the static method in a class. Users cannot override them. So even if a class implements an interface, it cannot override a static method of an interface.

Like a static method of a class, static method in an interface is called by using an interface name or class type reference, not using instance variables:

```
public interface MyInterface {
static String hello()
{
return "Inside static method in interface";
}
void absmethod();
}
```

Here is an interface with an abstract method and a static method:

```
public class MyInterfaceImpl implements MyInterface{
@Override
public void absmethod() {
System.out.println("Abstract method implementaion in class");
}
}
```

Here is the class that implements the interface given previously, so to adhere to the contract it has to provide implementation of the abstract method as it is not an abstract class. However, it does not need to implement the static method.

Now, let's try to call the static method:

```
public class MyInterfaceDemo {
public static void main(String[] args) {
System.out.println(MyInterface.hello()); // it works
MyInterfaceImpl obj = new MyInterfaceImpl();
obj.hello(); // wont-complie
}
}
```

So, calling the static method using the interface name reference works, but it