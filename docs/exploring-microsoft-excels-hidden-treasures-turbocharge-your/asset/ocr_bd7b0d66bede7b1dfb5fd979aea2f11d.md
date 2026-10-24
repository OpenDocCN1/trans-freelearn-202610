Now let’s walk through each tab of the **Data Validation** dialog box to understand the full range of options.

# ## Settings tab

The Allow list on the **Settings** tab of the **Data Validation** dialog box enables you to assign the following rules to one or more cells:

+   - **Any value**: No restrictions are placed – this is the default for all worksheet cells

+   - **Whole number**: Inputs are restricted to whole numbers only

+   - **Decimal**: Inputs are restricted to numbers with or without decimals

+   - **List**: Inputs are restricted to items from a specified list

+   - **Date**: Inputs are restricted to date values

+   - **Time**: Inputs are restricted to time values

+   - **Text length**: Inputs are limited based upon a specified number of characters

+   - **Custom**: Formulas that return `TRUE` or `0` (zero) indicate valid inputs, whereas `FALSE`, `1`, or an error such as `#VALUE!` indicates an invalid input

You can further limit each rule, except for **List** and **Custom**, with one of the following restrictions:

+   - **Between**: **Minimum** and **Maximum** fields allow you to specify a range of valid values.

+   - **Not between**: **Minimum** and **Maximum** or **Start** and **End** fields allow you to specify a range of invalid values.

+   - **Equal to**: A **Value** field enables you to specify text, value, date, or time that the user must enter.

+   - **Not equal to**: A **Value** field enables you to specify text, value, date, or time that the user cannot enter.

+   - **Greater than**: A **Minimum** or **Start** field allows you to specify the lower end of a range of values. The minimum or start value provided **_will not_** be a valid choice in the cell.

+   - **Less than**: A **Maximum** or **End** field allows you to specify the upper end of a range. The maximum or end value provided **_will not_** be a valid choice in the cell.

+   - **Greater than or equal to**: A **Minimum** or **Start** field allows you to specify the lower end of a range of values. The minimum or start value provided **_will_** be a valid choice in the cell.

+   - **Less than or equal to**: A **Maximum** or **End** field allows you to specify the upper end of a range. The maximum or end value provided **_will_** be a valid choice in the cell.