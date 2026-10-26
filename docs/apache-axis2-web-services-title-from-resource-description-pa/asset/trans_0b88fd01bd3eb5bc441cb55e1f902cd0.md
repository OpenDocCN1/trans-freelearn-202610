
``` 
wsimport -d generated http://example.org/stock?wsdl
```

如第二个示例所示，如果你想基于此创建一个服务，`wsimport` 也会生成 .java 文件。其中包含你 Web 服务的已生成接口。只需实现该接口，它就将成为你的 Web 服务类。然后在顶部添加 @WebService 注解，如下所示。

我们将为以下 WSDL 1.1 文档生成服务：StudentsMarksService.wsdl：

```xml
<definitions xmlns="http://schemas.xmlsoap.org/wsdl/"
    xmlns:xs="http://www.w3.org/2001/XMLSchema"
    xmlns:soap="http://schemas.xmlsoap.org/wsdl/soap/"
    xmlns:tns="http://apache.org/jaxws/sample1"
    name="StudentMarksService"
    targetNamespace="http://apache.org/jaxws/sample1">

<types>...</types>
<message>...</message>
<portType>...</portType>
<binding>

    <service name="StudentMarksService">
        <port name="StudentMarksPort" binding="tns:StudentMarksPortBinding">
            <soap:address location="https://localhost/services/StudentMarksService.StudentMarksPort/"/>
        </port>
    </service>
</definitions>
```

该 WSDL 的 XML Schema 类型定义如下所示：

```xml
<types>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
        xmlns:tns="http://apache.org/jaxws/sample1"
        attributeFormDefault="unqualified"
        elementFormDefault="unqualified"
        targetNamespace="http://apache.org/jaxws/sample1">

        <xs:element name="computeAverage" type="tns:computeAverage"/>
        <xs:element name="computeAverageResponse" type="tns:computeAverageResponse"/>
        <xs:element name="computeHighestMarks"
            type="tns:computeHighestMarks"/>
        <xs:element name="computeHighestMarksResponse"
            type="tns:computeHighestMarksResponse"/>
        <xs:complexType name="computeAverage">
            <xs:sequence>
                <xs:element form="qualified" minOccurs="0"
                    name="student"
                    type="tns:student"/>
            </xs:sequence>
        </xs:complexType>
        <xs:complexType name="student">
            <xs:sequence>
                <xs:element name="age" type="xs:int"/>
                <xs:element maxOccurs="unbounded" minOccurs="0"
                    name="marks"
                    nillable="true"
                    type="xs:int"/>
                <xs:element minOccurs="0" name="name"
                    type="xs:string"/>
            </xs:sequence>
        </xs:complexType>
        <xs:complexType name="computeAverageResponse">
            <xs:sequence>
                <xs:element form="qualified" name="average"
                    type="xs:double"/>
            </xs:sequence>
        </xs:complexType>
        <xs:complexType name="computeHighestMarks">
            <xs:sequence>
                <xs:element form="qualified" minOccurs="0"
                    name="student"
                    type="tns:student"/>
            </xs:sequence>
        </xs:complexType>
        <xs:complexType name="computeHighestMarksResponse">
            <xs:sequence>
                <xs:element form="qualified" name="highest"
                    type="xs:int"/>
            </xs:sequence>
        </xs:complexType>
    </xs:schema>
</types>
```

该 WSDL 的消息定义如下所示：

```xml
<message name="computeHighestMarks">
    <part name="parameters" element="tns:computeHighestMarks">
    </part>
</message>
<message name="computeAverage">
    <part name="parameters" element="tns:computeAverage">
    </part>
</message>
<message name="computeHighestMarksResponse">
    <part name="parameters" element="tns:computeHighestMarksResponse">
    </part>
</message>
<message name="computeAverageResponse">
    <part name="parameters" element="tns:computeAverageResponse">
    </part>
</message>
```

该服务在 WSDL 中的 portType 定义如下所示：

```xml
<portType name="StudentMarks">
    <operation name="computeAverage">
        <input message="tns:computeAverage">
        </input>
        <output message="tns:computeAverageResponse">
        </output>
    </operation>
    <operation name="computeHighestMarks">
        <input message="tns:computeHighestMarks">
        </input>
        <output message="tns:computeHighestMarksResponse">
        </output>
    </operation>
</portType>
```

该 WSDL 中的绑定定义如下：

```xml
<binding name="StudentMarksPortBinding" type="tns:StudentMarks">
    <soap:binding style="document"
        transport="http://schemas.xmlsoap.org/soap/http"/>
    <operation name="computeAverage">
        <soap:operation soapAction="urn:getAverage"/>
        <input>
            <soap:body use="literal"/>
        </input>
        <output>
            <soap:body use="literal"/>
        </output>
    </operation>
    <operation name="computeHighestMarks">
        <soap:operation soapAction="urn:getHighestMarks"/>
        <input>
            <soap:body use="literal"/>
        </input>
        <output>
            <soap:body use="literal"/>
        </output>
    </operation>
</binding>
```

让我们使用 wsimport 工具为此 WSDL 生成代码，如下所示：

```
wsimport -s wsdl2java StudentsMarksService.wsdl
```

这将生成 org.apache.jaxws.sample1.StudentMarksPort 和 org.apache.jaxws.sample1.Student 类。现在我们将使用这些已生成的类编写一个示例 Web 服务：

```java
package org.apache.jaxws.sample1.service;

import org.apache.jaxws.sample1.StudentMarksPort;
import org.apache.jaxws.sample1.Student;

import javax.jws.WebService;

@WebService(
    serviceName = "StudentMarksService",
    portName = "StudentMarksPort",
    targetNamespace = "http://apache.org/jaxws/sample1",
    endpointInterface = "org.apache.jaxws.sample1.StudentMarksPort",
    wsdlLocation = "StudentMarksService.wsdl"
)
public class StudentMarksImpl implements StudentMarksPort {

    public double computeAverage(Student student) {
        double totalMarks = 0;
```

## Dispatch 客户端

当你想在 XML 消息级别工作或在 JAX-WS 级别工作而无需任何已生成的工件时，你将使用此客户端。Dispatch 客户端 API 要求应用程序客户端以 XML 的形式构造消息或有效负载，这需要对消息或消息有效负载有详细的了解。Dispatch 客户端支持以下类型的对象：

- `javax.xml.transform.Source`：你将使用 Source 对象使客户端能够直接使用 XML API。你可以在 SOAP 或 HTTP 绑定中使用 Source 对象。
- **JAXB 对象**：你将使用 JAXB 对象，以便客户端可以使用从 XML 模式生成的 JAXB 对象来创建和操作 JAX-WS 应用程序中的 XML。JAXB 对象只能与 SOAP 或 HTTP 绑定一起使用。
- `javax.xml.soap.SOAPMessage`：你将使用 SOAPMessage 对象，以便客户端可以处理 SOAP 消息。你只能将 SOAPMessage 对象与 SOAP 绑定一起使用。
- `javax.activation.DataSource`：你将使用 DataSource 对象，以便客户端可以处理多用途互联网邮件扩展 MIME 消息。DataSource 只能与 HTTP 绑定一起使用。

以下示例演示了如何使用 JAX-WS Dispatch 客户端 API。此客户端与 HelloService 通信，其中包含单个操作，即 greet。该选项接收单个字符串参数。请特别注意突出显示的代码。正如你所见，由于使用了 Message 服务模式，客户端必须构造 SOAP 信封、头、主体和主体内容：

```java
package org.apache.jaxwsclient;

import javax.xml.namespace.QName;
import javax.xml.soap.*;
import javax.xml.ws.Dispatch;
import javax.xml.ws.Service;
import javax.xml.ws.soap.SOAPBinding;
public class DispatchClient {
    public static void main(String[] args) {
        try {
            String endpointUrl = "http://localhost:8080/axis2/services/HelloService";
            QName serviceName = new QName("http://apache.org/types", "HelloService");
            QName portName = new QName("http://apache.org/types", "HelloServiceHttpSoap11Endpoint");

            /** Create a service and add at least one port to it. **/
            Service service = Service.create(serviceName);
            service.addPort(portName, SOAPBinding.SOAP11HTTP_BINDING, endpointUrl);

            /** Create a Dispatch instance from a service.**/
            Dispatch<SOAPMessage> dispatch = service.createDispatch(portName, SOAPMessage.class, Service.Mode.MESSAGE);

            /** Create SOAPMessage request. **/
            // compose a request message
            MessageFactory mf = MessageFactory.newInstance(SOAPConstants.SOAP_1_1_PROTOCOL);

            // Create a message. This example works with the SOAPPART.
            SOAPMessage request = mf.createMessage();
            SOAPPart part = request.getSOAPPart();

            // Obtain the SOAPEnvelope and header and body elements.
            SOAPEnvelope env = part.getEnvelope();
            SOAPHeader header = env.getHeader();
            SOAPBody body = env.getBody();

            // Construct the message payload.
            SOAPElement operation = body.addChildElement("greet", "ns2", "http://apache.org/types");
            SOAPElement age = operation.addChildElement("name");
            age.addTextNode("Packt");
            request.saveChanges();

            /** Invoke the service endpoint. **/
            SOAPMessage response = dispatch.invoke(request);
            System.out.println(response.getSOAPBody().getFirstChild().toString());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

你需要 JDK 1.5 或更高版本来编译上述类。

