# Lambda expressions

Lambda expressions are the brand new feature of Java. Lambda expressions are introduced in Java 8 and it is a step towards facilitating functional programming in Java.

Lambda expressions help you to define a method without declaring it. So, you do not need a name for the method, return-type, and so on. Lambda expressions, like anonymous inner classes, provide the way to pass behaviors to functions. Lambda, however, is a much more concise way of writing the code.

For example, the preceding example of an anonymous inner class can be converted to Lambda as follows:

```
public class MyFilterImpl {
    public static void main(String[] args) {
        File dir = new File("src/main/java");
        dir.list((dirname,name)->name.endsWith("java")); //Lambda Expression
    }
}
```

Note that the signature of the Lambda expression is exactly matching the signature of the accept method in the FilenameFilter interface.

> One of the huge differences between Lambda and anonymous inner classes lies in the way Lambdas are invoked. Lambdas are invoked dynamically using Method Handle (MH) calls, unlike anonymous inner classes where a separate inner class is created at complie time for reference.