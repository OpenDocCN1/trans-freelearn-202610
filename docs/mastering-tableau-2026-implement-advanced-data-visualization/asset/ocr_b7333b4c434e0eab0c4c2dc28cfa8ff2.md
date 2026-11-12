**City Count:** The IF FIRST() = 0 THEN LOOKUP([Size],0) ELSE NULL END field is not strictly necessary. You could, instead, simply place **Size** on the **Text/Label** shelf. However, if you do so, you will note that the numbers in the view will look bold. This occurs because the numbers are actually repeated and then layered on top of each other. Utilizing FIRST() = 0 causes only one set of numbers to display.

Note that the preceding exercise had an option for **Nested Calculations**, which is because the Size calculated field was referenced within the City Count calculated field.

Real-world scenario: **Size** is probably the least used. I honestly do not think I have ever used it other than for explaining and showcasing it. But what I have seen others do with **Size** is the following: by turning on the **grand total** column view, **Size** can generate a non-default aggregation, such as standard deviation. Simply use this formula: if size()=1 then sum({STDEV([Sales])}) else sum([Sales]) end.

If you cannot get enough of table calculation, here are some more scenarios:

https://www.tableau.com/blog/top-10-tableau-table-calculations

Now that we have presented the different table calculations, we will see how they can be manipulated in the partitioning (scope) and addressing (direction) of calculations.

# Application of functions

So far, we have covered the first of our two major questions: _What is the function?_ Now we will proceed to the next question: _How is the function applied?_

Let's try to understand that question via the following three options, which are all applications of the INDEX function:

| Category | Consumer | | Corporate | | Home Office | | | Category | Consumer | Corporate | Home Office |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Furniture | | 1 | | 2 | | 3 | | Furniture | | 1 | | 1 | | 1 |
| Office Supply | | 1 | | 2 | | 3 | | Office Supply | | 2 | | 2 | | 2 |
| Technology | | 1 | | 2 | | 3 | | Technology | | 3 | | 3 | | 3 |
| | | | | Category | Consumer | Corporate | Home Office |
| | | | | Furniture | | 1 | | 2 | | 3 |
| | | | | Office Supply | | 4 | | 5 | | 6 |
| | | | | Technology | | 7 | | 8 | | 9 |

Figure 5.15: Applications

The INDEX function is used in each of these three screenshots; however, it is applied differently in each. The first and second screenshots both display **1, 2, and 3**, but differ directionally. The third screenshot ranges from **1 to 9**. So, how is INDEX being applied in each case?