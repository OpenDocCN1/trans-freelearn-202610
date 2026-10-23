Gauges expose the return value of the annotated method, like so:

```
@Gauge(name = "total_projecrs", tags={"projects"}, unit = MetricUnits.
NONE)
public Long getProjectCount()
```

# @Metric

@Metric contains the metadata information when a metric is requested to be injected. It can be applied to fields and parameters.

When using this annotation, the implementation needs to inject the request metric into the field or parameter:

```
@Inject
@Metric(name = "total_projects")
Long projectCount;
```

The value of the gauge that we defined previously is injected into the projectCount field. This allows your code to directly act upon collected metrics.

# @Timed

@Timed tracks the duration of the annotated object. It can be applied to methods, constructors, and types. It tracks how often the object was invoked and how long it took to complete.

Here's an example of its usage:

```
@POST
@Timed(name = "project_create_duration")
public Response post(Project project)
```

# Telemetry Tracing

The telemetry specification is new in MicroProfile 6. Telemetry assembles information regarding the applications running in a microservice environment. This information can come from various sources, most notably logs, traces, and metrics.

MicroProfile already had a specification for tracing known as the Open Tracing specification. Replacing one with the other did cause some stir in the community, but in the end, the choice of OpenTelemetry makes sense. It is a CNCF-supported project (https://www.cncf.io/projects/opentelemetry/). Furthermore, it is vendor- and tool-agnostic and can be integrated with a broad number of open source and commercial projects.