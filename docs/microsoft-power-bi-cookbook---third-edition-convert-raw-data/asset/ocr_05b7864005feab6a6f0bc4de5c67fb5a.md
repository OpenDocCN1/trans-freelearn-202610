As a review of the concepts covered thus far and for future reference, Table 2.2 presents a glossary of the main concepts of the M language utilized in this book.

| Concept | Definition |
|---|---|
| Expression | Formulas evaluated to yield a single value. Expressions can reference other values, such as functions, and may include operators. |
| Value | The result of evaluating an expression. Values can be categorized into types that are either primitive, such as text ("abc"), or structured kinds, such as tables and lists. |
| Function | A value that produces a new one, based on the mapping of input values to the parameters of the function. Functions can be invoked by passing parameter values. |
| Type | A value that classifies other values. The structure and behavior of values are restricted based on the classification of their type, such as Record, List, or Table. |
| let | An expression that allows a set of unique expressions to be assigned names (variables) and evaluated (if necessary), when evaluating the expression following the in expression in a let...in construct. |
| Variable | A unique, named expression within an environment to be conditionally evaluated. Variables are represented as APPLIED STEPS in Query Editor. |
| Environment | A set of variables to be evaluated. The global environment containing the M library is exposed to root expressions. |
| Evaluation | The computation of expressions. Lazy evaluation is applied to expressions defined within let expressions; evaluation occurs only if needed. |
| Operators | A set of symbols used in expressions to define the computation. The evaluation of operators depends on the values to be operated on. |

Table 2.2: M language elements

M queries are not intended as a substitute for the data loading and transformation workloads typically handled by enterprise data integration and orchestration tools, such as Azure Data Factory (ADF), Azure Databricks, or SQL Server Integration Services (SSIS). However, just as BI professionals carefully review the logic and test the performance of SQL stored procedures and Extract Transform Load (ETL) packages supporting their various cubes and reporting environments, they should also review the M queries created to support Power BI models and reports. When developing retrieval processes for Power BI models, consider these common ETL questions:

+   - How do queries impact the source systems?
+   - Can we make our queries more resilient to changes in source data so that they avoid failure?
+   - Are our queries efficient and simple to follow and support, or are there unnecessary steps and queries?
+   - Are our queries delivering sufficient performance to the BI application?
+   - Is our process flexible, such that we can quickly apply changes to data sources and logic?
+   - Can some or all of the required transformation logic be implemented in a source system, such as the loading process for a data warehouse table or a SQL view?