
| | Initialization Formula | String | If specified, this formula will be evaluated when the `Formula` function is called for the first time, instead of evaluating the default formula |

## Chart data and value data functions

Chart related functions have been covered in detail in a previous chapter, so there is no need to restate them in this list, where the properties and behavior are exactly the same. Value related functions contain mainly `Thermometer DataSet Collection`, already covered in a previous chapter.

## Image functions

As introduced for chart and value data functions, image related functions have been covered in detail in the previous chapter, so there is no need to restate them in this list, where the properties and behavior are exactly the same.

## Script functions

Scripting functions make it easy to customize your report through various scripting languages:

| Name | Description |
|------|-------------|
| `Bean-Scripting Framework (BSF)` | This function uses Apache's Bean Scripting Framework to generate a result. See http://jakarta.apache.org/bsf for more information on the Bean Scripting Framework. The properties defined for this function are described in the following table: |
| `BeanShell (BSH)` | This function uses the BeanShell framework to generate a result. See http://www.beanshell.org for more information on BeanShell. The properties defined for this function are the same as introduced earlier for `Expression`. |

| Name | Type | Description |
|------|------|-------------|
| `Expression` | String | An expression defined in the specified programming language |
| `Expression Programming Language` | String | The programming language used in the expression |
| `Initialization Script` | String | A script defined in the specified programming language, which is executed during the initialization of the scripting language environment |

## Table of contents functions

Table of content and index were introduced in Chapter 5, *Design and Layout in Report Designer* as a report element. Here are described the functions related to the same topic:

| Name | Description |
|------|-------------|
| `Index Text Generator` | It produces a text as a collection of strings defining a nice table of contents. The properties defined for this function are described in the following table: |
| `Index Number Generator` | It produces an integer array defining a nice table of contents. The properties defined for this function are the same as introduced earlier: `Collect Details` and Depth. |

| Name | Type | Description |
|------|------|-------------|
| `CollectDetails` | Boolean | Defines whether detail items should be included in the data collection. |
| `indexSeparator` | String | Defines the separator text that is used between the index elements. It defaults to .. |
| `Depth` | Integer | Defines the number of levels of the table of content. |
| `Condensed Style` | Boolean | Define whether a - is used between continuous page numbers; for example, 1, 2, 3, 4 would display as 1-4. |

| Name | Description |
|------|-------------|
| Index Generator | The index generator function has been covered in detail in Chapter 5, *Design and Layout in Report Designer*, so there is no need to restate it here, as the properties and behavior are exactly the same. |
| Table of Contents Generator | The table of content generator function has been covered in detail in Chapter 5, *Design and Layout in Report Designer*, so there is no need to restate it here, as the properties and behavior are exactly the same. |

## Environment variables

If you are publishing your Pentaho reports to the Business Analytics Platform (see Chapter 15, *Using Reports in Pentaho Business Analytics Platform* for an introduction), you can use certain environment variables to customize and control the behavior. The environment variables are available by default in a Pentaho report when it's created, and they live in the Data tab of the report explorer. The following screenshot shows how the report explorer looks with the environment variable expanded:

![](img/9337ac94537bf7e092e1ea6531dce8fd_455_0.png)

Environment variable can be used as usual, by dragging and dropping it into the right place of the report. The following is a list of the variables with brief descriptions of their meanings:

| Name | Description |
|------|-------------|
| `env:hostColonPort` | The hostname and port number for the Business Analytics Platform. |
| `env:locale` | Info about locale of the client requesting the report. `env:locale-language` and `env:locale-short` are other two variables available. |
| `env:pentahoBaseURL` | The complete URL to the Business Analytics Platform, as set in the base URL property. |
| `env:requestURL` | The complete URL to the request for the resource. |
| `env:roles` | Returns a comma-separated list of roles that the Business Analytics Platform user, who is currently running this report, belongs to. |
| `env:roles-array` | Returns a Java array of strings containing the roles that the Business Analytics Platform user, who is currently running this report, belongs to. |
| `env:serverBaseURL` | The URL to the Business Analytics Platform, minus the Business Analytics Platform application context name (the default context is /pentaho/). |
| `env:solutionRoot` | The path to the top-level Pentaho solution directory. |
| `env:username` | Returns the Business Analytics Platform username of the person currently running the published report. |

## Working with formulas

In addition to providing functions within reports, formulas may also be used to generate dynamic content in a report. Formulas may be used to derive element property and style values. Also, the `Open Formula` and `Open Formula (Advanced)` functions defined earlier may be used to combine the formula and function mechanisms in a report.

Formulas in Pentaho Reporting are based on the `Open Formula` standard. This standard is similar to Excel formula support, and is used in OpenOffice as well as other tools such as Pentaho Metadata. This formula system is often referred to as `LibFormula`, which is the library name for reporting's formula subproject.

## Creating a formula

As you learnt in the section about functions and expressions, there are two places where Open Formula and Open Formula (Advanced) can be used: in the Data tab of report explorer and in each property expression in the element properties panel. Once Open Formula or Open Formula (Advanced) is requested, the formula editor is shown accessing the Formula/Initialization formula property.

![](img/9337ac94537bf7e092e1ea6531dce8fd_458_0.png)

As you can see from the preceding screenshot, the formula editor is very intuitive. Looking at the window, you can recognize: on the left, the list of available functions with a category selector on top; on the right, the helping panel to support the composition of the formula with the formula text area; and at the bottom, a description as reference.

![](img/9337ac94537bf7e092e1ea6531dce8fd_458_1.png)

*Use the field selection ( icon ) to use an available element in the formula. With fields, the formula editor defines the fields of the data sources, the custom functions, and the environment variables. As you will see, using this feature, Pentaho Reporting identifies a field using the syntax [NAME].*

Take your time to play a bit with the formula editor. You will see that its use is straightforward. As an example, you can try to develop the formula to show the date of execution of the report in the `Page Footer` band (as a suggestion, try to check the `now()` function).

```
IF([COL1] > 10; "Big"; "Small");
```

This formula uses the IF function with parameters separated by semicolons. The first parameter is a comparison, resulting in true or false. To reference an outside data column or named function, reference the column in brackets, as shown for COL1. If the comparison example evaluates to true, the second parameter in the IF function is returned. To specify a string, use double quotes, shown with "Big" and "Small". If the statement evaluates to false, the third parameter is returned. Also, the term null may be used to denote a null reference.
