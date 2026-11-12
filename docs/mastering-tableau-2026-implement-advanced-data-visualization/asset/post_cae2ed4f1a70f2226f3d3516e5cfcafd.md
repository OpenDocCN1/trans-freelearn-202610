
- 4. Create two parameters. Name one Overall Pixel Count and the other Rows Down/Columns Across. The settings for both are as follows:
    - Data type: Integer
    - Allowable values: All

- 5. Show both parameters. Set Rows Down/Columns Across to 10 and Overall Pixel Count to 1,000:

![](img/66cf6b89349fda3581a4f196cb3c98a4_343_0.png)

Figure 8.22: Showing the parameter

- 6. Create a calculated field named Concatenate Header with the following code:

```
'<?xml version="1.0" encoding="utf-8"?>&lt;svg version="1.1" id="Squares"
xmlns="http://www.w3.org/2000/svg"
xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 ' +
STR( [Overall Pixel Count] ) + " " + STR([Overall Pixel Count])+ " "
style="enable-background:new 0 0 ' + STR([Overall Pixel Count]) + ' ' +
STR([Overall Pixel Count]) + '; " xml:space="preserve">
<style type="text/css"> .st0{fill:none;stroke:#000000;stroke-miterlimit:10;}
</style>'
```

Note that entering line breaks in the Calculated Field dialog box may make the results difficult to extract from Tableau. In other words, remove all line breaks.

- 7. Now we have created a skeleton or template that will help us create multiple locations in order to draw a grid in Tableau

- 8. Place the newly created calculated field on the Text shelf

- 9. In the toolbar, choose to fit to Entire View to view the results; you can see that the parameters fill in STR([Overall Pixel Count]) from Concatenate Header

![](img/66cf6b89349fda3581a4f196cb3c98a4_344_0.png)

```
<?xml version="1.0" encoding="utf-8"?><svg version="1.1" id="Squares" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 1000 1000" style="enable-background:new 0 0 1000 1000;" xml:space="preserve"> <style type="text/css"> .st0{fill:none;stroke:#000000;stroke-miterlimit:10;}</style>
```

Figure 8.23: Concatenate Header

- 10. Create a new worksheet named Location Codes

- 11. Create the following calculated fields:

| Name | Code |
| :--- | :--- |
| Rows Down/Columns Across | [Parameters].[Rows Down/Columns Across] |
| Which Column? | LAST()+1 |
| Which Row? | INDEX() |
| Grid Size | [Overall Pixel Count]/LOOKUP([Which Column?],FIRST()) |
| X | [Overall Pixel Count] - ([Grid Size] * ([Which Row?])) |
| Y | [Overall Pixel Count] - ([Grid Size] * ([Which Column?]-1)) |
| Count | If [Records] = 1 THEN 1 ELSE [Rows Down/Columns Across] END |
| Decicount | If [Records] = 1 THEN 1 ELSE [Rows Down/Columns Across] END |
| Location Codes | Index() |
| Concatenate Locations | '<text transform="matrix(1 0 0 1 ' + STR([X]) + ' ' + STR([Y]) + ')">' + STR( [Location Codes] ) + '</text>' |

Table 8.2: Creating calculated fields

- 12. The created calculated fields will be used in the next steps to create a table of values, similar to the table of values that we generated in the drawing tool. Only now, we will do it ourselves and use SVG code instead of longitude and latitude values.

- 13. Right-click on Count and select Create | Bins. In the resulting dialog box, set Size of bins to 1

![](img/66cf6b89349fda3581a4f196cb3c98a4_345_0.png)

Figure 8.24: Count bin

- 14. Right-click on Decicount and select Create | Bins. In the resulting dialog box, set Size of bins to 1.

- 15. Place Count (bin), Decicount (bin), Location Codes, X, and Y on the Rows shelf. Be sure to place those fields in the order listed.

- 16. If your fields are green (meaning continuous), right-click on each field on the Rows shelf and set it to Discrete. Then:
    - Right-click on Count (bin) and Decicount (bin) and ensure that Show Missing Values is selected
    - Right-click on Location Codes and select Compute Using | Table (Down)
    - Set the Compute Using value for X to Count (bin)

- 17. Set the Compute Using value for Y to Decicount (bin)

| Count (bin) | Decicount (bin) | Location Codes | X | Y |
|---|---|---|---|---|
| 1 | 1 | 900 | 100 | Abc |
| 2 | 2 | 900 | 200 | Abc |
| 3 | 3 | 900 | 300 | Abc |
| 4 | 4 | 900 | 400 | Abc |
| 5 | 5 | 900 | 500 | Abc |
| 6 | 6 | 900 | 600 | Abc |
| 7 | 7 | 900 | 700 | Abc |
| 8 | 8 | 900 | 800 | Abc |
| 9 | 9 | 900 | 900 | Abc |
| 10 | 10 | 900 | 1,000 | Abc |
| 1 | 11 | 800 | 100 | Abc |
| 2 | 12 | 800 | 200 | Abc |
| 3 | 13 | 800 | 300 | Abc |
| 4 | 14 | 800 | 400 | Abc |
| 5 | 15 | 800 | 500 | Abc |
| 6 | 16 | 800 | 600 | Abc |
| 7 | 17 | 800 | 700 | Abc |
| 8 | 18 | 800 | 800 | Abc |

Figure 8.25: Location Codes worksheet

- 18. Place Concatenate Locations on the Text shelf

- 19. Right-click on the instance of Concatenate Locations you just placed on the Text shelf and select Edit Table Calculations

- 20. At the top of the resulting dialog box, note that there are four options under Nested Calculations: Grid Size, Which Column?, Which Row?, and Location Codes. Set the Compute Using definition for each as follows:
    - Nested Calculations: Grid Size > Compute Using: Table (down)
    - Nested Calculations: Which Column? > Compute Using: Specific Dimensions: Decicount (bin) > move this dimension to the top
    - Nested Calculations: Which Row? > Compute Using: Specific Dimensions: Count (bin) > move this dimension to the top
    - Nested Calculations: Location Codes > Compute Using: Table (down)

- 21. In the toolbar, choose Fit Width to view the results; you can already see that multiple rows have been created. Those rows will later be used to draw a grid:

| Count (bin) | Decicount (bin) | Location Codes | X | Y | XML |
|---|---|---|---|---|---|
| 1 | 1 | 900 | 100 | <text transform="matrix(1 0 0 1 900 100)">1</text> |
| | 2 | 900 | 200 | <text transform="matrix(1 0 0 1 900 200)">2</text> |
| | 3 | 900 | 300 | <text transform="matrix(1 0 0 1 900 300)">3</text> |
| | 4 | 900 | 400 | <text transform="matrix(1 0 0 1 900 400)">4</text> |
| | 5 | 900 | 500 | <text transform="matrix(1 0 0 1 900 500)">5</text> |
| | 6 | 900 | 600 | <text transform="matrix(1 0 0 1 900 600)">6</text> |
| | 7 | 900 | 700 | <text transform="matrix(1 0 0 1 900 700)">7</text> |
| | 8 | 900 | 800 | <text transform="matrix(1 0 0 1 900 800)">8</text> |
| | 9 | 900 | 900 | <text transform="matrix(1 0 0 1 900 900)">9</text> |
| | 10 | 900 | 1,000 | <text transform="matrix(1 0 0 1 900 1000)">10</text> |
| 2 | 11 | 800 | 100 | <text transform="matrix(1 0 0 1 800 100)">11</text> |
| | 12 | 800 | 200 | <text transform="matrix(1 0 0 1 800 200)">12</text> |
| | 13 | 800 | 300 | <text transform="matrix(1 0 0 1 800 300)">13</text> |
| | 14 | 800 | 400 | <text transform="matrix(1 0 0 1 800 400)">14</text> |
| | 15 | 800 | 500 | <text transform="matrix(1 0 0 1 800 500)">15</text> |
| | 16 | 800 | 600 | <text transform="matrix(1 0 0 1 800 600)">16</text> |
| | 17 | 800 | 700 | <text transform="matrix(1 0 0 1 800 700)">17</text> |
| | 18 | 800 | 800 | <text transform="matrix(1 0 0 1 800 800)">18</text> |
| | 19 | 800 | 900 | <text transform="matrix(1 0 0 1 800 900)">19</text> |
| | 20 | 800 | 1,000 | <text transform="matrix(1 0 0 1 800 1000)">20</text> |
| 3 | 21 | 700 | 100 | <text transform="matrix(1 0 0 1 700 100)">21</text> |
| | 22 | 700 | 200 | <text transform="matrix(1 0 0 1 700 200)">22</text> |
| | 23 | 700 | 300 | <text transform="matrix(1 0 0 1 700 300)">23</text> |

Figure 8.26: Fit width

- 22. Now, create a new worksheet named Lines

- 23. Create the following calculated fields:

| Name | Compute Using Setting |
|---|---|
| H Then V | Index() |
| HLine | Last() |
| VLine | Index()-1 |
| X1 | IF [H Then V] = 1 THEN 0 ELSE [Overall Pixel Count] - ((Grid Size) * ([VLine])) END |
| Y1 | IF [H Then V] = 2 THEN 0 ELSE [Overall Pixel Count] - ((Grid Size) * ([VLine])) END |
| X2 | IF [H Then V] = 1 THEN [Overall Pixel Count] ELSE [Overall Pixel Count] - ((Grid Size) * ([VLine])) END |
| Y2 | IF [H Then V] = 2 THEN [Overall Pixel Count] ELSE [Overall Pixel Count] - ((Grid Size) * ([VLine])) END |

Table 8.3: Creating calculated fields

- 24. Next, place the following fields on the Rows shelf in the following order: Count (bin), Decicount (bin), H Then V, HLine, VLine, Grid Size, Which Column?, X1, Y1, X2, and Y2. Note that each should be cast as discrete.

- 25. Right-click on Count (bin) and Decicount (bin) and set each to Show Missing Values

- 26. Right-click on each of the remaining fields on the Rows shelf and select Edit Table Calculations. Set the Compute Using definition of each field as shown in the following table:

| Name | Nested Calculations | Compute Using Setting |
|---|---|---|
| H Then V | N/A | Count (bin) |
| HLine | N/A | Count (bin) |
| VLine | N/A | Decicount (bin) |
| Grid Size | Grid Size | Table (Down) |
| Grid Size | Which Column? | Decicount (bin) |
| X1, Y1, X2, Y2 | H Then V | Count (bin) |
| X1, Y1, X2, Y2 | Grid Size | Count (bin) |
| X1, Y1, X2, Y2 | Which Column? | Count (bin) |
| X1, Y1, X2, Y2 | VLine | Decicount (bin) |

Table 8.4: Compute Using Settings

- 27. Note that some of the fields include nested table calculations. In such cases, the Table Calculation dialog box will include an extra option at the top entitled Nested Calculations
