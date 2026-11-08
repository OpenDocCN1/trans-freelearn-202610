# Table of contents functions

Table of content and index were introduced in Chapter 5, *Design and Layout in Report Designer* as a report element. Here are described the functions related to the same topic:

| Name | Description |
|------|-------------|
| `Index Text Generator` | It produces a text as a collection of strings defining a nice table of contents. The properties defined for this function are described in the following table: |
| `Index Number Generator` | It produces an integer array defining a nice table of contents. The properties defined for this function are the same as introduced earlier: `Collect Details` |

| Name | Type | Description |
|------|------|-------------|
| `CollectDetails` | Boolean | Defines whether detail items should be included in the data collection. |
| `indexSeparator` | String | Defines the separator text that is used between the index elements. It defaults to .. |
| `Depth` | Integer | Defines the number of levels of the table of content. |
| `Condensed Style` | Boolean | Define whether a - is used between continuous page numbers; for example, 1, 2, 3, 4 would display as 1-4. |