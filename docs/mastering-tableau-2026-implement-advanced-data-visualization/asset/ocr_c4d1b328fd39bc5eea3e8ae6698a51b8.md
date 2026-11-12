# Extrapolating data

This exercise will expand on the previous exercise by cleaning existing data and populating some of the missing data from known information. We will assume that we know which country belongs to which region. We'll use that knowledge to fix errors in the **Region** field and also to fill in the gaps using Tableau.

Starting from where the previous exercise ended, create a calculated field entitled **Region Extrapolated** with the following code block:

```
CASE [Country]
WHEN 'Afghanistan' THEN 'Southern Asia'
WHEN 'Albania' THEN 'Central and Eastern Europe'
WHEN 'Algeria' THEN 'Middle East and Northern Africa'
WHEN 'Angola' THEN 'Sub-Saharan Africa'
WHEN 'Argentina' THEN 'Latin America and Caribbean'
WHEN 'Armenia' THEN 'Central and Eastern Europe'
WHEN 'Australia' THEN 'Australia and New Zealand'
WHEN 'Austria' THEN 'Western Europe'
//complete the case statement with the remaining fields in the data set
END
```

To speed up the tedious creation of a long calculated field, you could download the data to an Excel file and create the calculated field by concatenating the separate parts, as shown here:

![](bbox=[0.16, 0.51, 0.89, 0.78])

Figure 2.6: Compiling a calculation in Excel