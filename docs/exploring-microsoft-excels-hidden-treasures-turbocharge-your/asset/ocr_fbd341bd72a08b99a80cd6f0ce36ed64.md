I offer this as a simple example of how repetitive calculations such as (C13-D13) / D13, which appears twice in cell E13, can creep into our formulas. IFERROR was added to Excel to eliminate repetitive calculations when handling formula errors, but there are countless ways that we can find ourselves repeating a chunk of a formula more than once. It’s always tricky trying to make the same edit in multiple places in a formula, if you miss just _one_ spot, then you can have an error in your formula that could affect other cells as well. This brings us back to the LET function.

# ## Eliminating repetitive calculations

Cell E15 of the Formula Errors worksheet contains the =LET(calc, (C15-D15) / D15, IF(ISERROR(calc), "n/a", calc)) formula and returns 11% because (C15-D15) / D15 does not evaluate to an error. Let’s break the formula down:

+   - Name1: This argument establishes a calc Variable that we can use later in the formula.

+   - name_value1: This argument contains the repetitive part of our calculation, (C15 - D15) / D15, which we will be able to refer to by the calc Variable rather than repeating the calculation twice.

+   - Name2_or_calculation: Since we only need one Variable, the third argument contains the IF(ISERROR(calc), "n/a", calc) calculation. Now, (C15-D15) / D15 only appears once in the formula but is used twice. This means that if we need to edit the formula, we only need to do so in one place.

If you’re following along in column B, you can copy the formula you entered in cell B15 down to cell B16 without making any edits.

Tip

The LET function will return #NAME? in Excel 2019 and earlier. In Chapter 9, Excel Quirks and Nuances, I explained how you can use Compatibility Checker to determine if a worksheet utilizes any worksheet functions or features that are not backward compatible with older versions of Excel.

Now, let’s create a third calculation using LET so that I can elaborate further on how Variables work.

# ## Variables restrictions

Let’s return to the LET function worksheet, as shown Figure 11.15, so that I can show you how to establish Variables in the LET function without using them in your calculation: