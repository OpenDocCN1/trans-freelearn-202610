The messaging standards are intended to give the framework for exchanging information in a distributed environment. These standards have to be reliable so that the message will be sent only once and only the intended receiver will receive it. This is one of the primary areas where research is being conducted, as everything depends on the messaging ability.

Next, we are going to discuss some of these standards in details. Firstly, we will start with XML-RPC, which later evolved into SOAP.

# XML-RPC

The **XML-RPC** standard was created by Dave Winer in 1998 with Microsoft. The existing RPC systems were very bulky. Therefore, to create a light-weight system, the developer simplified it by specifying only the essentials and defined only a handful of data types and commands. This protocol uses XML to encode its calls to HTTP as a transport mechanism. The message is sent as a POST request in which the body of the request is in XML. A procedure is executed on the server and the value it returns is also formatted into XML. The parameters can be scalars, numbers, strings, dates, as well as complex record and list structures.

As new functionalities were introduced, XML-RPC evolved into what is now known as SOAP, which is discussed next. Still, some people prefer using XML-RPC because of its simplicity, minimalism, and the ease of use.

# SOAP

Initially, SOAP was defined as Simple Object Access Protocol. However, the latest version (SOAP 1.2) has moved beyond its original definition. The SOAP standard was originally designed by four developers with the backing of Microsoft as an object-access protocol. The protocol specifies exchange of XML-based messages over computer networks in transport independent manner. The developers had chosen XML as the standard message format because of its widespread use by major organizations and open source initiatives. Also, there is a wide variety of freely available tools that ease the transition to a SOAP-based implementation.

The concept of **SOAP** is a stateless, one-way message exchange. However, applications can create more complex interaction patterns – such as request-response, request-multiple responses, and so on – by combining such one-way exchanges with features provided by an underlying protocol and application-specific information. SOAP is silent on the semantics of any application-specific data it conveys as it is on issues such as routing of SOAP messages, reliable data transfer, firewall traversal, and so on. However, SOAP provides the framework by which application-specific information may be conveyed in an extensible manner.