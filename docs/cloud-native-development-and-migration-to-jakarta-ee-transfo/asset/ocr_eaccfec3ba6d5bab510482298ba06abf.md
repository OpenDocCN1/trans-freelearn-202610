So, when using `java -jar myapp.jar -Dmp.config.profile=testing`, it will first load the settings from `META-INF/microprofile-config.properties` and then from `META-INF/microprofile-config-testing.properties`. When a property is defined in both files, the latter will override the former.

# MicroProfile Health

Health is a wonderful thing – not just for us humans, but also for our microservices. Once you have deployed your microservices, you want to check their temperature occasionally. Enter the Microprofile Health specification, which allows you to validate the health of your services at regular intervals.

It should be noted that, although this specification exposes an API, it is not meant for human consumption, but more for machine-to-machine. Think of a container management system such as Kubernetes that wants to inquire about the status of certain pods to determine if they are still functioning as expected.

Three sorts of health checks can be performed, as described in the following table:

| Type | Annotation | Usage |
|---|---|---|
| Readiness | `@Readiness` | Determines if the application is ready to process requests |
| Liveness | `@Liveness` | Determines if the application is running |
| Startup | `@Startup` | Determines if the application is still starting up |

Table 11.1 – Health check annotations

For a valid health check, your bean needs to implement the `HealthCheck` interface. This functional interface consists of the `call` method, which returns a `HealthCheckResponse` object.

`HealthCheckResponse` needs to return if a service is either *up* or *down*. It can also supply additional information, such as a name. Furthermore, it can hold more details on failing resources. This can be specified via the `withData` method.

Here is an example of a HealthCheck service:

```
@ApplicationScoped
@Readiness
@Liveness
@Startup
public class HealthService implements HealthCheck {
    @Override
    public HealthCheckResponse call() {
        return HealthCheckResponse.named("Project")
            .up()
            .withData("redis", "Not available")
```