# Multipie chart

The multipie chart renders a group of pie charts, based on a category dataset. This meta-chart renders individual series data as a pie chart, each broken into individual categories within the individual pie charts. The pie chart utilizes the category dataset (previously introduced), together with the common properties defined earlier. In the following description are listed all the differences in terms of available properties:

+   - The title set of properties is provided with one additional properties:

| Name | Type | Description |
|---|---|---|
| `pie-title-font` | String | Sets the font for the pie title for the chart |

+   - The options set of properties is provided with a specific content for this chart type:

| Name | Type | Description |
|---|---|---|
| `by-row` | Boolean | Switches the category and series result sets. Default value is true. |
| `series-color` | Array of strings | A comma-separated list of hexadecimal color values. The colors are used in order, starting with the first value returned by the dataset. |
| `show-labels` | Selection | Defines if labels in correspondence of the value of the chart should be presented or not. Two possible values are defined: `Show Labels` and `Hide Labels`. Default value is `Hide` |