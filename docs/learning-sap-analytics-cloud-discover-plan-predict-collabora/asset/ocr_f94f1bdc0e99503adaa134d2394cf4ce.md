|  | STORE | DATE | Actuals | Budget | Forecast |
|---|---|---|---|---|---|
|  |  | VERSION | Actuals | Budget | Forecast |
|  |  | VERSION | Actual | Budget | Forecast |
|  | STORE | DATE |  |  |  |
| 5 | InterMart | ▶ (all) | 5,200.00 | 6,500.00 | 6,600.00 |
| 6 | Frills | ▶ (all) | 3,300.00 | 3,300.00 | 3,400.00 |
| 7 | Kullens | ▶ (all) | 5,700.00 | 5,000.00 | 5,000.00 |
| 8 | Farlos | ▼ (all) | 3,300.00 | 3,700.00 | 4,000.00 |
| 9 |  | ▼ 2017 | 3,300.00 | 3,700.00 | 4,000.00 |
| 10 |  | ▼ Q1 (2017) | 3,300.00 | 3,700.00 | 4,000.00 |
| 11 |  | Jan (2017) | 600.00 | 900.00 | 864.80 |
| 12 |  | Feb (2017) | 1,300.00 | 1,000.00 | 1,513.60 |
| 13 |  | Mar (2017) | 1,400.00 | 1,800.00 | 1,621.60 |
| 14 | Hudsons | ▶ (all) | 4,200.00 | 4,300.00 | 4,300.00 |

In addition to spreading values automatically, you can also manually specify weights for members of a specific dimension and level by selecting the source cell and then going to Allocate values (↕) | Spreading from the toolbar.

In the Spreading dialog, you need to specify Target Dimension to spread the chosen value on. In the current scenario, the Date dimension will be set as Target Dimension. Using the icons to the right of this dialog, you can choose the hierarchical level at which you want to spread the chosen value. The Unbooked option allows you to spread to members that do not have values.

![](bbox=[0.08, 0.5, 0.16, 0.55])

> *Note that if you do not choose the bottom level of the hierarchy as your target, the values will automatically be spread to leaf cells using the existing weights if these cells are already populated. If you have turned on the Unbooked option and the cells are empty, equal values will be spread to all leaf cells.*

There are a couple of methods using which you can spread the values. You can enter numeric weight values, such as simple ratios between the members, or click on the eyedropper icon to copy values from other cells in the story sequentially into the available fields. When you enter values manually, it is the relative weights of the values that are spread, not the values themselves.

This feature is useful when we want to apply the same weighting between members. The Preview column shows the values that will appear in the table after spreading. Once you are satisfied with the statistics in this dialog, click on the Apply Spreading button to spread the values: