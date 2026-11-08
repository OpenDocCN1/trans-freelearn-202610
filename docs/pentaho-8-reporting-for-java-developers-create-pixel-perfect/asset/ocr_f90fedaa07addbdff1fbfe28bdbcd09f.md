```
// Create a formula expression.
FormulaExpression formula = new FormulaExpression();

// Configure the formulas properties.
formula.setName("totalCost");

formula.setFormula("=[Cost]*[Quantity]");

// Add the expression to the report.
report.addExpression(formula);
```