# Pie chart

The pie chart displays a sliced, multi-colored pie with individual slices that consist of individual series information. The pie chart utilizes the pie dataset (previously introduced), together with the common properties defined earlier. In the following description are listed all the differences in terms of available properties:

+   - The required set of properties is provided with a couple of additional properties for this chart type:

| Name | Type | Description |
|------|------|-------------|
| `ignore-nulls` | Boolean | Specifies whether nonexistent data points should be referenced in the chart. Default value is true. |
| `ignore-zeros` | Boolean | Specifies whether a data value of zero should be referenced in the chart. Default value is true. |

+   - The options set of properties is provided with a specific content for this chart type:

| Name | Type | Description |
|------|------|-------------|
| `slice-colors` | Array of strings | A comma-separated list of hexadecimal color values. The colors are used in order, starting with the first value returned by the dataset. |
| `show-labels` | Selection | Defines if labels in correspondence of the value of the |