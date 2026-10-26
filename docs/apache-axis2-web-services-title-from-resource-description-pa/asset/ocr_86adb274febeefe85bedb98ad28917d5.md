# name

This is the local name of the XML element representing the property of the annotated JavaBean and the default value is the name of the annotated Java attribute.

# namespace

This is the namespace of the XML element representing the property of the annotated JavaBean, and the default value is the namespace of the class containing the Java attribute.

# JSR 250 (Common Annotations)

In this section, we will be looking at some of the useful annotations introduced in the JSR 250 (Common Annotations) specification. We will be looking into the following annotations:

+   - `javax.annotation.Resource`
+   - `javax.annotation.PostConstruct`
+   - `javax.annotation.PreDestroy`

# javax.annotation.Resource

This annotation is used to mark a WebServiceContext resource that is needed by a web service. It is applied to a field or a method for JAX-WS endpoints. The container will inject an instance of the WebServiceContext resource into the endpoint implementation when it is initialized. This annotation is illustrated in the following example:

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