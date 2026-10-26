# operationName

This property defines the name of the wsdl:operation in WSDL 1.1 corresponding to this method. The namespace of this name is taken from the value WebService.targetNamespace or its default value. The default value of this property is the name of the annotated Java method.

# action

This property defines the action of this method's web service operation. It defaults to "" (an empty string).

# exclude

This property is used for excluding certain methods from a web service. Such methods will not show up as web service operations. The default value is false.

Let us look at an example where the WebMethod annotation is used along with some of its attributes. In this example, the add method will be exposed as a web service operation with the name addInt and the action will be set to urn:addInt. The print method will not be exposed as a web service, as the exclude attribute of the WebMethod annotation has been set to false.

```
@WebService
public class CalculatorImpl {
    @WebMethod(operationName="addInt", action="urn:addInt")
    public int add(int a, int b) {
        print(a, b);
        return a + b;
    }

    @WebMethod(exclude="true")
    public void print(int a, int b) {
        System.out.println("a=" + a + ", b=" + b);
    }
}
```

# javax.jws.OneWay

This annotation is used for marking a method as a one-way method. The corresponding web service operation will be an in-only operation. This annotation has no properties and is defined as follows:

```
@Retention(value=RetentionPolicy.RUNTIME)
@Target({METHOD})
public @interface Oneway {
}
```