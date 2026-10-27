The **Custom Summary Formula** is not the same as a custom formula field. You can't retrieve a value from another object using a **Custom Summary Formula**, and a **Custom Summary Formula** only lives in that report; it does not affect another report, object, or field, and you can't use it from other reports.

A **Custom Summary Formula** is not available in the **Tabular** report and when you change the report type with the summary formula, the existing summary formula will be deleted, so be aware of this.

There are many functions available in **Custom Summary Formulas**; the value will be returned as a number, currency, or percentage only, and you can configure the decimal place returned. The formula accepts a limited type of field only, but this includes the formula field and the roll-up summary field.

The `PARENTGROUPVAL()` and `PREVGROUPVAL()` functions are available only in **Custom Summary Formulas**. You can't find them in another formula in Salesforce; they are very powerful functions. You can use the `PARENTGROUPVAL()` function to retrieve the group value from the parent group hierarchy and use the `PREVGROUPVAL()` function to retrieve the value from the previous group in the same level of the hierarchy.

In the next chapter, we will discuss more features related to reporting, adding a chart to a report, and embedding a chart from a report into the parent record page.