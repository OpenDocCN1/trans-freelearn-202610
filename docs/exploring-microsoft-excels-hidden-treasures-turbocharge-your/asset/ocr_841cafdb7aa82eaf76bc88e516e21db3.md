![](bbox=[0.133, 0.102, 0.858, 0.317])

# Figure 10.4 – The INDEX/MATCH functions

The INDEX function has three arguments:

+   • **Array** – A range of cells in a row or column, or an array of data; in this case, it is D3 : D12.

+   • **Row_num** – The row number or the range of cells to return data from, which can be provided
+   by the MATCH function; in this case, it is MATCH( $N3 , $B3 : $B12 , 0) .

+   • **Column_num** – Optionally, you can specify a column position within the array, which I
+   omitted in this case.

The INDEX/MATCH function eliminates a lot of frustration that VLOOKUP causes because the combination
can return information from the left-hand side or the right-hand side of the **Lookup** column, or even
perform a two-way lookup that looks down a row and across a column to search for information based upon
two criteria. The = INDEX( $B3 : $E12 , MATCH( $N9 , $B3 : $B12 , 0) , MATCH( O$8 , $B2 : $E2 , 0) )
formula in cell **O9** of Figure 10.4 shows you how to perform a double match. The first MATCH function
identifies which row **Stockton Beach** appears in, and the second MATCH function returns the column
that the word **Miles** appears in. As written, this formula can be copied to cell **P9** to look up the length
in kilometers, too.

A limitation of both VLOOKUP and INDEX/MATCH is that they can only match a single piece of data.
SUMIF enables you to search for one or more instances of criteria and then add up the corresponding
values.