
- The `HTML` set of properties doesn't provide the `append-header` property
- The `Excel` set of properties only provides the `formula` property

## Bar-sparkline, line-sparkline, and pie-sparkline

This section is about `bar-sparkline`, `line-sparkline`, and `pie-sparkline` elements. These elements contain exactly the same set of common properties as introduced earlier in the chapter, except for some differences, described as follows:

- The `font` set of properties is not defined for these elements
- `Sparklines` set is introduced here with the following properties:

| Name | Type | Description |
|---|---|---|
| `low-color` | String | Sets the color for the lowest slice. No default is provided for the property. Valid for `pie-sparkline` only. |
| `medium-color` | String | Sets the color for the medium slice. No default is provided for the property. Valid for `pie-sparkline` only. |
| `high-color` | String | Sets the color for the higher slice. No default is provided for the property. Valid for `pie-sparkline` and `bar-sparkline`. |
| `lastcolor` | String | Sets the color for the bar/line. No default is provided for the property. Valid for `bar-sparkline` and `line-sparkline`. |

- The `text` set of properties contains only `h-align`, `v-align`, `text-color`, and `bg-color`
- The `text-spacing` set of properties is not defined for these elements
- The `links` set of properties does not provide the `pdf-bookmark` property
- The `excel` set of properties is not defined for these elements
- `Common properties in Attributes` defines also the `image-map` property as a string, optionally developed in different languages

- `Sparklines` in the `Attributes` tab set is introduced here with the following properties:

| Name | Type | Description |
|---|---|---|
| `spacing` | Integer | Sets the spacing (in pixels) between bars for `bar/line` `sparkline`. Default is 0. |
| `low-slice`, `medium-slice`, `high-slice` | Decimal | Angle for the low/medium/high slice in `pie-sparkline`. |
| `start-angle` | Integer | Sets the start angle (in degrees) on `pie-sparkline`. Possible values are from 1 to 360. Default is 1. |
| `counter-clockwise` | Boolean | Sets the plot direction on `pie-sparkline`. Default is false, which represents clockwise. |

- The `Encoding` set is provided with the same properties as introduced for the `image` and `image-field` elements
- The `HTML` set of properties doesn't provide the `append-header` property
- The `Excel` set of properties only provides the `formula` property

## Band

This section is about the `band` element. This element contains exactly the same set of common properties as introduced earlier in the chapter, except for some differences, described as follows:

- The `object` set of properties contains only `anti-alias` and `keep-aspect-ratio`
- The `excel` set of properties does not provide `formula-override`
- The `page-behaviour` set of properties provides the `pagebreak-before` and `pagebreak-after` properties also
- The `Table` set is provided with the same properties as introduced for the `Page Header` and `Page footer` elements
- The `HTML` set of properties doesn't provide the `append-header` property
- The `Excel` set of properties only provides the `formula` property

## Sub-report

This section is about the `sub-report` element. This element contains exactly the same set of common properties as introduced earlier in the chapter, except for some differences, described as follows:

- The `excel` set of properties does not provide `formula-override`.
- The `page-behaviour` set of properties provides the `pagebreak-before` and `pagebreak-after` properties also.
- The `Query` set is provided here with some properties as defined for the `Master Report` element.
- The `Common` properties in attributes has the `sub-report-active` Boolean property. The property contains the flag to *switch-on* or *switch-off* the sub-report.
- The `HTML` set of properties doesn't provide the `append-header` property.
- The `Excel` set of properties only provides the `formula` property.

## Table-of-content and index

This section is about the `table-of-content` and `index` elements. These elements contain exactly the same set of common properties as introduced earlier in the chapter, except for some differences, described as follows:

- The `excel` set of properties does not provide `formula-override`.
- The `query` set is provided here with the same properties as defined for the `Master Report` element.
- The `common` properties in attributes has the `sub-report-active` Boolean property. The property contains the flag to *switch-on* or *switch-off* the sub-report containing the element.
- `Table-of-contents` in the `Attributes` tab set is introduced here for `Table-of-content` only, with the following properties:

| Name | Type | Description |
|---|---|---|
| `group-fields` | String | Defines both the depth of the data-collection and the fields from where to read the `group-value-x` values. If the `group-field` given in the array is empty, the field value will be read from the current relational group, and in the `details-processing`, the value will be null. If the `group-fields` list is empty, an automatic mode is activated that collects all the groups, extracting `group-value` from the relational group. |
| `title-field` | String | Defines a field in the master-report that will be read for a valid item-title. |
| `title-formula` | String | Defines a formula that is evaluated when a new item has been collected. The formula will only be evaluated if `title-field` is not set. |
| `collect-details` | Boolean | Defines whether detail items should be included in the data-collection. |
| `index-separator` | String | Defines the separator text that is used between the index-elements. It defaults to the dot character (.). |

- `Index` in the `Attributes` tab set is introduced here for `Index` only, with the following properties:

| Name | Type | Description |
|------|------|-------------|
| `data-field` | String | Defines the field to be used as `item-data` or `item-key`. |
| `data-formula` | String | Defines an open formula to be used as `item-data` or `item-key`. |
| `condensed-style` | Boolean | Defines whether - is used between continuous page numbers; for example, 1,2,3,4 would display as 1-4. |
| `index-separator` | String | Defines the separator text that is used between page numbers in the item-pages field in the index sub report. It defaults to the dot character (.). |

- The `HTML` set of properties doesn't provide the `append-header` property.
- The `Excel` set of properties only provides the `formula` property.

## Hiding and showing bands

In the previous sections, you learnt much about the report's structure through the report explorer and its hierarchical definition, from the root called Master Report to the first level represented by the bands, and the sub-elements defining the visible content of the report. This section describes the possibility of hiding (or showing) some elements, to customize the final report's layout.

As an example, you can focus your attention to the structure shown in report explorer and the visible bands (and content) in the report canvas. As introduced earlier in the chapter, by default, the report canvas shows the Page Header, Report Header, Details, Report Footer, and Page Footer bands. But taking a look at the report's structure, you can also find the Group element with nested Group Header, Details Header, Details Footer, and Group Footer bands. As you can easily imagine, the last set of bands come hidden by default.

To show the hidden elements (usually bands) or hide the visible ones, right click on the entity in the report element and uncheck (or check) the Hide Element item, as shown in the following image:

![](img/9337ac94537bf7e092e1ea6531dce8fd_271_0.png)

Acting on the Hide Element item, the corresponding band will be shown (or hidden) in the report canvas accordingly.

![](img/9337ac94537bf7e092e1ea6531dce8fd_271_1.png)

*This behavior of hiding/showing bands is useful for customizing the final report's layout, and is also useful for managing the grouping of results. Grouping the results of a report in a sort of master-detail layout is a powerful feature that you saw an example in chapter 2, Gett*
