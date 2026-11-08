# Environment variables

If you are publishing your Pentaho reports to the Business Analytics Platform (see Chapter 15, *Using Reports in Pentaho Business Analytics Platform* for an introduction), you can use certain environment variables to customize and control the behavior. The environment variables are available by default in a Pentaho report when it's created, and they live in the Data tab of the report explorer. The following screenshot shows how the report explorer looks with the

![](bbox=[0.425, 0.315, 0.665, 0.525])

environment variable expanded:

Environment variable can be used as usual, by dragging and dropping it into the right place of the report. The following is a list of the variables with brief descriptions of their meanings:

| Name | Description |
|------|-------------|
| `env:hostColonPort` | The hostname and port number for the Business Analytics Platform. |
| `env:locale` | Info about locale of the client requesting the report. `env:locale-language` and `env:locale-short` are other two variables available. |
| `env:pentahoBaseURL` | The complete URL to the Business Analytics Platform, as set in the |