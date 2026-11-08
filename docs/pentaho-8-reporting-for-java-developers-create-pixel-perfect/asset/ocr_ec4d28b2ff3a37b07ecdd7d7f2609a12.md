# Parameters

Parameters are defined as children of the `data-definition/parameter-definition` elements. There are two types of parameters: plain parameters and list parameters. Plain parameters are represented in XML as a `plain-parameter` element. Plain parameters define the following attributes:

| Name | Description |
|---|---|
| `default-value` | The default value of the parameter |
| `mandatory` | If set to true, a value for this parameter is required before the report is rendered |
| `name` | The parameter name |
| `type` | The fully qualified Java type of the parameter |

List parameters are represented in XML as a `list-parameter` element. In addition to sharing the same attributes as the plain parameter, list parameters also define the following attributes:

| Name | Description |
|---|---|
| `query` | The query name providing the list of values to choose from. |
| `allow-multi-selection` | If set to true, this attribute allows multiple selections of parameter values from the query results, creating an array of objects versus a single object. |