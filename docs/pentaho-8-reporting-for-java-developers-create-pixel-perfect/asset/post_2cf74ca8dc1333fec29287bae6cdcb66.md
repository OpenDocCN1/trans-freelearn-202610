
## Thermometer dataset

Last but not least, the thermometer dataset is used to implement a variation of the typical *traffic light* indicator. The dataset requires only a single value to work, and if you have more than one row of data per grouping, the chart collector will aggregate all values together as one big sum.

Due to its specific nature, the dataset is used by only one chart, called thermometer, and Pentaho Reporting builds a thermometer dataset using `ThermometerDataSetCollector`. The thermometer dataset defines a limited set of properties, composed of `value-column` (containing the field name of the data source representing the single value) and the same group properties as of the category dataset. Check the category dataset for a detailed explanation of the properties included in the group.

## Style and layout common properties

As you saw in Chapter 5, *Design and Layout in Report Designer*, for the report bands, and also in the case of charts, the style and layout properties are grouped for better management and understanding. In the case of charts, the style and layout properties can be managed in the left panel of the chart editor. The groups of properties are managed in exactly the same way as the element properties panel: same behavior and same user experience. Of course, the properties are different with different meanings and behaviors, and here in this section, you will learn more about them, discovering all the relevant details.

As we saw for the report bands, most charts share a common set of properties. The following properties are common across all the charts. Any exceptions are mentioned as part of the specific chart type.

## Required properties

A set of properties defined as mandatory for the correct behavior of the chart:

| Name | Type | Description |
|------|------|-------------|
| `no-data-message` | String | The message to display if no data is available to render the chart |

## Title properties

A set of properties defined to control the (optional) title of the chart:

| Name | Type | Description |
|---|---|---|
| `chart-title` | String | The title of the chart, which is rendered in the report |
| `chart-title-field` | String | A field of the data source representing the chart title |
| `title-font` | String | The chart title's font family, size, and style |

## General properties

A set of properties defined to control the general properties of the chart:

| Name | Type | Description |
|------|------|-------------|
| 3-D | Boolean | If set to true, renders the chart in a 3D perspective. The default value is set to false. |
| bg-color | String | The background color for the chart. |
| bg-image | String | Path to the background image for the chart. |
| show-border | Boolean | Flag indicating if the border of the entire chart is requested to be shown. Default is false. |
| border-color | String | The color for the chart's border. |
| plot-bg-color | String | Sets the plot's background color to the specified color. If not set, defaults to white. |
| plot-fg-alpha | Decimal | Sets the alpha value of the plot foreground color relative to the plot background. The default value is set to 1.0. |
| plot-bg-alpha | Decimal | Sets the alpha value of the plot background color relative to the chart background color. The default value is set to 1.0. |
| anti-alias | Boolean | If set to true, renders chart fonts as anti-aliased. The default value is set to true. |

## Legend properties

A set of properties defined to control the legend of the chart:

| Name | Type | Description |
|---|---|---|
| show-legend | Flag | Flag controlling the view/hide of the legend. Default value is true. |
| location | Selection | Position of the legend with respect to the chart. Possible values are: north, west, south, east, top, left, bottom, and right. |
| legend-bg-color | String | Background color. The color picker discussed in Chapter 5, Design and Layout in Report Designer is available in this property. |
| legend-border | String | Flag indicating if the border of the legend should be visible or not. Default value is true. |
| legend-font | String | Font used in the legend. |
| legend-font-color | String | Font color. The color picker discussed in Chapter 5, Design and Layout in Report Designer is available in this property. |

## Scripting properties

A set of properties defining the scripts that control the processing of the chart:

| Name | Type | Description |
|---|---|---|
| Chart Post-Processing Script Language | Selection | Language of scripting used in the field. Possible values are: `beanshell`, `groovy`, `netrexx`, `javascript`, `xslt`, `jac1`, and `jython`. |
| Chart Post-Processing Script | String | Source code for post-processing the chart rendering. |

## Bar chart

Starting from here, you will see all the details related to the different types of charts. The bar chart is the first one, displaying individual bars broken into individual categories and series. Bar charts are useful for comparing relative sizes of data across categories. The bar chart utilizes the category dataset (previously introduced), together with the common properties defined earlier.

In the following description are listed all the differences in terms of available properties:

- The options set is introduced here with the following properties:

| Name | Type | Description |
|---|---|---|
| `stacked` | Boolean | If set to true, the series values will appear layered on top of one another instead of being displayed relative to one another. |
| `stacked-percent` | Boolean | If set to true, determines the percentages of each series, and renders the bar height based on those percentages. The property `stacked` must be set to true for this property to have an effect. |
| `horizontal` | Boolean | If set to true, the chart's x and y axis are rotated horizontally. The default value is set to false. |
| `bar-borders` | Boolean | If set to false, clears the default rendering value of the chart border. |
| `max-bar-width` | Decimal | The maximum width of a bar in pixels. Unless set, bars will generally expand to the available space within the chart. |
| `series-color` | Array of strings | The color in which to render each series. The default for the first series colors are defined but you can change it using a color editor. |

- The shadow set is introduced here with the following properties:

| Name | Type | Description |
|---|---|---|
| `shadow-visible` | Boolean | If set to true, displays the shadow for the chart. This value is set to false by default. |
| `shadow-color` | String | Color of the shadow. |
| `shadow-x-offset` | Integer | Distance of the shadow on the x axis in pixels. This value is set to 0 by default. |
| `shadow-y-offset` | Integer | Distance of the shadow on the y axis in pixels. This value is set to 0 by default. |

- The general set has the same properties as described in the general common set, and some additional properties listed as follows:

| Name | Type | Description |
|---|---|---|
| `gridlines` | Boolean | If set to true, displays category grid lines. This value is set to true by default. |
| show-plot-border | Boolean | If set to false, clears the default rendering value of the chart border. This value is set to false by default. |

- The X axis set is introduced here with the following properties:

| Name | Type | Description |
|---|---|---|
| x-axis-title | String | Sets the title for the primary numeric data axis; typically the x axis in a horizontally-oriented chart. |
| x-font | String | Font used for the x axis. The property uses a font picker for a friendly user experience. |
| x-tick-font | String | Font used for the range ticks in the chart. The property uses a font picker for a friendly user experience. |
| x-tick-format | String | The label format used for displaying category items within the chart. This property is required if you would like to display the category item values. The following parameters may be defined in the format string to access details of the item: 0 to access the series name detail of an item; 1 to access the category detail of an item 2 to access the item value details of an item. |
| x-axis-label-width | Decimal | Sets the maximum category label width ratio, which determines the maximum length each category label should render in. This might be useful if you have really long category names. The invisible default value is 20.0. If your labels are being truncated, try increasing this value. |
| x-axis- |  |  |
