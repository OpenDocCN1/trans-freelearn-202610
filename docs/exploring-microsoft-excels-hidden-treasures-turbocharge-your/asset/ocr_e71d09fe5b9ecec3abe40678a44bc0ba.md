Now that you can dynamically sort data with a formula, let’s up our game by seeing how to filter a list by using the FILTER function.

# # The FILTER function

The =FILTER(B3:E12,B3:B12=H2) formula in cell G6 of Figure 10.17 displays all the beaches for the name entered in cell H2, in this case, Ninety Mile Beach. Earlier in the chapter, you saw that functions such as VLOOKUP, MATCH, and XLOOKUP stop looking after finding an initial match. The FILTER function gives you a way to return multiple values from a list:

![](bbox=[0.1328, 0.2906, 0.8525, 0.4984])

Figure 10.17 – The FILTER function

> > **Tip**
> > It’s best not to embed criteria within the formula itself. Using input cells such as cell H2 enables you or other users to change the criteria without having to edit the formula.

The FILTER function has two required arguments and one optional argument:

+   • Array – This required argument is a list composed of one or more columns that you wish to filter, which, in this case, is B3:E12.

+   • Include – This required argument is the cell coordinates of the column that you wish to filter on and the criteria itself, which, in this case, is B3:B12=H2. You can only provide one criterion per column, but you can filter on multiple columns.

+   • If_empty – An alternate message to display, which I omitted in this case but will be illustrated later.