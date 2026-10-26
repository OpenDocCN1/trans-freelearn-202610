

```
@SOAPBinding(style=SOAPBinding.Style.RPC, use=SOAPBinding.Use.LITERAL)
public class CalculatorImpl {
    public int add(int a, int b) {
        return a + b;
    }
}
```

## JSR 224（JAX-WS）注解

本节将介绍 JSR 224（JAX-WS）规范中引入的一些有用注解。这些注解用于补充 JSR-181 注解。我们将介绍以下注解：

- javax.xml.ws.BindingType
- javax.xml.ws.RequestWrapper
- javax.xml.ws.ResponseWrapper
- javax.xml.ws.ServiceMode
- javax.xml.ws.WebEndpoint
- javax.xml.ws.WebFault
- javax.xml.ws.WebServiceClient
- javax.xml.ws.WebServiceProvider
- javax.xml.ws.WebServiceRef

### javax.xml.ws.BindingType

BindingType 注解用于指定 Web 服务端点实现类所使用的绑定。它只有一个属性 `value`，该属性是绑定 URI，默认值为 SOAP 1.1 / HTTP。下面来看一个简单示例：

```
@WebService
@BindingType(value="http://www.w3.org/2003/05/soap/bindings/HTTP/")
public class AddNumbers {
    public int add(int a, int b) {
        ...
    }
}
```

如 BindingType 注解的 `value` 属性所示，部署后的端点将使用基于 HTTP 的 SOAP1.2 绑定。

### javax.xml.ws.RequestWrapper 和 javax.xml.ws.ResponseWrapper

javax.xml.ws.RequestWrapper 注解用于为 SEI 中的方法指定运行时使用的请求包装器 Bean。javax.xml.ws.ResponseWrapper 注解用于为 SEI 中的方法指定运行时使用的响应包装器 Bean。这些注解包含三个属性，即 `localName`、`targetNamespace` 和 `className`。

#### localName

此属性定义表示请求/响应包装器的 XML Schema 元素的本地名称，默认值为由 javax.jws.WebMethod 定义的 `operationName`。

#### targetNamespace

此属性定义请求/响应包装器元素的命名空间，默认值为 SEI 的 `targetNamespace`。

#### className

此属性定义表示请求/响应包装器的类名称。

请看以下示例：

```
public interface AddNumbersImpl {
    @WebMethod
    @WebResult(targetNamespace = "")
    @RequestWrapper(localName = "addNumbers",
        targetNamespace = "http://axis.apache.org/axis2/jaxws",
        className = "org.apache.axis2.jaxws.sample.AddNumbers")
    @ResponseWrapper(localName = "addNumbersResponse",
        targetNamespace = "http://server.fromjava/",
        className = "org.apache.axis2.jaxws.sample.AddNumbersResponse")
    public int addNumbers(
        @WebParam(name = "arg0", targetNamespace = "")
        int arg0,
        @WebParam(name = "arg1", targetNamespace = "")
        int arg1);
}
```

### javax.xml.ws.ServiceMode

Web 服务端点可以通过实现 Provider 接口，在 XML 消息级别进行工作。具体来说，可以实现 `Provider<Source>`、`Provider<SOAPMessage>` 或 `Provider<DataSource>`。端点通过这种低级别的通用 API 访问消息或消息负载。所有 Provider 端点都必须具有 `@WebServiceProvider` 注解。

`@ServiceMode` 注解用于说明端点希望访问消息（Service.Mode.MESSAGE）还是消息负载（Service.Mode.PAYLOAD）。如果端点上没有 `@ServiceMode` 注解，则默认值为消息负载。此注解包含一个 `value` 属性，用于指示 Provider 端点是要访问完整消息（MESSAGE）还是仅访问消息负载（PAYLOAD），默认值为 PAYLOAD。下面的示例演示了此注解的使用方式：

```
@ServiceMode(value=Service.Mode.PAYLOAD)
public class AddNumbersImpl implements Provider<Source> {
    public Source invoke(Source source) throws RemoteException {
    }
}
```

### javax.xml.ws.WebEndpoint

此注解用于标注生成的服务接口中的 `getPortName()` 方法。此注解中指定的信息足以唯一标识 WSDL 中 `wsdl:service` 内的 `wsdl:port` 元素。它只有一个 `name` 属性，用于定义 WSDL 中表示相应端口的 XML 元素的本地名称，默认值为 `""`（空字符串）。下面的示例演示了此注解：

```
@WebServiceClient(name = "AddNumbersImplService",
targetNamespace = "http://axis.apache.org/axis2/jaxws", wsdlLocation = "http://localhost:8080/services/addnumbers?wsdl")
public class NumberAdder {
    ...
    @WebEndpoint(name = "AddNumbersImplPort")
    public AddNumbersImpl getAddNumbersImplPort() {
        ...
    }
}
```

### javax.xml.ws.WebFault

JAX-WS 工具会在根据 WSDL 生成的、特定于服务的异常类中生成此注解，用于自定义故障元素的本地名称和命名空间名称，以及故障 Bean 的名称，并将特定于服务的异常标记为由 WSDL 生成。它包含三个属性：`name`、`targetNamespace` 和 `faultBean`。

#### name

此属性定义 WSDL 中表示相应故障的 XML 元素的本地名称，默认值为 `""`（空字符串）。

#### targetNamespace

此属性定义 WSDL 中表示相应故障的 XML 元素的命名空间，默认值为 `""`（空字符串）。

#### faultBean

此属性定义表示故障消息详细信息的 Java 类的限定名称，默认值为空字符串 `""`。

下面的示例展示了此注解的使用方式：

```
@javax.xml.ws.WebFault(name="AddNumbersException",
                targetNamespace="http://axis.apache.org/axis2/jaxws")
public class AddNumbersException_Exception extends Exception {
    private org.apache.axis2.jaxws.AddNumbersException fault;
}
```

### javax.xml.ws.WebServiceClient

此注解中指定的信息足以唯一标识 WSDL 文档中的 `wsdl:service` 元素。此 `wsdl:service` 元素表示由生成的服务接口提供客户端视图的 Web 服务。此注解可以包含三个属性：`name`、`targetNamespace` 和 `wsdlLocation`。

#### name

此属性定义 WSDL 中 `wsdl:serviceName` 的本地名称，默认值为 `""`。

#### name

这是表示带注解 JavaBean 属性的 XML 元素的本地名称，默认值为带注解 Java 属性的名称。

#### namespace

这是表示带注解 JavaBean 属性的 XML 元素的命名空间，默认值为包含该 Java 属性的类的命名空间。

## JSR 250（通用注解）

本节将介绍 JSR 250（通用注解）规范中引入的一些有用注解。我们将介绍以下注解：

- `javax.annotation.Resource`
- `javax.annotation.PostConstruct`
- `javax.annotation.PreDestroy`

### javax.annotation.Resource

此注解用于标记 Web 服务所需的 WebServiceContext 资源。对于 JAX-WS 端点，它可以应用于字段或方法。容器在端点实现初始化时，会将 WebServiceContext 资源的实例注入端点。下面的示例展示了此注解：

```
@WebService
public class HelloImpl {
    @Resource
    private WebServiceContext context;

    public String echo(String name) {
        ...
    }
}
```

```
temp = student.getMarks()[i];
    if (temp > highest) {
        highest = temp;
    }
}
    return highest;
}
```

Student 类是一个简单的 JavaBean，包含相应学生的姓名、年龄和成绩。其声明如下：

```
// Student.java
package org.apache.jaxws.sample;

public class Student {
    private String name;
    private int age;
    private int[] marks;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public int[] getMarks() {
        return marks;
    }

    public void setMarks(int[] marks) {
        this.marks = marks;
    }
}
```

编译给定的类，根据编译后的代码创建 JAR 文件，然后将其部署到 Axis2 的 `repository/servicejars` 目录中。启动 Axis2，并访问 URL http://localhost:8080/axis2/services/StudentMarksService?wsdl 查看 WSDL。

