# 11
Developing JAX-WS Web Services

Java API for XML-based web services is a popular standard, targeting Java developers who write web services and web service clients. The conventional **contract first** approach, which consists of defining the web service contract in a WSDL and then generating the appropriate code from those WSDLs, is not the most natural way of programming for Java developers. The JAX-WS specifications allow you to write a regular Java code and annotate the code appropriately, in order to make them into web services. Therefore, JAX-WS is a **code first** approach for developing web services and clients in Java. The ease of development is achieved through writing everything as Java classes and not having to write any specific deployment descriptor file such as `services.xml` files utilized by Axis2 AAR services. Older Java Web Service specifications such as JAX-RPC required the web service classes to implement a specific interface. With JAX-WS, there is no such restriction and regular POJOs can be made into web services with minimal changes. JAX-WS also allows contract first development. You will see how JAX-WS-annotated web services can be generated starting from WSDL using the wsimport tool later in this chapter. At the time of writing, Axis2 only supports the JAX-WS 2.0 specification.

In this chapter, we will be looking at:

+   - JAX-WS Annotations
+   - Server-side JAX-WS
+   - Client-side JAX-WS

By the end of this chapter, you will have a good understanding about JAX-WS annotations and how to develop JAX-WS services and clients in Apache Axis2.